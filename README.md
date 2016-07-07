# Store
>Store is a dead simple configuration manager for Go applications.

[![GoDoc](https://godoc.org/github.com/tucnak/store?status.svg)](https://godoc.org/github.com/tucnak/store)

I didn't like existing configuration management solutions like [globalconf](https://github.com/rakyll/globalconf), [tachyon](https://github.com/vektra/tachyon) or [viper](https://github.com/spf13/viper). First two just don't feel right and viper, imo, a little overcomplicated—definitely offering too much for small things. Store supports either JSON, TOML or YAML out-of-the-box and lets you register practically any other configuration format. It persists all of your configurations in either $XDG_CONFIG_HOME or $HOME on Linux and in %APPDATA%
on Windows.

Look, when I say it's dead simple, I actually mean it:
```go
package main

import (
	"github.com/tucnak/store"
	"log"
	"os"
)

func init() {
	// You must init store with some truly unique path first!
	store.Init("cats-n-dogs/project-hotel")
}

type Cat struct {
	Name    string `toml:"naym"`
	Clever  bool   `toml:"ayy"`
}

type Hotel struct {
	Name string
	Cats []Cat `toml:"guests"`

	Opens  *time.Time
	Closes *time.Time
}

func main() {
	var hotel Hotel
	err := store.Load("hotel.toml", &settings)
	if err != nil {
		log.Println("failed to load the cat hotel:", err)
		return
	}

	// ...

	err := store.Save("hotel.toml", &settings)
	if err != nil {
		log.Println("failed to save the cat hotel:", err)
		return
	}
}
```

Store supports any other formats via the handy registration system: register the format once and you'd be able to Load and Save files in it afterwards:
```go
store.Register("ini", ini.Marshal, ini.Unmarshal)

err := store.Load("configuration.ini", &object)
// ...
```
