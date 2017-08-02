
 Package quotedprintable

    import "mime/quotedprintable"

    Overview
    Index
    Examples

Overview ▾

Package quotedprintable implements quoted-printable encoding as specified by RFC 2045.
Index ▾

    type Reader
        func NewReader(r io.Reader) *Reader
        func (r *Reader) Read(p []byte) (n int, err error)
    type Writer
        func NewWriter(w io.Writer) *Writer
        func (w *Writer) Close() error
        func (w *Writer) Write(p []byte) (n int, err error)

Examples

    NewReader
    NewWriter

Package files

reader.go writer.go
type Reader

Reader is a quoted-printable decoder.

type Reader struct {
        // contains filtered or unexported fields
}

func NewReader

func NewReader(r io.Reader) *Reader

NewReader returns a quoted-printable reader, decoding from r.

▹ Example
func (*Reader) Read

func (r *Reader) Read(p []byte) (n int, err error)

Read reads and decodes quoted-printable data from the underlying reader.
type Writer

A Writer is a quoted-printable writer that implements io.WriteCloser.

type Writer struct {
        // Binary mode treats the writer's input as pure binary and processes end of
        // line bytes as binary data.
        Binary bool
        // contains filtered or unexported fields
}

func NewWriter

func NewWriter(w io.Writer) *Writer

NewWriter returns a new Writer that writes to w.

▹ Example
func (*Writer) Close

func (w *Writer) Close() error

Close closes the Writer, flushing any unwritten data to the underlying io.Writer, but does not close the underlying io.Writer.
func (*Writer) Write

func (w *Writer) Write(p []byte) (n int, err error)

Write encodes p using quoted-printable encoding and writes it to the underlying io.Writer. It limits line length to 76 characters. The encoded bytes are not necessarily flushed until the Writer is closed. 
