
 Package scanner

    import "go/scanner"

    Overview
    Index
    Examples

Overview ▾

Package scanner implements a scanner for Go source text. It takes a []byte as source which can then be tokenized through repeated calls to the Scan method.
Index ▾

    func PrintError(w io.Writer, err error)
    type Error
        func (e Error) Error() string
    type ErrorHandler
    type ErrorList
        func (p *ErrorList) Add(pos token.Position, msg string)
        func (p ErrorList) Err() error
        func (p ErrorList) Error() string
        func (p ErrorList) Len() int
        func (p ErrorList) Less(i, j int) bool
        func (p *ErrorList) RemoveMultiples()
        func (p *ErrorList) Reset()
        func (p ErrorList) Sort()
        func (p ErrorList) Swap(i, j int)
    type Mode
    type Scanner
        func (s *Scanner) Init(file *token.File, src []byte, err ErrorHandler, mode Mode)
        func (s *Scanner) Scan() (pos token.Pos, tok token.Token, lit string)

Examples

    Scanner.Scan

Package files

errors.go scanner.go
func PrintError

func PrintError(w io.Writer, err error)

PrintError is a utility function that prints a list of errors to w, one error per line, if the err parameter is an ErrorList. Otherwise it prints the err string.
type Error

In an ErrorList, an error is represented by an *Error. The position Pos, if valid, points to the beginning of the offending token, and the error condition is described by Msg.

type Error struct {
        Pos token.Position
        Msg string
}

func (Error) Error

func (e Error) Error() string

Error implements the error interface.
type ErrorHandler

An ErrorHandler may be provided to Scanner.Init. If a syntax error is encountered and a handler was installed, the handler is called with a position and an error message. The position points to the beginning of the offending token.

type ErrorHandler func(pos token.Position, msg string)

type ErrorList

ErrorList is a list of *Errors. The zero value for an ErrorList is an empty ErrorList ready to use.

type ErrorList []*Error

func (*ErrorList) Add

func (p *ErrorList) Add(pos token.Position, msg string)

Add adds an Error with given position and error message to an ErrorList.
func (ErrorList) Err

func (p ErrorList) Err() error

Err returns an error equivalent to this error list. If the list is empty, Err returns nil.
func (ErrorList) Error

func (p ErrorList) Error() string

An ErrorList implements the error interface.
func (ErrorList) Len

func (p ErrorList) Len() int

ErrorList implements the sort Interface.
func (ErrorList) Less

func (p ErrorList) Less(i, j int) bool

func (*ErrorList) RemoveMultiples

func (p *ErrorList) RemoveMultiples()

RemoveMultiples sorts an ErrorList and removes all but the first error per line.
func (*ErrorList) Reset

func (p *ErrorList) Reset()

Reset resets an ErrorList to no errors.
func (ErrorList) Sort

func (p ErrorList) Sort()

Sort sorts an ErrorList. *Error entries are sorted by position, other errors are sorted by error message, and before any *Error entry.
func (ErrorList) Swap

func (p ErrorList) Swap(i, j int)

type Mode

A mode value is a set of flags (or 0). They control scanner behavior.

type Mode uint

const (
        ScanComments Mode = 1 << iota // return comments as COMMENT tokens

)

type Scanner

A Scanner holds the scanner's internal state while processing a given text. It can be allocated as part of another data structure but must be initialized via Init before use.

type Scanner struct {

        // public state - ok to modify
        ErrorCount int // number of errors encountered
        // contains filtered or unexported fields
}

func (*Scanner) Init

func (s *Scanner) Init(file *token.File, src []byte, err ErrorHandler, mode Mode)

Init prepares the scanner s to tokenize the text src by setting the scanner at the beginning of src. The scanner uses the file set file for position information and it adds line information for each line. It is ok to re-use the same file when re-scanning the same file as line information which is already present is ignored. Init causes a panic if the file size does not match the src size.

Calls to Scan will invoke the error handler err if they encounter a syntax error and err is not nil. Also, for each error encountered, the Scanner field ErrorCount is incremented by one. The mode parameter determines how comments are handled.

Note that Init may call err if there is an error in the first character of the file.
func (*Scanner) Scan

func (s *Scanner) Scan() (pos token.Pos, tok token.Token, lit string)

Scan scans the next token and returns the token position, the token, and its literal string if applicable. The source end is indicated by token.EOF.

If the returned token is a literal (token.IDENT, token.INT, token.FLOAT, token.IMAG, token.CHAR, token.STRING) or token.COMMENT, the literal string has the corresponding value.

If the returned token is a keyword, the literal string is the keyword.

If the returned token is token.SEMICOLON, the corresponding literal string is ";" if the semicolon was present in the source, and "\n" if the semicolon was inserted because of a newline or at EOF.

If the returned token is token.ILLEGAL, the literal string is the offending character.

In all other cases, Scan returns an empty literal string.

For more tolerant parsing, Scan will return a valid token if possible even if a syntax error was encountered. Thus, even if the resulting token sequence contains no illegal tokens, a client may not assume that no error occurred. Instead it must check the scanner's ErrorCount or the number of calls of the error handler, if there was one installed.

Scan adds line information to the file added to the file set with Init. Token positions are relative to that file and thus relative to the file set.

▹ Example
