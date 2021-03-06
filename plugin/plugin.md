
 Package plugin

    import "plugin"

    Overview
    Index

Overview ▾

Package plugin implements loading and symbol resolution of Go plugins.

Currently plugins only work on Linux.

A plugin is a Go main package with exported functions and variables that has been built with:

go build -buildmode=plugin

When a plugin is first opened, the init functions of all packages not already part of the program are called. The main function is not run. A plugin is only initialized once, and cannot be closed.
Index ▾

    type Plugin
        func Open(path string) (*Plugin, error)
        func (p *Plugin) Lookup(symName string) (Symbol, error)
    type Symbol

Package files

plugin.go plugin_dlopen.go
type Plugin

Plugin is a loaded Go plugin.

type Plugin struct {
        // contains filtered or unexported fields
}

func Open

func Open(path string) (*Plugin, error)

Open opens a Go plugin. If a path has already been opened, then the existing *Plugin is returned. It is safe for concurrent use by multiple goroutines.
func (*Plugin) Lookup

func (p *Plugin) Lookup(symName string) (Symbol, error)

Lookup searches for a symbol named symName in plugin p. A symbol is any exported variable or function. It reports an error if the symbol is not found. It is safe for concurrent use by multiple goroutines.
type Symbol

A Symbol is a pointer to a variable or function.

For example, a plugin defined as

package main

// // No C code needed.
import "C"

import "fmt"

var V int

func F() { fmt.Printf("Hello, number %d\n", V) }

may be loaded with the Open function and then the exported package symbols V and F can be accessed

p, err := plugin.Open("plugin_name.so")
if err != nil {
	panic(err)
}
v, err := p.Lookup("V")
if err != nil {
	panic(err)
}
f, err := p.Lookup("F")
if err != nil {
	panic(err)
}
*v.(*int) = 7
f.(func())() // prints "Hello, number 7"

type Symbol interface{}
