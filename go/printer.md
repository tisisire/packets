
 Package printer

    import "go/printer"

    Overview
    Index
    Examples

Overview ▾

Package printer implements printing of AST nodes.
Index ▾

    func Fprint(output io.Writer, fset *token.FileSet, node interface{}) error
    type CommentedNode
    type Config
        func (cfg *Config) Fprint(output io.Writer, fset *token.FileSet, node interface{}) error
    type Mode

Examples

    Fprint

Package files

nodes.go printer.go
func Fprint

func Fprint(output io.Writer, fset *token.FileSet, node interface{}) error

Fprint "pretty-prints" an AST node to output. It calls Config.Fprint with default settings. Note that gofmt uses tabs for indentation but spaces for alignent; use format.Node (package go/format) for output that matches gofmt.

▹ Example
type CommentedNode

A CommentedNode bundles an AST node and corresponding comments. It may be provided as argument to any of the Fprint functions.

type CommentedNode struct {
        Node     interface{} // *ast.File, or ast.Expr, ast.Decl, ast.Spec, or ast.Stmt
        Comments []*ast.CommentGroup
}

type Config

A Config node controls the output of Fprint.

type Config struct {
        Mode     Mode // default: 0
        Tabwidth int  // default: 8
        Indent   int  // default: 0 (all code is indented at least by this much)
}

func (*Config) Fprint

func (cfg *Config) Fprint(output io.Writer, fset *token.FileSet, node interface{}) error

Fprint "pretty-prints" an AST node to output for a given configuration cfg. Position information is interpreted relative to the file set fset. The node type must be *ast.File, *CommentedNode, []ast.Decl, []ast.Stmt, or assignment-compatible to ast.Expr, ast.Decl, ast.Spec, or ast.Stmt.
type Mode

A Mode value is a set of flags (or 0). They control printing.

type Mode uint

const (
        RawFormat Mode = 1 << iota // do not use a tabwriter; if set, UseSpaces is ignored
        TabIndent                  // use tabs for indentation independent of UseSpaces
        UseSpaces                  // use spaces instead of tabs for alignment
        SourcePos                  // emit //line comments to preserve original source positions
)
