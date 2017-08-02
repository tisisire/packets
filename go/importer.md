
 Package importer

    import "go/importer"

    Overview
    Index

Overview ▾

Package importer provides access to export data importers.
Index ▾

    func Default() types.Importer
    func For(compiler string, lookup Lookup) types.Importer
    type Lookup
    Bugs

Package files

importer.go
func Default

func Default() types.Importer

Default returns an Importer for the compiler that built the running binary. If available, the result implements types.ImporterFrom.
func For

func For(compiler string, lookup Lookup) types.Importer

For returns an Importer for the given compiler and lookup interface, or nil. Supported compilers are "gc", and "gccgo". If lookup is nil, the default package lookup mechanism for the given compiler is used. BUG(issue13847): For does not support non-nil lookup functions.
type Lookup

A Lookup function returns a reader to access package data for a given import path, or an error if no matching package is found.

type Lookup func(path string) (io.ReadCloser, error)

Bugs

    ☞

    For does not support non-nil lookup functions.
