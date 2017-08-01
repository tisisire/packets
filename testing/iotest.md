
 Package iotest

    import "testing/iotest"

    Overview
    Index

Overview ▾

Package iotest implements Readers and Writers useful mainly for testing.
Index ▾

    Variables
    func DataErrReader(r io.Reader) io.Reader
    func HalfReader(r io.Reader) io.Reader
    func NewReadLogger(prefix string, r io.Reader) io.Reader
    func NewWriteLogger(prefix string, w io.Writer) io.Writer
    func OneByteReader(r io.Reader) io.Reader
    func TimeoutReader(r io.Reader) io.Reader
    func TruncateWriter(w io.Writer, n int64) io.Writer

Package files

logger.go reader.go writer.go
Variables

var ErrTimeout = errors.New("timeout")

func DataErrReader

func DataErrReader(r io.Reader) io.Reader

DataErrReader changes the way errors are handled by a Reader. Normally, a Reader returns an error (typically EOF) from the first Read call after the last piece of data is read. DataErrReader wraps a Reader and changes its behavior so the final error is returned along with the final data, instead of in the first call after the final data.
func HalfReader

func HalfReader(r io.Reader) io.Reader

HalfReader returns a Reader that implements Read by reading half as many requested bytes from r.
func NewReadLogger

func NewReadLogger(prefix string, r io.Reader) io.Reader

NewReadLogger returns a reader that behaves like r except that it logs (using log.Print) each read to standard error, printing the prefix and the hexadecimal data read.
func NewWriteLogger

func NewWriteLogger(prefix string, w io.Writer) io.Writer

NewWriteLogger returns a writer that behaves like w except that it logs (using log.Printf) each write to standard error, printing the prefix and the hexadecimal data written.
func OneByteReader

func OneByteReader(r io.Reader) io.Reader

OneByteReader returns a Reader that implements each non-empty Read by reading one byte from r.
func TimeoutReader

func TimeoutReader(r io.Reader) io.Reader

TimeoutReader returns ErrTimeout on the second read with no data. Subsequent calls to read succeed.
func TruncateWriter

func TruncateWriter(w io.Writer, n int64) io.Writer

TruncateWriter returns a Writer that writes to w but stops silently after n bytes. 
