
 
  Package csv

    import "encoding/csv"

    Overview
    Index
    Examples

Overview ▾

Package csv reads and writes comma-separated values (CSV) files. There are many kinds of CSV files; this package supports the format described in RFC 4180.

A csv file contains zero or more records of one or more fields per record. Each record is separated by the newline character. The final record may optionally be followed by a newline character.

field1,field2,field3

White space is considered part of a field.

Carriage returns before newline characters are silently removed.

Blank lines are ignored. A line with only whitespace characters (excluding the ending newline character) is not considered a blank line.

Fields which start and stop with the quote character " are called quoted-fields. The beginning and ending quote are not part of the field.

The source:

normal string,"quoted-field"

results in the fields

{`normal string`, `quoted-field`}

Within a quoted-field a quote character followed by a second quote character is considered a single quote.

"the ""word"" is true","a ""quoted-field"""

results in

{`the "word" is true`, `a "quoted-field"`}

Newlines and commas may be included in a quoted-field

"Multi-line
field","comma is ,"

results in

{`Multi-line
field`, `comma is ,`}

Index ▾

    Variables
    type ParseError
        func (e *ParseError) Error() string
    type Reader
        func NewReader(r io.Reader) *Reader
        func (r *Reader) Read() (record []string, err error)
        func (r *Reader) ReadAll() (records [][]string, err error)
    type Writer
        func NewWriter(w io.Writer) *Writer
        func (w *Writer) Error() error
        func (w *Writer) Flush()
        func (w *Writer) Write(record []string) error
        func (w *Writer) WriteAll(records [][]string) error

Examples

    Reader
    Reader.ReadAll
    Reader (Options)
    Writer
    Writer.WriteAll

Package files

reader.go writer.go
Variables

These are the errors that can be returned in ParseError.Error

var (
        ErrTrailingComma = errors.New("extra delimiter at end of line") // no longer used
        ErrBareQuote     = errors.New("bare \" in non-quoted-field")
        ErrQuote         = errors.New("extraneous \" in field")
        ErrFieldCount    = errors.New("wrong number of fields in line")
)

type ParseError

A ParseError is returned for parsing errors. The first line is 1. The first column is 0.

type ParseError struct {
        Line   int   // Line where the error occurred
        Column int   // Column (rune index) where the error occurred
        Err    error // The actual error
}

func (*ParseError) Error

func (e *ParseError) Error() string

type Reader

A Reader reads records from a CSV-encoded file.

As returned by NewReader, a Reader expects input conforming to RFC 4180. The exported fields can be changed to customize the details before the first call to Read or ReadAll.

type Reader struct {
        // Comma is the field delimiter.
        // It is set to comma (',') by NewReader.
        Comma rune
        // Comment, if not 0, is the comment character. Lines beginning with the
        // Comment character without preceding whitespace are ignored.
        // With leading whitespace the Comment character becomes part of the
        // field, even if TrimLeadingSpace is true.
        Comment rune
        // FieldsPerRecord is the number of expected fields per record.
        // If FieldsPerRecord is positive, Read requires each record to
        // have the given number of fields. If FieldsPerRecord is 0, Read sets it to
        // the number of fields in the first record, so that future records must
        // have the same field count. If FieldsPerRecord is negative, no check is
        // made and records may have a variable number of fields.
        FieldsPerRecord int
        // If LazyQuotes is true, a quote may appear in an unquoted field and a
        // non-doubled quote may appear in a quoted field.
        LazyQuotes    bool
        TrailingComma bool // ignored; here for backwards compatibility
        // If TrimLeadingSpace is true, leading white space in a field is ignored.
        // This is done even if the field delimiter, Comma, is white space.
        TrimLeadingSpace bool
        // contains filtered or unexported fields
}

▹ Example

▹ Example (Options)
func NewReader

func NewReader(r io.Reader) *Reader

NewReader returns a new Reader that reads from r.
func (*Reader) Read

func (r *Reader) Read() (record []string, err error)

Read reads one record (a slice of fields) from r. If the record has an unexpected number of fields, Read returns the record along with the error ErrFieldCount. Except for that case, Read always returns either a non-nil record or a non-nil error, but not both. If there is no data left to be read, Read returns nil, io.EOF.
func (*Reader) ReadAll

func (r *Reader) ReadAll() (records [][]string, err error)

ReadAll reads all the remaining records from r. Each record is a slice of fields. A successful call returns err == nil, not err == io.EOF. Because ReadAll is defined to read until EOF, it does not treat end of file as an error to be reported.

▹ Example
type Writer

A Writer writes records to a CSV encoded file.

As returned by NewWriter, a Writer writes records terminated by a newline and uses ',' as the field delimiter. The exported fields can be changed to customize the details before the first call to Write or WriteAll.

Comma is the field delimiter.

If UseCRLF is true, the Writer ends each record with \r\n instead of \n.

type Writer struct {
        Comma   rune // Field delimiter (set to ',' by NewWriter)
        UseCRLF bool // True to use \r\n as the line terminator
        // contains filtered or unexported fields
}

▹ Example
func NewWriter

func NewWriter(w io.Writer) *Writer

NewWriter returns a new Writer that writes to w.
func (*Writer) Error

func (w *Writer) Error() error

Error reports any error that has occurred during a previous Write or Flush.
func (*Writer) Flush

func (w *Writer) Flush()

Flush writes any buffered data to the underlying io.Writer. To check if an error occurred during the Flush, call Error.
func (*Writer) Write

func (w *Writer) Write(record []string) error

Writer writes a single CSV record to w along with any necessary quoting. A record is a slice of strings with each string being one field.
func (*Writer) WriteAll

func (w *Writer) WriteAll(records [][]string) error

WriteAll writes multiple CSV records to w using Write and then calls Flush.

▹ Example
