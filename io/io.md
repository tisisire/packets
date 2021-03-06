
 Package io

    import "io"

    Overview
    Index
    Examples
    Subdirectories

Overview ▾

Package io provides basic interfaces to I/O primitives. Its primary job is to wrap existing implementations of such primitives, such as those in package os, into shared public interfaces that abstract the functionality, plus some other related primitives.

Because these interfaces and primitives wrap lower-level operations with various implementations, unless otherwise informed clients should not assume they are safe for parallel execution.
Index ▾

    Constants
    Variables
    func Copy(dst Writer, src Reader) (written int64, err error)
    func CopyBuffer(dst Writer, src Reader, buf []byte) (written int64, err error)
    func CopyN(dst Writer, src Reader, n int64) (written int64, err error)
    func ReadAtLeast(r Reader, buf []byte, min int) (n int, err error)
    func ReadFull(r Reader, buf []byte) (n int, err error)
    func WriteString(w Writer, s string) (n int, err error)
    type ByteReader
    type ByteScanner
    type ByteWriter
    type Closer
    type LimitedReader
        func (l *LimitedReader) Read(p []byte) (n int, err error)
    type PipeReader
        func Pipe() (*PipeReader, *PipeWriter)
        func (r *PipeReader) Close() error
        func (r *PipeReader) CloseWithError(err error) error
        func (r *PipeReader) Read(data []byte) (n int, err error)
    type PipeWriter
        func (w *PipeWriter) Close() error
        func (w *PipeWriter) CloseWithError(err error) error
        func (w *PipeWriter) Write(data []byte) (n int, err error)
    type ReadCloser
    type ReadSeeker
    type ReadWriteCloser
    type ReadWriteSeeker
    type ReadWriter
    type Reader
        func LimitReader(r Reader, n int64) Reader
        func MultiReader(readers ...Reader) Reader
        func TeeReader(r Reader, w Writer) Reader
    type ReaderAt
    type ReaderFrom
    type RuneReader
    type RuneScanner
    type SectionReader
        func NewSectionReader(r ReaderAt, off int64, n int64) *SectionReader
        func (s *SectionReader) Read(p []byte) (n int, err error)
        func (s *SectionReader) ReadAt(p []byte, off int64) (n int, err error)
        func (s *SectionReader) Seek(offset int64, whence int) (int64, error)
        func (s *SectionReader) Size() int64
    type Seeker
    type WriteCloser
    type WriteSeeker
    type Writer
        func MultiWriter(writers ...Writer) Writer
    type WriterAt
    type WriterTo

Examples

    Copy
    CopyBuffer
    CopyN
    LimitReader
    MultiReader
    MultiWriter
    ReadAtLeast
    ReadFull
    SectionReader
    SectionReader.ReadAt
    SectionReader.Seek
    TeeReader
    WriteString

Package files

io.go multi.go pipe.go
Constants

Seek whence values.

const (
        SeekStart   = 0 // seek relative to the origin of the file
        SeekCurrent = 1 // seek relative to the current offset
        SeekEnd     = 2 // seek relative to the end
)

Variables

EOF is the error returned by Read when no more input is available. Functions should return EOF only to signal a graceful end of input. If the EOF occurs unexpectedly in a structured data stream, the appropriate error is either ErrUnexpectedEOF or some other error giving more detail.

var EOF = errors.New("EOF")

ErrClosedPipe is the error used for read or write operations on a closed pipe.

var ErrClosedPipe = errors.New("io: read/write on closed pipe")

ErrNoProgress is returned by some clients of an io.Reader when many calls to Read have failed to return any data or error, usually the sign of a broken io.Reader implementation.

var ErrNoProgress = errors.New("multiple Read calls return no data or error")

ErrShortBuffer means that a read required a longer buffer than was provided.

var ErrShortBuffer = errors.New("short buffer")

ErrShortWrite means that a write accepted fewer bytes than requested but failed to return an explicit error.

var ErrShortWrite = errors.New("short write")

ErrUnexpectedEOF means that EOF was encountered in the middle of reading a fixed-size block or data structure.

var ErrUnexpectedEOF = errors.New("unexpected EOF")

func Copy

func Copy(dst Writer, src Reader) (written int64, err error)

Copy copies from src to dst until either EOF is reached on src or an error occurs. It returns the number of bytes copied and the first error encountered while copying, if any.

A successful Copy returns err == nil, not err == EOF. Because Copy is defined to read from src until EOF, it does not treat an EOF from Read as an error to be reported.

If src implements the WriterTo interface, the copy is implemented by calling src.WriteTo(dst). Otherwise, if dst implements the ReaderFrom interface, the copy is implemented by calling dst.ReadFrom(src).

▹ Example
func CopyBuffer

func CopyBuffer(dst Writer, src Reader, buf []byte) (written int64, err error)

CopyBuffer is identical to Copy except that it stages through the provided buffer (if one is required) rather than allocating a temporary one. If buf is nil, one is allocated; otherwise if it has zero length, CopyBuffer panics.

▹ Example
func CopyN

func CopyN(dst Writer, src Reader, n int64) (written int64, err error)

CopyN copies n bytes (or until an error) from src to dst. It returns the number of bytes copied and the earliest error encountered while copying. On return, written == n if and only if err == nil.

If dst implements the ReaderFrom interface, the copy is implemented using it.

▹ Example
func ReadAtLeast

func ReadAtLeast(r Reader, buf []byte, min int) (n int, err error)

ReadAtLeast reads from r into buf until it has read at least min bytes. It returns the number of bytes copied and an error if fewer bytes were read. The error is EOF only if no bytes were read. If an EOF happens after reading fewer than min bytes, ReadAtLeast returns ErrUnexpectedEOF. If min is greater than the length of buf, ReadAtLeast returns ErrShortBuffer. On return, n >= min if and only if err == nil.

▹ Example
func ReadFull

func ReadFull(r Reader, buf []byte) (n int, err error)

ReadFull reads exactly len(buf) bytes from r into buf. It returns the number of bytes copied and an error if fewer bytes were read. The error is EOF only if no bytes were read. If an EOF happens after reading some but not all the bytes, ReadFull returns ErrUnexpectedEOF. On return, n == len(buf) if and only if err == nil.

▹ Example
func WriteString

func WriteString(w Writer, s string) (n int, err error)

WriteString writes the contents of the string s to w, which accepts a slice of bytes. If w implements a WriteString method, it is invoked directly. Otherwise, w.Write is called exactly once.

▹ Example
type ByteReader

ByteReader is the interface that wraps the ReadByte method.

ReadByte reads and returns the next byte from the input.

type ByteReader interface {
        ReadByte() (byte, error)
}

type ByteScanner

ByteScanner is the interface that adds the UnreadByte method to the basic ReadByte method.

UnreadByte causes the next call to ReadByte to return the same byte as the previous call to ReadByte. It may be an error to call UnreadByte twice without an intervening call to ReadByte.

type ByteScanner interface {
        ByteReader
        UnreadByte() error
}

type ByteWriter

ByteWriter is the interface that wraps the WriteByte method.

type ByteWriter interface {
        WriteByte(c byte) error
}

type Closer

Closer is the interface that wraps the basic Close method.

The behavior of Close after the first call is undefined. Specific implementations may document their own behavior.

type Closer interface {
        Close() error
}

type LimitedReader

A LimitedReader reads from R but limits the amount of data returned to just N bytes. Each call to Read updates N to reflect the new amount remaining. Read returns EOF when N <= 0 or when the underlying R returns EOF.

type LimitedReader struct {
        R Reader // underlying reader
        N int64  // max bytes remaining
}

func (*LimitedReader) Read

func (l *LimitedReader) Read(p []byte) (n int, err error)

type PipeReader

A PipeReader is the read half of a pipe.

type PipeReader struct {
        // contains filtered or unexported fields
}

func Pipe

func Pipe() (*PipeReader, *PipeWriter)

Pipe creates a synchronous in-memory pipe. It can be used to connect code expecting an io.Reader with code expecting an io.Writer.

Reads and Writes on the pipe are matched one to one except when multiple Reads are needed to consume a single Write. That is, each Write to the PipeWriter blocks until it has satisfied one or more Reads from the PipeReader that fully consume the written data. The data is copied directly from the Write to the corresponding Read (or Reads); there is no internal buffering.

It is safe to call Read and Write in parallel with each other or with Close. Parallel calls to Read and parallel calls to Write are also safe: the individual calls will be gated sequentially.
func (*PipeReader) Close

func (r *PipeReader) Close() error

Close closes the reader; subsequent writes to the write half of the pipe will return the error ErrClosedPipe.
func (*PipeReader) CloseWithError

func (r *PipeReader) CloseWithError(err error) error

CloseWithError closes the reader; subsequent writes to the write half of the pipe will return the error err.
func (*PipeReader) Read

func (r *PipeReader) Read(data []byte) (n int, err error)

Read implements the standard Read interface: it reads data from the pipe, blocking until a writer arrives or the write end is closed. If the write end is closed with an error, that error is returned as err; otherwise err is EOF.
type PipeWriter

A PipeWriter is the write half of a pipe.

type PipeWriter struct {
        // contains filtered or unexported fields
}

func (*PipeWriter) Close

func (w *PipeWriter) Close() error

Close closes the writer; subsequent reads from the read half of the pipe will return no bytes and EOF.
func (*PipeWriter) CloseWithError

func (w *PipeWriter) CloseWithError(err error) error

CloseWithError closes the writer; subsequent reads from the read half of the pipe will return no bytes and the error err, or EOF if err is nil.

CloseWithError always returns nil.
func (*PipeWriter) Write

func (w *PipeWriter) Write(data []byte) (n int, err error)

Write implements the standard Write interface: it writes data to the pipe, blocking until one or more readers have consumed all the data or the read end is closed. If the read end is closed with an error, that err is returned as err; otherwise err is ErrClosedPipe.
type ReadCloser

ReadCloser is the interface that groups the basic Read and Close methods.

type ReadCloser interface {
        Reader
        Closer
}

type ReadSeeker

ReadSeeker is the interface that groups the basic Read and Seek methods.

type ReadSeeker interface {
        Reader
        Seeker
}

type ReadWriteCloser

ReadWriteCloser is the interface that groups the basic Read, Write and Close methods.

type ReadWriteCloser interface {
        Reader
        Writer
        Closer
}

type ReadWriteSeeker

ReadWriteSeeker is the interface that groups the basic Read, Write and Seek methods.

type ReadWriteSeeker interface {
        Reader
        Writer
        Seeker
}

type ReadWriter

ReadWriter is the interface that groups the basic Read and Write methods.

type ReadWriter interface {
        Reader
        Writer
}

type Reader

Reader is the interface that wraps the basic Read method.

Read reads up to len(p) bytes into p. It returns the number of bytes read (0 <= n <= len(p)) and any error encountered. Even if Read returns n < len(p), it may use all of p as scratch space during the call. If some data is available but not len(p) bytes, Read conventionally returns what is available instead of waiting for more.

When Read encounters an error or end-of-file condition after successfully reading n > 0 bytes, it returns the number of bytes read. It may return the (non-nil) error from the same call or return the error (and n == 0) from a subsequent call. An instance of this general case is that a Reader returning a non-zero number of bytes at the end of the input stream may return either err == EOF or err == nil. The next Read should return 0, EOF.

Callers should always process the n > 0 bytes returned before considering the error err. Doing so correctly handles I/O errors that happen after reading some bytes and also both of the allowed EOF behaviors.

Implementations of Read are discouraged from returning a zero byte count with a nil error, except when len(p) == 0. Callers should treat a return of 0 and nil as indicating that nothing happened; in particular it does not indicate EOF.

Implementations must not retain p.

type Reader interface {
        Read(p []byte) (n int, err error)
}

func LimitReader

func LimitReader(r Reader, n int64) Reader

LimitReader returns a Reader that reads from r but stops with EOF after n bytes. The underlying implementation is a *LimitedReader.

▹ Example
func MultiReader

func MultiReader(readers ...Reader) Reader

MultiReader returns a Reader that's the logical concatenation of the provided input readers. They're read sequentially. Once all inputs have returned EOF, Read will return EOF. If any of the readers return a non-nil, non-EOF error, Read will return that error.

▹ Example
func TeeReader

func TeeReader(r Reader, w Writer) Reader

TeeReader returns a Reader that writes to w what it reads from r. All reads from r performed through it are matched with corresponding writes to w. There is no internal buffering - the write must complete before the read completes. Any error encountered while writing is reported as a read error.

▹ Example
type ReaderAt

ReaderAt is the interface that wraps the basic ReadAt method.

ReadAt reads len(p) bytes into p starting at offset off in the underlying input source. It returns the number of bytes read (0 <= n <= len(p)) and any error encountered.

When ReadAt returns n < len(p), it returns a non-nil error explaining why more bytes were not returned. In this respect, ReadAt is stricter than Read.

Even if ReadAt returns n < len(p), it may use all of p as scratch space during the call. If some data is available but not len(p) bytes, ReadAt blocks until either all the data is available or an error occurs. In this respect ReadAt is different from Read.

If the n = len(p) bytes returned by ReadAt are at the end of the input source, ReadAt may return either err == EOF or err == nil.

If ReadAt is reading from an input source with a seek offset, ReadAt should not affect nor be affected by the underlying seek offset.

Clients of ReadAt can execute parallel ReadAt calls on the same input source.

Implementations must not retain p.

type ReaderAt interface {
        ReadAt(p []byte, off int64) (n int, err error)
}

type ReaderFrom

ReaderFrom is the interface that wraps the ReadFrom method.

ReadFrom reads data from r until EOF or error. The return value n is the number of bytes read. Any error except io.EOF encountered during the read is also returned.

The Copy function uses ReaderFrom if available.

type ReaderFrom interface {
        ReadFrom(r Reader) (n int64, err error)
}

type RuneReader

RuneReader is the interface that wraps the ReadRune method.

ReadRune reads a single UTF-8 encoded Unicode character and returns the rune and its size in bytes. If no character is available, err will be set.

type RuneReader interface {
        ReadRune() (r rune, size int, err error)
}

type RuneScanner

RuneScanner is the interface that adds the UnreadRune method to the basic ReadRune method.

UnreadRune causes the next call to ReadRune to return the same rune as the previous call to ReadRune. It may be an error to call UnreadRune twice without an intervening call to ReadRune.

type RuneScanner interface {
        RuneReader
        UnreadRune() error
}

type SectionReader

SectionReader implements Read, Seek, and ReadAt on a section of an underlying ReaderAt.

type SectionReader struct {
        // contains filtered or unexported fields
}

▹ Example
func NewSectionReader

func NewSectionReader(r ReaderAt, off int64, n int64) *SectionReader

NewSectionReader returns a SectionReader that reads from r starting at offset off and stops with EOF after n bytes.
func (*SectionReader) Read

func (s *SectionReader) Read(p []byte) (n int, err error)

func (*SectionReader) ReadAt

func (s *SectionReader) ReadAt(p []byte, off int64) (n int, err error)

▹ Example
func (*SectionReader) Seek

func (s *SectionReader) Seek(offset int64, whence int) (int64, error)

▹ Example
func (*SectionReader) Size

func (s *SectionReader) Size() int64

Size returns the size of the section in bytes.
type Seeker

Seeker is the interface that wraps the basic Seek method.

Seek sets the offset for the next Read or Write to offset, interpreted according to whence: SeekStart means relative to the start of the file, SeekCurrent means relative to the current offset, and SeekEnd means relative to the end. Seek returns the new offset relative to the start of the file and an error, if any.

Seeking to an offset before the start of the file is an error. Seeking to any positive offset is legal, but the behavior of subsequent I/O operations on the underlying object is implementation-dependent.

type Seeker interface {
        Seek(offset int64, whence int) (int64, error)
}

type WriteCloser

WriteCloser is the interface that groups the basic Write and Close methods.

type WriteCloser interface {
        Writer
        Closer
}

type WriteSeeker

WriteSeeker is the interface that groups the basic Write and Seek methods.

type WriteSeeker interface {
        Writer
        Seeker
}

type Writer

Writer is the interface that wraps the basic Write method.

Write writes len(p) bytes from p to the underlying data stream. It returns the number of bytes written from p (0 <= n <= len(p)) and any error encountered that caused the write to stop early. Write must return a non-nil error if it returns n < len(p). Write must not modify the slice data, even temporarily.

Implementations must not retain p.

type Writer interface {
        Write(p []byte) (n int, err error)
}

func MultiWriter

func MultiWriter(writers ...Writer) Writer

MultiWriter creates a writer that duplicates its writes to all the provided writers, similar to the Unix tee(1) command.

▹ Example
type WriterAt

WriterAt is the interface that wraps the basic WriteAt method.

WriteAt writes len(p) bytes from p to the underlying data stream at offset off. It returns the number of bytes written from p (0 <= n <= len(p)) and any error encountered that caused the write to stop early. WriteAt must return a non-nil error if it returns n < len(p).

If WriteAt is writing to a destination with a seek offset, WriteAt should not affect nor be affected by the underlying seek offset.

Clients of WriteAt can execute parallel WriteAt calls on the same destination if the ranges do not overlap.

Implementations must not retain p.

type WriterAt interface {
        WriteAt(p []byte, off int64) (n int, err error)
}

type WriterTo

WriterTo is the interface that wraps the WriteTo method.

WriteTo writes data to w until there's no more data to write or when an error occurs. The return value n is the number of bytes written. Any error encountered during the write is also returned.

The Copy function uses WriterTo if available.

type WriterTo interface {
        WriteTo(w Writer) (n int64, err error)
}
