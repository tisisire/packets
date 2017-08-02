
 Package types

    import "go/types"

    Overview
    Index
    Examples

Overview ▾

Package types declares the data types and implements the algorithms for type-checking of Go packages. Use Config.Check to invoke the type checker for a package. Alternatively, create a new type checker with NewChecker and invoke it incrementally by calling Checker.Files.

Type-checking consists of several interdependent phases:

Name resolution maps each identifier (ast.Ident) in the program to the language object (Object) it denotes. Use Info.{Defs,Uses,Implicits} for the results of name resolution.

Constant folding computes the exact constant value (constant.Value) for every expression (ast.Expr) that is a compile-time constant. Use Info.Types[expr].Value for the results of constant folding.

Type inference computes the type (Type) of every expression (ast.Expr) and checks for compliance with the language specification. Use Info.Types[expr].Type for the results of type inference.

For a tutorial, see https://golang.org/s/types-tutorial.
Index ▾

    Variables
    func AssertableTo(V *Interface, T Type) bool
    func AssignableTo(V, T Type) bool
    func Comparable(T Type) bool
    func ConvertibleTo(V, T Type) bool
    func DefPredeclaredTestFuncs()
    func ExprString(x ast.Expr) string
    func Id(pkg *Package, name string) string
    func Identical(x, y Type) bool
    func IdenticalIgnoreTags(x, y Type) bool
    func Implements(V Type, T *Interface) bool
    func IsInterface(typ Type) bool
    func ObjectString(obj Object, qf Qualifier) string
    func SelectionString(s *Selection, qf Qualifier) string
    func TypeString(typ Type, qf Qualifier) string
    func WriteExpr(buf *bytes.Buffer, x ast.Expr)
    func WriteSignature(buf *bytes.Buffer, sig *Signature, qf Qualifier)
    func WriteType(buf *bytes.Buffer, typ Type, qf Qualifier)
    type Array
        func NewArray(elem Type, len int64) *Array
        func (a *Array) Elem() Type
        func (a *Array) Len() int64
        func (t *Array) String() string
        func (t *Array) Underlying() Type
    type Basic
        func (b *Basic) Info() BasicInfo
        func (b *Basic) Kind() BasicKind
        func (b *Basic) Name() string
        func (t *Basic) String() string
        func (t *Basic) Underlying() Type
    type BasicInfo
    type BasicKind
    type Builtin
        func (obj *Builtin) Exported() bool
        func (obj *Builtin) Id() string
        func (obj *Builtin) Name() string
        func (obj *Builtin) Parent() *Scope
        func (obj *Builtin) Pkg() *Package
        func (obj *Builtin) Pos() token.Pos
        func (obj *Builtin) String() string
        func (obj *Builtin) Type() Type
    type Chan
        func NewChan(dir ChanDir, elem Type) *Chan
        func (c *Chan) Dir() ChanDir
        func (c *Chan) Elem() Type
        func (t *Chan) String() string
        func (t *Chan) Underlying() Type
    type ChanDir
    type Checker
        func NewChecker(conf *Config, fset *token.FileSet, pkg *Package, info *Info) *Checker
        func (check *Checker) Files(files []*ast.File) error
    type Config
        func (conf *Config) Check(path string, fset *token.FileSet, files []*ast.File, info *Info) (*Package, error)
    type Const
        func NewConst(pos token.Pos, pkg *Package, name string, typ Type, val constant.Value) *Const
        func (obj *Const) Exported() bool
        func (obj *Const) Id() string
        func (obj *Const) Name() string
        func (obj *Const) Parent() *Scope
        func (obj *Const) Pkg() *Package
        func (obj *Const) Pos() token.Pos
        func (obj *Const) String() string
        func (obj *Const) Type() Type
        func (obj *Const) Val() constant.Value
    type Error
        func (err Error) Error() string
    type Func
        func MissingMethod(V Type, T *Interface, static bool) (method *Func, wrongType bool)
        func NewFunc(pos token.Pos, pkg *Package, name string, sig *Signature) *Func
        func (obj *Func) Exported() bool
        func (obj *Func) FullName() string
        func (obj *Func) Id() string
        func (obj *Func) Name() string
        func (obj *Func) Parent() *Scope
        func (obj *Func) Pkg() *Package
        func (obj *Func) Pos() token.Pos
        func (obj *Func) Scope() *Scope
        func (obj *Func) String() string
        func (obj *Func) Type() Type
    type ImportMode
    type Importer
    type ImporterFrom
    type Info
        func (info *Info) ObjectOf(id *ast.Ident) Object
        func (info *Info) TypeOf(e ast.Expr) Type
    type Initializer
        func (init *Initializer) String() string
    type Interface
        func NewInterface(methods []*Func, embeddeds []*Named) *Interface
        func (t *Interface) Complete() *Interface
        func (t *Interface) Embedded(i int) *Named
        func (t *Interface) Empty() bool
        func (t *Interface) ExplicitMethod(i int) *Func
        func (t *Interface) Method(i int) *Func
        func (t *Interface) NumEmbeddeds() int
        func (t *Interface) NumExplicitMethods() int
        func (t *Interface) NumMethods() int
        func (t *Interface) String() string
        func (t *Interface) Underlying() Type
    type Label
        func NewLabel(pos token.Pos, pkg *Package, name string) *Label
        func (obj *Label) Exported() bool
        func (obj *Label) Id() string
        func (obj *Label) Name() string
        func (obj *Label) Parent() *Scope
        func (obj *Label) Pkg() *Package
        func (obj *Label) Pos() token.Pos
        func (obj *Label) String() string
        func (obj *Label) Type() Type
    type Map
        func NewMap(key, elem Type) *Map
        func (m *Map) Elem() Type
        func (m *Map) Key() Type
        func (t *Map) String() string
        func (t *Map) Underlying() Type
    type MethodSet
        func NewMethodSet(T Type) *MethodSet
        func (s *MethodSet) At(i int) *Selection
        func (s *MethodSet) Len() int
        func (s *MethodSet) Lookup(pkg *Package, name string) *Selection
        func (s *MethodSet) String() string
    type Named
        func NewNamed(obj *TypeName, underlying Type, methods []*Func) *Named
        func (t *Named) AddMethod(m *Func)
        func (t *Named) Method(i int) *Func
        func (t *Named) NumMethods() int
        func (t *Named) Obj() *TypeName
        func (t *Named) SetUnderlying(underlying Type)
        func (t *Named) String() string
        func (t *Named) Underlying() Type
    type Nil
        func (obj *Nil) Exported() bool
        func (obj *Nil) Id() string
        func (obj *Nil) Name() string
        func (obj *Nil) Parent() *Scope
        func (obj *Nil) Pkg() *Package
        func (obj *Nil) Pos() token.Pos
        func (obj *Nil) String() string
        func (obj *Nil) Type() Type
    type Object
        func LookupFieldOrMethod(T Type, addressable bool, pkg *Package, name string) (obj Object, index []int, indirect bool)
    type Package
        func NewPackage(path, name string) *Package
        func (pkg *Package) Complete() bool
        func (pkg *Package) Imports() []*Package
        func (pkg *Package) MarkComplete()
        func (pkg *Package) Name() string
        func (pkg *Package) Path() string
        func (pkg *Package) Scope() *Scope
        func (pkg *Package) SetImports(list []*Package)
        func (pkg *Package) SetName(name string)
        func (pkg *Package) String() string
    type PkgName
        func NewPkgName(pos token.Pos, pkg *Package, name string, imported *Package) *PkgName
        func (obj *PkgName) Exported() bool
        func (obj *PkgName) Id() string
        func (obj *PkgName) Imported() *Package
        func (obj *PkgName) Name() string
        func (obj *PkgName) Parent() *Scope
        func (obj *PkgName) Pkg() *Package
        func (obj *PkgName) Pos() token.Pos
        func (obj *PkgName) String() string
        func (obj *PkgName) Type() Type
    type Pointer
        func NewPointer(elem Type) *Pointer
        func (p *Pointer) Elem() Type
        func (t *Pointer) String() string
        func (t *Pointer) Underlying() Type
    type Qualifier
        func RelativeTo(pkg *Package) Qualifier
    type Scope
        func NewScope(parent *Scope, pos, end token.Pos, comment string) *Scope
        func (s *Scope) Child(i int) *Scope
        func (s *Scope) Contains(pos token.Pos) bool
        func (s *Scope) End() token.Pos
        func (s *Scope) Innermost(pos token.Pos) *Scope
        func (s *Scope) Insert(obj Object) Object
        func (s *Scope) Len() int
        func (s *Scope) Lookup(name string) Object
        func (s *Scope) LookupParent(name string, pos token.Pos) (*Scope, Object)
        func (s *Scope) Names() []string
        func (s *Scope) NumChildren() int
        func (s *Scope) Parent() *Scope
        func (s *Scope) Pos() token.Pos
        func (s *Scope) String() string
        func (s *Scope) WriteTo(w io.Writer, n int, recurse bool)
    type Selection
        func (s *Selection) Index() []int
        func (s *Selection) Indirect() bool
        func (s *Selection) Kind() SelectionKind
        func (s *Selection) Obj() Object
        func (s *Selection) Recv() Type
        func (s *Selection) String() string
        func (s *Selection) Type() Type
    type SelectionKind
    type Signature
        func NewSignature(recv *Var, params, results *Tuple, variadic bool) *Signature
        func (s *Signature) Params() *Tuple
        func (s *Signature) Recv() *Var
        func (s *Signature) Results() *Tuple
        func (t *Signature) String() string
        func (t *Signature) Underlying() Type
        func (s *Signature) Variadic() bool
    type Sizes
    type Slice
        func NewSlice(elem Type) *Slice
        func (s *Slice) Elem() Type
        func (t *Slice) String() string
        func (t *Slice) Underlying() Type
    type StdSizes
        func (s *StdSizes) Alignof(T Type) int64
        func (s *StdSizes) Offsetsof(fields []*Var) []int64
        func (s *StdSizes) Sizeof(T Type) int64
    type Struct
        func NewStruct(fields []*Var, tags []string) *Struct
        func (s *Struct) Field(i int) *Var
        func (s *Struct) NumFields() int
        func (t *Struct) String() string
        func (s *Struct) Tag(i int) string
        func (t *Struct) Underlying() Type
    type Tuple
        func NewTuple(x ...*Var) *Tuple
        func (t *Tuple) At(i int) *Var
        func (t *Tuple) Len() int
        func (t *Tuple) String() string
        func (t *Tuple) Underlying() Type
    type Type
        func Default(typ Type) Type
    type TypeAndValue
        func Eval(fset *token.FileSet, pkg *Package, pos token.Pos, expr string) (TypeAndValue, error)
        func (tv TypeAndValue) Addressable() bool
        func (tv TypeAndValue) Assignable() bool
        func (tv TypeAndValue) HasOk() bool
        func (tv TypeAndValue) IsBuiltin() bool
        func (tv TypeAndValue) IsNil() bool
        func (tv TypeAndValue) IsType() bool
        func (tv TypeAndValue) IsValue() bool
        func (tv TypeAndValue) IsVoid() bool
    type TypeName
        func NewTypeName(pos token.Pos, pkg *Package, name string, typ Type) *TypeName
        func (obj *TypeName) Exported() bool
        func (obj *TypeName) Id() string
        func (obj *TypeName) Name() string
        func (obj *TypeName) Parent() *Scope
        func (obj *TypeName) Pkg() *Package
        func (obj *TypeName) Pos() token.Pos
        func (obj *TypeName) String() string
        func (obj *TypeName) Type() Type
    type Var
        func NewField(pos token.Pos, pkg *Package, name string, typ Type, anonymous bool) *Var
        func NewParam(pos token.Pos, pkg *Package, name string, typ Type) *Var
        func NewVar(pos token.Pos, pkg *Package, name string, typ Type) *Var
        func (obj *Var) Anonymous() bool
        func (obj *Var) Exported() bool
        func (obj *Var) Id() string
        func (obj *Var) IsField() bool
        func (obj *Var) Name() string
        func (obj *Var) Parent() *Scope
        func (obj *Var) Pkg() *Package
        func (obj *Var) Pos() token.Pos
        func (obj *Var) String() string
        func (obj *Var) Type() Type

Examples

    Info
    MethodSet
    Scope

Package files

api.go assignments.go builtins.go call.go check.go conversions.go decl.go errors.go eval.go expr.go exprstring.go initorder.go labels.go lookup.go methodset.go object.go objset.go operand.go ordering.go package.go predicates.go resolver.go return.go scope.go selection.go sizes.go stmt.go type.go typestring.go typexpr.go universe.go
Variables

var (
        Universe *Scope
        Unsafe   *Package
)

var Typ = []*Basic{
        Invalid: {Invalid, 0, "invalid type"},

        Bool:          {Bool, IsBoolean, "bool"},
        Int:           {Int, IsInteger, "int"},
        Int8:          {Int8, IsInteger, "int8"},
        Int16:         {Int16, IsInteger, "int16"},
        Int32:         {Int32, IsInteger, "int32"},
        Int64:         {Int64, IsInteger, "int64"},
        Uint:          {Uint, IsInteger | IsUnsigned, "uint"},
        Uint8:         {Uint8, IsInteger | IsUnsigned, "uint8"},
        Uint16:        {Uint16, IsInteger | IsUnsigned, "uint16"},
        Uint32:        {Uint32, IsInteger | IsUnsigned, "uint32"},
        Uint64:        {Uint64, IsInteger | IsUnsigned, "uint64"},
        Uintptr:       {Uintptr, IsInteger | IsUnsigned, "uintptr"},
        Float32:       {Float32, IsFloat, "float32"},
        Float64:       {Float64, IsFloat, "float64"},
        Complex64:     {Complex64, IsComplex, "complex64"},
        Complex128:    {Complex128, IsComplex, "complex128"},
        String:        {String, IsString, "string"},
        UnsafePointer: {UnsafePointer, 0, "Pointer"},

        UntypedBool:    {UntypedBool, IsBoolean | IsUntyped, "untyped bool"},
        UntypedInt:     {UntypedInt, IsInteger | IsUntyped, "untyped int"},
        UntypedRune:    {UntypedRune, IsInteger | IsUntyped, "untyped rune"},
        UntypedFloat:   {UntypedFloat, IsFloat | IsUntyped, "untyped float"},
        UntypedComplex: {UntypedComplex, IsComplex | IsUntyped, "untyped complex"},
        UntypedString:  {UntypedString, IsString | IsUntyped, "untyped string"},
        UntypedNil:     {UntypedNil, IsUntyped, "untyped nil"},
}

func AssertableTo

func AssertableTo(V *Interface, T Type) bool

AssertableTo reports whether a value of type V can be asserted to have type T.
func AssignableTo

func AssignableTo(V, T Type) bool

AssignableTo reports whether a value of type V is assignable to a variable of type T.
func Comparable

func Comparable(T Type) bool

Comparable reports whether values of type T are comparable.
func ConvertibleTo

func ConvertibleTo(V, T Type) bool

ConvertibleTo reports whether a value of type V is convertible to a value of type T.
func DefPredeclaredTestFuncs

func DefPredeclaredTestFuncs()

DefPredeclaredTestFuncs defines the assert and trace built-ins. These built-ins are intended for debugging and testing of this package only.
func ExprString

func ExprString(x ast.Expr) string

ExprString returns the (possibly simplified) string representation for x.
func Id

func Id(pkg *Package, name string) string

Id returns name if it is exported, otherwise it returns the name qualified with the package path.
func Identical

func Identical(x, y Type) bool

Identical reports whether x and y are identical.
func IdenticalIgnoreTags

func IdenticalIgnoreTags(x, y Type) bool

IdenticalIgnoreTags reports whether x and y are identical if tags are ignored.
func Implements

func Implements(V Type, T *Interface) bool

Implements reports whether type V implements interface T.
func IsInterface

func IsInterface(typ Type) bool

IsInterface reports whether typ is an interface type.
func ObjectString

func ObjectString(obj Object, qf Qualifier) string

ObjectString returns the string form of obj. The Qualifier controls the printing of package-level objects, and may be nil.
func SelectionString

func SelectionString(s *Selection, qf Qualifier) string

SelectionString returns the string form of s. The Qualifier controls the printing of package-level objects, and may be nil.

Examples:

"field (T) f int"
"method (T) f(X) Y"
"method expr (T) f(X) Y"

func TypeString

func TypeString(typ Type, qf Qualifier) string

TypeString returns the string representation of typ. The Qualifier controls the printing of package-level objects, and may be nil.
func WriteExpr

func WriteExpr(buf *bytes.Buffer, x ast.Expr)

WriteExpr writes the (possibly simplified) string representation for x to buf.
func WriteSignature

func WriteSignature(buf *bytes.Buffer, sig *Signature, qf Qualifier)

WriteSignature writes the representation of the signature sig to buf, without a leading "func" keyword. The Qualifier controls the printing of package-level objects, and may be nil.
func WriteType

func WriteType(buf *bytes.Buffer, typ Type, qf Qualifier)

WriteType writes the string representation of typ to buf. The Qualifier controls the printing of package-level objects, and may be nil.
type Array

An Array represents an array type.

type Array struct {
        // contains filtered or unexported fields
}

func NewArray

func NewArray(elem Type, len int64) *Array

NewArray returns a new array type for the given element type and length.
func (*Array) Elem

func (a *Array) Elem() Type

Elem returns element type of array a.
func (*Array) Len

func (a *Array) Len() int64

Len returns the length of array a.
func (*Array) String

func (t *Array) String() string

func (*Array) Underlying

func (t *Array) Underlying() Type

type Basic

A Basic represents a basic type.

type Basic struct {
        // contains filtered or unexported fields
}

func (*Basic) Info

func (b *Basic) Info() BasicInfo

Info returns information about properties of basic type b.
func (*Basic) Kind

func (b *Basic) Kind() BasicKind

Kind returns the kind of basic type b.
func (*Basic) Name

func (b *Basic) Name() string

Name returns the name of basic type b.
func (*Basic) String

func (t *Basic) String() string

func (*Basic) Underlying

func (t *Basic) Underlying() Type

type BasicInfo

BasicInfo is a set of flags describing properties of a basic type.

type BasicInfo int

Properties of basic types.

const (
        IsBoolean BasicInfo = 1 << iota
        IsInteger
        IsUnsigned
        IsFloat
        IsComplex
        IsString
        IsUntyped

        IsOrdered   = IsInteger | IsFloat | IsString
        IsNumeric   = IsInteger | IsFloat | IsComplex
        IsConstType = IsBoolean | IsNumeric | IsString
)

type BasicKind

BasicKind describes the kind of basic type.

type BasicKind int

const (
        Invalid BasicKind = iota // type is invalid

        // predeclared types
        Bool
        Int
        Int8
        Int16
        Int32
        Int64
        Uint
        Uint8
        Uint16
        Uint32
        Uint64
        Uintptr
        Float32
        Float64
        Complex64
        Complex128
        String
        UnsafePointer

        // types for untyped values
        UntypedBool
        UntypedInt
        UntypedRune
        UntypedFloat
        UntypedComplex
        UntypedString
        UntypedNil

        // aliases
        Byte = Uint8
        Rune = Int32
)

type Builtin

A Builtin represents a built-in function. Builtins don't have a valid type.

type Builtin struct {
        // contains filtered or unexported fields
}

func (*Builtin) Exported

func (obj *Builtin) Exported() bool

func (*Builtin) Id

func (obj *Builtin) Id() string

func (*Builtin) Name

func (obj *Builtin) Name() string

func (*Builtin) Parent

func (obj *Builtin) Parent() *Scope

func (*Builtin) Pkg

func (obj *Builtin) Pkg() *Package

func (*Builtin) Pos

func (obj *Builtin) Pos() token.Pos

func (*Builtin) String

func (obj *Builtin) String() string

func (*Builtin) Type

func (obj *Builtin) Type() Type

type Chan

A Chan represents a channel type.

type Chan struct {
        // contains filtered or unexported fields
}

func NewChan

func NewChan(dir ChanDir, elem Type) *Chan

NewChan returns a new channel type for the given direction and element type.
func (*Chan) Dir

func (c *Chan) Dir() ChanDir

Dir returns the direction of channel c.
func (*Chan) Elem

func (c *Chan) Elem() Type

Elem returns the element type of channel c.
func (*Chan) String

func (t *Chan) String() string

func (*Chan) Underlying

func (t *Chan) Underlying() Type

type ChanDir

A ChanDir value indicates a channel direction.

type ChanDir int

The direction of a channel is indicated by one of these constants.

const (
        SendRecv ChanDir = iota
        SendOnly
        RecvOnly
)

type Checker

A Checker maintains the state of the type checker. It must be created with NewChecker.

type Checker struct {
        *Info
        // contains filtered or unexported fields
}

func NewChecker

func NewChecker(conf *Config, fset *token.FileSet, pkg *Package, info *Info) *Checker

NewChecker returns a new Checker instance for a given package. Package files may be added incrementally via checker.Files.
func (*Checker) Files

func (check *Checker) Files(files []*ast.File) error

Files checks the provided files as part of the checker's package.
type Config

A Config specifies the configuration for type checking. The zero value for Config is a ready-to-use default configuration.

type Config struct {
        // If IgnoreFuncBodies is set, function bodies are not
        // type-checked.
        IgnoreFuncBodies bool

        // If FakeImportC is set, `import "C"` (for packages requiring Cgo)
        // declares an empty "C" package and errors are omitted for qualified
        // identifiers referring to package C (which won't find an object).
        // This feature is intended for the standard library cmd/api tool.
        //
        // Caution: Effects may be unpredictable due to follow-up errors.
        //          Do not use casually!
        FakeImportC bool

        // If Error != nil, it is called with each error found
        // during type checking; err has dynamic type Error.
        // Secondary errors (for instance, to enumerate all types
        // involved in an invalid recursive type declaration) have
        // error strings that start with a '\t' character.
        // If Error == nil, type-checking stops with the first
        // error found.
        Error func(err error)

        // An importer is used to import packages referred to from
        // import declarations.
        // If the installed importer implements ImporterFrom, the type
        // checker calls ImportFrom instead of Import.
        // The type checker reports an error if an importer is needed
        // but none was installed.
        Importer Importer

        // If Sizes != nil, it provides the sizing functions for package unsafe.
        // Otherwise &StdSizes{WordSize: 8, MaxAlign: 8} is used instead.
        Sizes Sizes

        // If DisableUnusedImportCheck is set, packages are not checked
        // for unused imports.
        DisableUnusedImportCheck bool
}

func (*Config) Check

func (conf *Config) Check(path string, fset *token.FileSet, files []*ast.File, info *Info) (*Package, error)

Check type-checks a package and returns the resulting package object and the first error if any. Additionally, if info != nil, Check populates each of the non-nil maps in the Info struct.

The package is marked as complete if no errors occurred, otherwise it is incomplete. See Config.Error for controlling behavior in the presence of errors.

The package is specified by a list of *ast.Files and corresponding file set, and the package path the package is identified with. The clean path must not be empty or dot (".").
type Const

A Const represents a declared constant.

type Const struct {
        // contains filtered or unexported fields
}

func NewConst

func NewConst(pos token.Pos, pkg *Package, name string, typ Type, val constant.Value) *Const

func (*Const) Exported

func (obj *Const) Exported() bool

func (*Const) Id

func (obj *Const) Id() string

func (*Const) Name

func (obj *Const) Name() string

func (*Const) Parent

func (obj *Const) Parent() *Scope

func (*Const) Pkg

func (obj *Const) Pkg() *Package

func (*Const) Pos

func (obj *Const) Pos() token.Pos

func (*Const) String

func (obj *Const) String() string

func (*Const) Type

func (obj *Const) Type() Type

func (*Const) Val

func (obj *Const) Val() constant.Value

type Error

An Error describes a type-checking error; it implements the error interface. A "soft" error is an error that still permits a valid interpretation of a package (such as "unused variable"); "hard" errors may lead to unpredictable behavior if ignored.

type Error struct {
        Fset *token.FileSet // file set for interpretation of Pos
        Pos  token.Pos      // error position
        Msg  string         // error message
        Soft bool           // if set, error is "soft"
}

func (Error) Error

func (err Error) Error() string

Error returns an error string formatted as follows: filename:line:column: message
type Func

A Func represents a declared function, concrete method, or abstract (interface) method. Its Type() is always a *Signature. An abstract method may belong to many interfaces due to embedding.

type Func struct {
        // contains filtered or unexported fields
}

func MissingMethod

func MissingMethod(V Type, T *Interface, static bool) (method *Func, wrongType bool)

MissingMethod returns (nil, false) if V implements T, otherwise it returns a missing method required by T and whether it is missing or just has the wrong type.

For non-interface types V, or if static is set, V implements T if all methods of T are present in V. Otherwise (V is an interface and static is not set), MissingMethod only checks that methods of T which are also present in V have matching types (e.g., for a type assertion x.(T) where x is of interface type V).
func NewFunc

func NewFunc(pos token.Pos, pkg *Package, name string, sig *Signature) *Func

func (*Func) Exported

func (obj *Func) Exported() bool

func (*Func) FullName

func (obj *Func) FullName() string

FullName returns the package- or receiver-type-qualified name of function or method obj.
func (*Func) Id

func (obj *Func) Id() string

func (*Func) Name

func (obj *Func) Name() string

func (*Func) Parent

func (obj *Func) Parent() *Scope

func (*Func) Pkg

func (obj *Func) Pkg() *Package

func (*Func) Pos

func (obj *Func) Pos() token.Pos

func (*Func) Scope

func (obj *Func) Scope() *Scope

func (*Func) String

func (obj *Func) String() string

func (*Func) Type

func (obj *Func) Type() Type

type ImportMode

ImportMode is reserved for future use.

type ImportMode int

type Importer

An Importer resolves import paths to Packages.

CAUTION: This interface does not support the import of locally vendored packages. See https://golang.org/s/go15vendor. If possible, external implementations should implement ImporterFrom.

type Importer interface {
        // Import returns the imported package for the given import
        // path, or an error if the package couldn't be imported.
        // Two calls to Import with the same path return the same
        // package.
        Import(path string) (*Package, error)
}

type ImporterFrom

An ImporterFrom resolves import paths to packages; it supports vendoring per https://golang.org/s/go15vendor. Use go/importer to obtain an ImporterFrom implementation.

type ImporterFrom interface {
        // Importer is present for backward-compatibility. Calling
        // Import(path) is the same as calling ImportFrom(path, "", 0);
        // i.e., locally vendored packages may not be found.
        // The types package does not call Import if an ImporterFrom
        // is present.
        Importer

        // ImportFrom returns the imported package for the given import
        // path when imported by the package in srcDir, or an error
        // if the package couldn't be imported. The mode value must
        // be 0; it is reserved for future use.
        // Two calls to ImportFrom with the same path and srcDir return
        // the same package.
        ImportFrom(path, srcDir string, mode ImportMode) (*Package, error)
}

type Info

Info holds result type information for a type-checked package. Only the information for which a map is provided is collected. If the package has type errors, the collected information may be incomplete.

type Info struct {
        // Types maps expressions to their types, and for constant
        // expressions, also their values. Invalid expressions are
        // omitted.
        //
        // For (possibly parenthesized) identifiers denoting built-in
        // functions, the recorded signatures are call-site specific:
        // if the call result is not a constant, the recorded type is
        // an argument-specific signature. Otherwise, the recorded type
        // is invalid.
        //
        // The Types map does not record the type of every identifier,
        // only those that appear where an arbitrary expression is
        // permitted. For instance, the identifier f in a selector
        // expression x.f is found only in the Selections map, the
        // identifier z in a variable declaration 'var z int' is found
        // only in the Defs map, and identifiers denoting packages in
        // qualified identifiers are collected in the Uses map.
        Types map[ast.Expr]TypeAndValue

        // Defs maps identifiers to the objects they define (including
        // package names, dots "." of dot-imports, and blank "_" identifiers).
        // For identifiers that do not denote objects (e.g., the package name
        // in package clauses, or symbolic variables t in t := x.(type) of
        // type switch headers), the corresponding objects are nil.
        //
        // For an anonymous field, Defs returns the field *Var it defines.
        //
        // Invariant: Defs[id] == nil || Defs[id].Pos() == id.Pos()
        Defs map[*ast.Ident]Object

        // Uses maps identifiers to the objects they denote.
        //
        // For an anonymous field, Uses returns the *TypeName it denotes.
        //
        // Invariant: Uses[id].Pos() != id.Pos()
        Uses map[*ast.Ident]Object

        // Implicits maps nodes to their implicitly declared objects, if any.
        // The following node and object types may appear:
        //
        //	node               declared object
        //
        //	*ast.ImportSpec    *PkgName for dot-imports and imports without renames
        //	*ast.CaseClause    type-specific *Var for each type switch case clause (incl. default)
        //      *ast.Field         anonymous parameter *Var
        //
        Implicits map[ast.Node]Object

        // Selections maps selector expressions (excluding qualified identifiers)
        // to their corresponding selections.
        Selections map[*ast.SelectorExpr]*Selection

        // Scopes maps ast.Nodes to the scopes they define. Package scopes are not
        // associated with a specific node but with all files belonging to a package.
        // Thus, the package scope can be found in the type-checked Package object.
        // Scopes nest, with the Universe scope being the outermost scope, enclosing
        // the package scope, which contains (one or more) files scopes, which enclose
        // function scopes which in turn enclose statement and function literal scopes.
        // Note that even though package-level functions are declared in the package
        // scope, the function scopes are embedded in the file scope of the file
        // containing the function declaration.
        //
        // The following node types may appear in Scopes:
        //
        //	*ast.File
        //	*ast.FuncType
        //	*ast.BlockStmt
        //	*ast.IfStmt
        //	*ast.SwitchStmt
        //	*ast.TypeSwitchStmt
        //	*ast.CaseClause
        //	*ast.CommClause
        //	*ast.ForStmt
        //	*ast.RangeStmt
        //
        Scopes map[ast.Node]*Scope

        // InitOrder is the list of package-level initializers in the order in which
        // they must be executed. Initializers referring to variables related by an
        // initialization dependency appear in topological order, the others appear
        // in source order. Variables without an initialization expression do not
        // appear in this list.
        InitOrder []*Initializer
}

▹ Example
func (*Info) ObjectOf

func (info *Info) ObjectOf(id *ast.Ident) Object

ObjectOf returns the object denoted by the specified id, or nil if not found.

If id is an anonymous struct field, ObjectOf returns the field (*Var) it uses, not the type (*TypeName) it defines.

Precondition: the Uses and Defs maps are populated.
func (*Info) TypeOf

func (info *Info) TypeOf(e ast.Expr) Type

TypeOf returns the type of expression e, or nil if not found. Precondition: the Types, Uses and Defs maps are populated.
type Initializer

An Initializer describes a package-level variable, or a list of variables in case of a multi-valued initialization expression, and the corresponding initialization expression.

type Initializer struct {
        Lhs []*Var // var Lhs = Rhs
        Rhs ast.Expr
}

func (*Initializer) String

func (init *Initializer) String() string

type Interface

An Interface represents an interface type.

type Interface struct {
        // contains filtered or unexported fields
}

func NewInterface

func NewInterface(methods []*Func, embeddeds []*Named) *Interface

NewInterface returns a new interface for the given methods and embedded types.
func (*Interface) Complete

func (t *Interface) Complete() *Interface

Complete computes the interface's method set. It must be called by users of NewInterface after the interface's embedded types are fully defined and before using the interface type in any way other than to form other types. Complete returns the receiver.
func (*Interface) Embedded

func (t *Interface) Embedded(i int) *Named

Embedded returns the i'th embedded type of interface t for 0 <= i < t.NumEmbeddeds(). The types are ordered by the corresponding TypeName's unique Id.
func (*Interface) Empty

func (t *Interface) Empty() bool

Empty returns true if t is the empty interface.
func (*Interface) ExplicitMethod

func (t *Interface) ExplicitMethod(i int) *Func

ExplicitMethod returns the i'th explicitly declared method of interface t for 0 <= i < t.NumExplicitMethods(). The methods are ordered by their unique Id.
func (*Interface) Method

func (t *Interface) Method(i int) *Func

Method returns the i'th method of interface t for 0 <= i < t.NumMethods(). The methods are ordered by their unique Id.
func (*Interface) NumEmbeddeds

func (t *Interface) NumEmbeddeds() int

NumEmbeddeds returns the number of embedded types in interface t.
func (*Interface) NumExplicitMethods

func (t *Interface) NumExplicitMethods() int

NumExplicitMethods returns the number of explicitly declared methods of interface t.
func (*Interface) NumMethods

func (t *Interface) NumMethods() int

NumMethods returns the total number of methods of interface t.
func (*Interface) String

func (t *Interface) String() string

func (*Interface) Underlying

func (t *Interface) Underlying() Type

type Label

A Label represents a declared label.

type Label struct {
        // contains filtered or unexported fields
}

func NewLabel

func NewLabel(pos token.Pos, pkg *Package, name string) *Label

func (*Label) Exported

func (obj *Label) Exported() bool

func (*Label) Id

func (obj *Label) Id() string

func (*Label) Name

func (obj *Label) Name() string

func (*Label) Parent

func (obj *Label) Parent() *Scope

func (*Label) Pkg

func (obj *Label) Pkg() *Package

func (*Label) Pos

func (obj *Label) Pos() token.Pos

func (*Label) String

func (obj *Label) String() string

func (*Label) Type

func (obj *Label) Type() Type

type Map

A Map represents a map type.

type Map struct {
        // contains filtered or unexported fields
}

func NewMap

func NewMap(key, elem Type) *Map

NewMap returns a new map for the given key and element types.
func (*Map) Elem

func (m *Map) Elem() Type

Elem returns the element type of map m.
func (*Map) Key

func (m *Map) Key() Type

Key returns the key type of map m.
func (*Map) String

func (t *Map) String() string

func (*Map) Underlying

func (t *Map) Underlying() Type

type MethodSet

A MethodSet is an ordered set of concrete or abstract (interface) methods; a method is a MethodVal selection, and they are ordered by ascending m.Obj().Id(). The zero value for a MethodSet is a ready-to-use empty method set.

type MethodSet struct {
        // contains filtered or unexported fields
}

▹ Example
func NewMethodSet

func NewMethodSet(T Type) *MethodSet

NewMethodSet returns the method set for the given type T. It always returns a non-nil method set, even if it is empty.
func (*MethodSet) At

func (s *MethodSet) At(i int) *Selection

At returns the i'th method in s for 0 <= i < s.Len().
func (*MethodSet) Len

func (s *MethodSet) Len() int

Len returns the number of methods in s.
func (*MethodSet) Lookup

func (s *MethodSet) Lookup(pkg *Package, name string) *Selection

Lookup returns the method with matching package and name, or nil if not found.
func (*MethodSet) String

func (s *MethodSet) String() string

type Named

A Named represents a named type.

type Named struct {
        // contains filtered or unexported fields
}

func NewNamed

func NewNamed(obj *TypeName, underlying Type, methods []*Func) *Named

NewNamed returns a new named type for the given type name, underlying type, and associated methods. The underlying type must not be a *Named.
func (*Named) AddMethod

func (t *Named) AddMethod(m *Func)

AddMethod adds method m unless it is already in the method list. TODO(gri) find a better solution instead of providing this function
func (*Named) Method

func (t *Named) Method(i int) *Func

Method returns the i'th method of named type t for 0 <= i < t.NumMethods().
func (*Named) NumMethods

func (t *Named) NumMethods() int

NumMethods returns the number of explicit methods whose receiver is named type t.
func (*Named) Obj

func (t *Named) Obj() *TypeName

TypeName returns the type name for the named type t.
func (*Named) SetUnderlying

func (t *Named) SetUnderlying(underlying Type)

SetUnderlying sets the underlying type and marks t as complete. TODO(gri) determine if there's a better solution rather than providing this function
func (*Named) String

func (t *Named) String() string

func (*Named) Underlying

func (t *Named) Underlying() Type

type Nil

Nil represents the predeclared value nil.

type Nil struct {
        // contains filtered or unexported fields
}

func (*Nil) Exported

func (obj *Nil) Exported() bool

func (*Nil) Id

func (obj *Nil) Id() string

func (*Nil) Name

func (obj *Nil) Name() string

func (*Nil) Parent

func (obj *Nil) Parent() *Scope

func (*Nil) Pkg

func (obj *Nil) Pkg() *Package

func (*Nil) Pos

func (obj *Nil) Pos() token.Pos

func (*Nil) String

func (obj *Nil) String() string

func (*Nil) Type

func (obj *Nil) Type() Type

type Object

An Object describes a named language entity such as a package, constant, type, variable, function (incl. methods), or label. All objects implement the Object interface.

type Object interface {
        Parent() *Scope // scope in which this object is declared; nil for methods and struct fields
        Pos() token.Pos // position of object identifier in declaration
        Pkg() *Package  // nil for objects in the Universe scope and labels
        Name() string   // package local object name
        Type() Type     // object type
        Exported() bool // reports whether the name starts with a capital letter
        Id() string     // object id (see Id below)

        // String returns a human-readable string of the object.
        String() string
        // contains filtered or unexported methods
}

func LookupFieldOrMethod

func LookupFieldOrMethod(T Type, addressable bool, pkg *Package, name string) (obj Object, index []int, indirect bool)

LookupFieldOrMethod looks up a field or method with given package and name in T and returns the corresponding *Var or *Func, an index sequence, and a bool indicating if there were any pointer indirections on the path to the field or method. If addressable is set, T is the type of an addressable variable (only matters for method lookups).

The last index entry is the field or method index in the (possibly embedded) type where the entry was found, either:

1) the list of declared methods of a named type; or
2) the list of all methods (method set) of an interface type; or
3) the list of fields of a struct type.

The earlier index entries are the indices of the anonymous struct fields traversed to get to the found entry, starting at depth 0.

If no entry is found, a nil object is returned. In this case, the returned index and indirect values have the following meaning:

	- If index != nil, the index sequence points to an ambiguous entry
	(the same name appeared more than once at the same embedding level).

	- If indirect is set, a method with a pointer receiver type was found
     but there was no pointer on the path from the actual receiver type to
	the method's formal receiver base type, nor was the receiver addressable.

type Package

A Package describes a Go package.

type Package struct {
        // contains filtered or unexported fields
}

func NewPackage

func NewPackage(path, name string) *Package

NewPackage returns a new Package for the given package path and name; the name must not be the blank identifier. The package is not complete and contains no explicit imports.
func (*Package) Complete

func (pkg *Package) Complete() bool

A package is complete if its scope contains (at least) all exported objects; otherwise it is incomplete.
func (*Package) Imports

func (pkg *Package) Imports() []*Package

Imports returns the list of packages directly imported by pkg; the list is in source order. Package unsafe is excluded.

If pkg was loaded from export data, Imports includes packages that provide package-level objects referenced by pkg. This may be more or less than the set of packages directly imported by pkg's source code.
func (*Package) MarkComplete

func (pkg *Package) MarkComplete()

MarkComplete marks a package as complete.
func (*Package) Name

func (pkg *Package) Name() string

Name returns the package name.
func (*Package) Path

func (pkg *Package) Path() string

Path returns the package path.
func (*Package) Scope

func (pkg *Package) Scope() *Scope

Scope returns the (complete or incomplete) package scope holding the objects declared at package level (TypeNames, Consts, Vars, and Funcs).
func (*Package) SetImports

func (pkg *Package) SetImports(list []*Package)

SetImports sets the list of explicitly imported packages to list. It is the caller's responsibility to make sure list elements are unique.
func (*Package) SetName

func (pkg *Package) SetName(name string)

SetName sets the package name.
func (*Package) String

func (pkg *Package) String() string

type PkgName

A PkgName represents an imported Go package.

type PkgName struct {
        // contains filtered or unexported fields
}

func NewPkgName

func NewPkgName(pos token.Pos, pkg *Package, name string, imported *Package) *PkgName

func (*PkgName) Exported

func (obj *PkgName) Exported() bool

func (*PkgName) Id

func (obj *PkgName) Id() string

func (*PkgName) Imported

func (obj *PkgName) Imported() *Package

Imported returns the package that was imported. It is distinct from Pkg(), which is the package containing the import statement.
func (*PkgName) Name

func (obj *PkgName) Name() string

func (*PkgName) Parent

func (obj *PkgName) Parent() *Scope

func (*PkgName) Pkg

func (obj *PkgName) Pkg() *Package

func (*PkgName) Pos

func (obj *PkgName) Pos() token.Pos

func (*PkgName) String

func (obj *PkgName) String() string

func (*PkgName) Type

func (obj *PkgName) Type() Type

type Pointer

A Pointer represents a pointer type.

type Pointer struct {
        // contains filtered or unexported fields
}

func NewPointer

func NewPointer(elem Type) *Pointer

NewPointer returns a new pointer type for the given element (base) type.
func (*Pointer) Elem

func (p *Pointer) Elem() Type

Elem returns the element type for the given pointer p.
func (*Pointer) String

func (t *Pointer) String() string

func (*Pointer) Underlying

func (t *Pointer) Underlying() Type

type Qualifier

A Qualifier controls how named package-level objects are printed in calls to TypeString, ObjectString, and SelectionString.

These three formatting routines call the Qualifier for each package-level object O, and if the Qualifier returns a non-empty string p, the object is printed in the form p.O. If it returns an empty string, only the object name O is printed.

Using a nil Qualifier is equivalent to using (*Package).Path: the object is qualified by the import path, e.g., "encoding/json.Marshal".

type Qualifier func(*Package) string

func RelativeTo

func RelativeTo(pkg *Package) Qualifier

RelativeTo(pkg) returns a Qualifier that fully qualifies members of all packages other than pkg.
type Scope

A Scope maintains a set of objects and links to its containing (parent) and contained (children) scopes. Objects may be inserted and looked up by name. The zero value for Scope is a ready-to-use empty scope.

type Scope struct {
        // contains filtered or unexported fields
}

▹ Example
func NewScope

func NewScope(parent *Scope, pos, end token.Pos, comment string) *Scope

NewScope returns a new, empty scope contained in the given parent scope, if any. The comment is for debugging only.
func (*Scope) Child

func (s *Scope) Child(i int) *Scope

Child returns the i'th child scope for 0 <= i < NumChildren().
func (*Scope) Contains

func (s *Scope) Contains(pos token.Pos) bool

Contains returns true if pos is within the scope's extent. The result is guaranteed to be valid only if the type-checked AST has complete position information.
func (*Scope) End

func (s *Scope) End() token.Pos

func (*Scope) Innermost

func (s *Scope) Innermost(pos token.Pos) *Scope

Innermost returns the innermost (child) scope containing pos. If pos is not within any scope, the result is nil. The result is also nil for the Universe scope. The result is guaranteed to be valid only if the type-checked AST has complete position information.
func (*Scope) Insert

func (s *Scope) Insert(obj Object) Object

Insert attempts to insert an object obj into scope s. If s already contains an alternative object alt with the same name, Insert leaves s unchanged and returns alt. Otherwise it inserts obj, sets the object's parent scope if not already set, and returns nil.
func (*Scope) Len

func (s *Scope) Len() int

Len() returns the number of scope elements.
func (*Scope) Lookup

func (s *Scope) Lookup(name string) Object

Lookup returns the object in scope s with the given name if such an object exists; otherwise the result is nil.
func (*Scope) LookupParent

func (s *Scope) LookupParent(name string, pos token.Pos) (*Scope, Object)

LookupParent follows the parent chain of scopes starting with s until it finds a scope where Lookup(name) returns a non-nil object, and then returns that scope and object. If a valid position pos is provided, only objects that were declared at or before pos are considered. If no such scope and object exists, the result is (nil, nil).

Note that obj.Parent() may be different from the returned scope if the object was inserted into the scope and already had a parent at that time (see Insert, below). This can only happen for dot-imported objects whose scope is the scope of the package that exported them.
func (*Scope) Names

func (s *Scope) Names() []string

Names returns the scope's element names in sorted order.
func (*Scope) NumChildren

func (s *Scope) NumChildren() int

NumChildren() returns the number of scopes nested in s.
func (*Scope) Parent

func (s *Scope) Parent() *Scope

Parent returns the scope's containing (parent) scope.
func (*Scope) Pos

func (s *Scope) Pos() token.Pos

Pos and End describe the scope's source code extent [pos, end). The results are guaranteed to be valid only if the type-checked AST has complete position information. The extent is undefined for Universe and package scopes.
func (*Scope) String

func (s *Scope) String() string

String returns a string representation of the scope, for debugging.
func (*Scope) WriteTo

func (s *Scope) WriteTo(w io.Writer, n int, recurse bool)

WriteTo writes a string representation of the scope to w, with the scope elements sorted by name. The level of indentation is controlled by n >= 0, with n == 0 for no indentation. If recurse is set, it also writes nested (children) scopes.
type Selection

A Selection describes a selector expression x.f. For the declarations:

type T struct{ x int; E }
type E struct{}
func (e E) m() {}
var p *T

the following relations exist:

Selector    Kind          Recv    Obj    Type               Index     Indirect

p.x         FieldVal      T       x      int                {0}       true
p.m         MethodVal     *T      m      func (e *T) m()    {1, 0}    true
T.m         MethodExpr    T       m      func m(_ T)        {1, 0}    false

type Selection struct {
        // contains filtered or unexported fields
}

func (*Selection) Index

func (s *Selection) Index() []int

Index describes the path from x to f in x.f. The last index entry is the field or method index of the type declaring f; either:

1) the list of declared methods of a named type; or
2) the list of methods of an interface type; or
3) the list of fields of a struct type.

The earlier index entries are the indices of the embedded fields implicitly traversed to get from (the type of) x to f, starting at embedding depth 0.
func (*Selection) Indirect

func (s *Selection) Indirect() bool

Indirect reports whether any pointer indirection was required to get from x to f in x.f.
func (*Selection) Kind

func (s *Selection) Kind() SelectionKind

Kind returns the selection kind.
func (*Selection) Obj

func (s *Selection) Obj() Object

Obj returns the object denoted by x.f; a *Var for a field selection, and a *Func in all other cases.
func (*Selection) Recv

func (s *Selection) Recv() Type

Recv returns the type of x in x.f.
func (*Selection) String

func (s *Selection) String() string

func (*Selection) Type

func (s *Selection) Type() Type

Type returns the type of x.f, which may be different from the type of f. See Selection for more information.
type SelectionKind

SelectionKind describes the kind of a selector expression x.f (excluding qualified identifiers).

type SelectionKind int

const (
        FieldVal   SelectionKind = iota // x.f is a struct field selector
        MethodVal                       // x.f is a method selector
        MethodExpr                      // x.f is a method expression
)

type Signature

A Signature represents a (non-builtin) function or method type.

type Signature struct {
        // contains filtered or unexported fields
}

func NewSignature

func NewSignature(recv *Var, params, results *Tuple, variadic bool) *Signature

NewSignature returns a new function type for the given receiver, parameters, and results, either of which may be nil. If variadic is set, the function is variadic, it must have at least one parameter, and the last parameter must be of unnamed slice type.
func (*Signature) Params

func (s *Signature) Params() *Tuple

Params returns the parameters of signature s, or nil.
func (*Signature) Recv

func (s *Signature) Recv() *Var

Recv returns the receiver of signature s (if a method), or nil if a function.

For an abstract method, Recv returns the enclosing interface either as a *Named or an *Interface. Due to embedding, an interface may contain methods whose receiver type is a different interface.
func (*Signature) Results

func (s *Signature) Results() *Tuple

Results returns the results of signature s, or nil.
func (*Signature) String

func (t *Signature) String() string

func (*Signature) Underlying

func (t *Signature) Underlying() Type

func (*Signature) Variadic

func (s *Signature) Variadic() bool

Variadic reports whether the signature s is variadic.
type Sizes

Sizes defines the sizing functions for package unsafe.

type Sizes interface {
        // Alignof returns the alignment of a variable of type T.
        // Alignof must implement the alignment guarantees required by the spec.
        Alignof(T Type) int64

        // Offsetsof returns the offsets of the given struct fields, in bytes.
        // Offsetsof must implement the offset guarantees required by the spec.
        Offsetsof(fields []*Var) []int64

        // Sizeof returns the size of a variable of type T.
        // Sizeof must implement the size guarantees required by the spec.
        Sizeof(T Type) int64
}

type Slice

A Slice represents a slice type.

type Slice struct {
        // contains filtered or unexported fields
}

func NewSlice

func NewSlice(elem Type) *Slice

NewSlice returns a new slice type for the given element type.
func (*Slice) Elem

func (s *Slice) Elem() Type

Elem returns the element type of slice s.
func (*Slice) String

func (t *Slice) String() string

func (*Slice) Underlying

func (t *Slice) Underlying() Type

type StdSizes

StdSizes is a convenience type for creating commonly used Sizes. It makes the following simplifying assumptions:

	- The size of explicitly sized basic types (int16, etc.) is the
	  specified size.
	- The size of strings and interfaces is 2*WordSize.
	- The size of slices is 3*WordSize.
	- The size of an array of n elements corresponds to the size of
	  a struct of n consecutive fields of the array's element type.
     - The size of a struct is the offset of the last field plus that
	  field's size. As with all element types, if the struct is used
	  in an array its size must first be aligned to a multiple of the
	  struct's alignment.
	- All other types have size WordSize.
	- Arrays and structs are aligned per spec definition; all other
	  types are naturally aligned with a maximum alignment MaxAlign.

*StdSizes implements Sizes.

type StdSizes struct {
        WordSize int64 // word size in bytes - must be >= 4 (32bits)
        MaxAlign int64 // maximum alignment in bytes - must be >= 1
}

func (*StdSizes) Alignof

func (s *StdSizes) Alignof(T Type) int64

func (*StdSizes) Offsetsof

func (s *StdSizes) Offsetsof(fields []*Var) []int64

func (*StdSizes) Sizeof

func (s *StdSizes) Sizeof(T Type) int64

type Struct

A Struct represents a struct type.

type Struct struct {
        // contains filtered or unexported fields
}

func NewStruct

func NewStruct(fields []*Var, tags []string) *Struct

NewStruct returns a new struct with the given fields and corresponding field tags. If a field with index i has a tag, tags[i] must be that tag, but len(tags) may be only as long as required to hold the tag with the largest index i. Consequently, if no field has a tag, tags may be nil.
func (*Struct) Field

func (s *Struct) Field(i int) *Var

Field returns the i'th field for 0 <= i < NumFields().
func (*Struct) NumFields

func (s *Struct) NumFields() int

NumFields returns the number of fields in the struct (including blank and anonymous fields).
func (*Struct) String

func (t *Struct) String() string

func (*Struct) Tag

func (s *Struct) Tag(i int) string

Tag returns the i'th field tag for 0 <= i < NumFields().
func (*Struct) Underlying

func (t *Struct) Underlying() Type

type Tuple

A Tuple represents an ordered list of variables; a nil *Tuple is a valid (empty) tuple. Tuples are used as components of signatures and to represent the type of multiple assignments; they are not first class types of Go.

type Tuple struct {
        // contains filtered or unexported fields
}

func NewTuple

func NewTuple(x ...*Var) *Tuple

NewTuple returns a new tuple for the given variables.
func (*Tuple) At

func (t *Tuple) At(i int) *Var

At returns the i'th variable of tuple t.
func (*Tuple) Len

func (t *Tuple) Len() int

Len returns the number variables of tuple t.
func (*Tuple) String

func (t *Tuple) String() string

func (*Tuple) Underlying

func (t *Tuple) Underlying() Type

type Type

A Type represents a type of Go. All types implement the Type interface.

type Type interface {
        // Underlying returns the underlying type of a type.
        Underlying() Type

        // String returns a string representation of a type.
        String() string
}

func Default

func Default(typ Type) Type

Default returns the default "typed" type for an "untyped" type; it returns the incoming type for all other types. The default type for untyped nil is untyped nil.
type TypeAndValue

TypeAndValue reports the type and value (for constants) of the corresponding expression.

type TypeAndValue struct {
        Type  Type
        Value constant.Value
        // contains filtered or unexported fields
}

func Eval

func Eval(fset *token.FileSet, pkg *Package, pos token.Pos, expr string) (TypeAndValue, error)

Eval returns the type and, if constant, the value for the expression expr, evaluated at position pos of package pkg, which must have been derived from type-checking an AST with complete position information relative to the provided file set.

If the expression contains function literals, their bodies are ignored (i.e., the bodies are not type-checked).

If pkg == nil, the Universe scope is used and the provided position pos is ignored. If pkg != nil, and pos is invalid, the package scope is used. Otherwise, pos must belong to the package.

An error is returned if pos is not within the package or if the node cannot be evaluated.

Note: Eval should not be used instead of running Check to compute types and values, but in addition to Check. Eval will re-evaluate its argument each time, and it also does not know about the context in which an expression is used (e.g., an assignment). Thus, top- level untyped constants will return an untyped type rather then the respective context-specific type.
func (TypeAndValue) Addressable

func (tv TypeAndValue) Addressable() bool

Addressable reports whether the corresponding expression is addressable (https://golang.org/ref/spec#Address_operators).
func (TypeAndValue) Assignable

func (tv TypeAndValue) Assignable() bool

Assignable reports whether the corresponding expression is assignable to (provided a value of the right type).
func (TypeAndValue) HasOk

func (tv TypeAndValue) HasOk() bool

HasOk reports whether the corresponding expression may be used on the lhs of a comma-ok assignment.
func (TypeAndValue) IsBuiltin

func (tv TypeAndValue) IsBuiltin() bool

IsBuiltin reports whether the corresponding expression denotes a (possibly parenthesized) built-in function.
func (TypeAndValue) IsNil

func (tv TypeAndValue) IsNil() bool

IsNil reports whether the corresponding expression denotes the predeclared value nil.
func (TypeAndValue) IsType

func (tv TypeAndValue) IsType() bool

IsType reports whether the corresponding expression specifies a type.
func (TypeAndValue) IsValue

func (tv TypeAndValue) IsValue() bool

IsValue reports whether the corresponding expression is a value. Builtins are not considered values. Constant values have a non- nil Value.
func (TypeAndValue) IsVoid

func (tv TypeAndValue) IsVoid() bool

IsVoid reports whether the corresponding expression is a function call without results.
type TypeName

A TypeName represents a declared type.

type TypeName struct {
        // contains filtered or unexported fields
}

func NewTypeName

func NewTypeName(pos token.Pos, pkg *Package, name string, typ Type) *TypeName

func (*TypeName) Exported

func (obj *TypeName) Exported() bool

func (*TypeName) Id

func (obj *TypeName) Id() string

func (*TypeName) Name

func (obj *TypeName) Name() string

func (*TypeName) Parent

func (obj *TypeName) Parent() *Scope

func (*TypeName) Pkg

func (obj *TypeName) Pkg() *Package

func (*TypeName) Pos

func (obj *TypeName) Pos() token.Pos

func (*TypeName) String

func (obj *TypeName) String() string

func (*TypeName) Type

func (obj *TypeName) Type() Type

type Var

A Variable represents a declared variable (including function parameters and results, and struct fields).

type Var struct {
        // contains filtered or unexported fields
}

func NewField

func NewField(pos token.Pos, pkg *Package, name string, typ Type, anonymous bool) *Var

func NewParam

func NewParam(pos token.Pos, pkg *Package, name string, typ Type) *Var

func NewVar

func NewVar(pos token.Pos, pkg *Package, name string, typ Type) *Var

func (*Var) Anonymous

func (obj *Var) Anonymous() bool

func (*Var) Exported

func (obj *Var) Exported() bool

func (*Var) Id

func (obj *Var) Id() string

func (*Var) IsField

func (obj *Var) IsField() bool

func (*Var) Name

func (obj *Var) Name() string

func (*Var) Parent

func (obj *Var) Parent() *Scope

func (*Var) Pkg

func (obj *Var) Pkg() *Package

func (*Var) Pos

func (obj *Var) Pos() token.Pos

func (*Var) String

func (obj *Var) String() string

func (*Var) Type

func (obj *Var) Type() Type
