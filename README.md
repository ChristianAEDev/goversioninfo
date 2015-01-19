goversioninfo
==========
[![Build Status](https://travis-ci.org/josephspurrier/goversioninfo.svg)](https://travis-ci.org/josephspurrier/goversioninfo) [![Coverage Status](https://coveralls.io/repos/josephspurrier/goversioninfo/badge.png)](https://coveralls.io/r/josephspurrier/goversioninfo) [![GoDoc](https://godoc.org/github.com/josephspurrier/goversioninfo?status.svg)](https://godoc.org/github.com/josephspurrier/goversioninfo)

Golang Microsoft Version Info and Icon Resource Generator

Package creates syso files with Version Info and Icon that the go build command will pick up and embed in your application. Fill in the versioninfo.json file and then use the code below to generate a .syso file. When you build, Go with automatically find the file if is is in the same directory as your file with the main() function. Complete documentation available on [GoDoc](https://godoc.org/github.com/josephspurrier/goversioninfo). Thanks to [Mateusz Czaplinski](https://github.com/akavel/rsrc) for his embedded binary resource package.

## Using the Library

Make a copy of versioninfo.json and copy it into your working directory. Fill in all the values and then use the code below.

```
package main

import (
	"fmt"
	"github.com/josephspurrier/goversioninfo"
	"io/ioutil"
)

func main() {
	// Read the config file
	jsonBytes, err := ioutil.ReadFile("versioninfo.json")
	if err != nil {
		fmt.Println("File Error:", err)
		return
	}

	// Create a new container
	vi := &goversioninfo.VersionInfo{}

	// Parse the config
	if ok := vi.ParseJSON(jsonBytes); ok {
		// Fill the structures with config data
		vi.Build()

		// Write the data to a buffer
		vi.Walk()

		// Create the file
		vi.WriteSyso("resource.syso")
	} else {
		fmt.Println("Could not parse the .json file")
	}
}
```

## Add an icon

To add an icon as a resource in the syso file, set Icon to true and set IconPath to the name of the icon to embed. If the icon has multiple sizes, all of the sizes will be embedded.

```
// Enable icon embeddeding
vi.Icon = true

// Path to icon (same directory)
vi.IconPath = "icon.ico"
```