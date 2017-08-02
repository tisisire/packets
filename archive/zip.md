
 Package zip

    import "archive/zip"

    Overview
    Index
    Examples

Overview ▾

Package zip provides support for reading and writing ZIP archives.

See: https://www.pkware.com/documents/casestudies/APPNOTE.TXT

This package does not support disk spanning.

A note about ZIP64:

To be backwards compatible the FileHeader has both 32 and 64 bit Size fields. The 64 bit fields will always contain the correct value and for normal archives both fields will be the same. For files requiring the ZIP64 format the 32 bit fields will be 0xffffffff and the 64 bit fields must be used instead.
Index ▾

    Constants
    Variables
    func RegisterCompressor(method uint16, comp Compressor)
    func RegisterDecompressor(method uint16, dcomp Decompressor)
    type Compressor
    type Decompressor
    type File
        func (f *File) DataOffset() (offset int64, err error)
        func (f *File) Open() (io.ReadCloser, error)
    type FileHeader
        func FileInfoHeader(fi os.FileInfo) (*FileHeader, error)
        func (h *FileHeader) FileInfo() os.FileInfo
        func (h *FileHeader) ModTime() time.Time
        func (h *FileHeader) Mode() (mode os.FileMode)
        func (h *FileHeader) SetModTime(t time.Time)
        func (h *FileHeader) SetMode(mode os.FileMode)
    type ReadCloser
        func OpenReader(name string) (*ReadCloser, error)
        func (rc *ReadCloser) Close() error
    type Reader
        func NewReader(r io.ReaderAt, size int64) (*Reader, error)
        func (z *Reader) RegisterDecompressor(method uint16, dcomp Decompressor)
    type Writer
        func NewWriter(w io.Writer) *Writer
        func (w *Writer) Close() error
        func (w *Writer) Create(name string) (io.Writer, error)
        func (w *Writer) CreateHeader(fh *FileHeader) (io.Writer, error)
        func (w *Writer) Flush() error
        func (w *Writer) RegisterCompressor(method uint16, comp Compressor)
        func (w *Writer) SetOffset(n int64)

Examples

    Reader
    Writer
    Writer.RegisterCompressor

Package files

reader.go register.go struct.go writer.go
Constants

Compression methods.

const (
        Store   uint16 = 0
        Deflate uint16 = 8
)

Variables

var (
        ErrFormat    = errors.New("zip: not a valid zip file")
        ErrAlgorithm = errors.New("zip: unsupported compression algorithm")
        ErrChecksum  = errors.New("zip: checksum error")
)

func RegisterCompressor

func RegisterCompressor(method uint16, comp Compressor)

RegisterCompressor registers custom compressors for a specified method ID. The common methods Store and Deflate are built in.
func RegisterDecompressor

func RegisterDecompressor(method uint16, dcomp Decompressor)

RegisterDecompressor allows custom decompressors for a specified method ID. The common methods Store and Deflate are built in.
type Compressor

A Compressor returns a new compressing writer, writing to w. The WriteCloser's Close method must be used to flush pending data to w. The Compressor itself must be safe to invoke from multiple goroutines simultaneously, but each returned writer will be used only by one goroutine at a time.

type Compressor func(w io.Writer) (io.WriteCloser, error)

type Decompressor

A Decompressor returns a new decompressing reader, reading from r. The ReadCloser's Close method must be used to release associated resources. The Decompressor itself must be safe to invoke from multiple goroutines simultaneously, but each returned reader will be used only by one goroutine at a time.

type Decompressor func(r io.Reader) io.ReadCloser

type File

type File struct {
        FileHeader
        // contains filtered or unexported fields
}

func (*File) DataOffset

func (f *File) DataOffset() (offset int64, err error)

DataOffset returns the offset of the file's possibly-compressed data, relative to the beginning of the zip file.

Most callers should instead use Open, which transparently decompresses data and verifies checksums.
func (*File) Open

func (f *File) Open() (io.ReadCloser, error)

Open returns a ReadCloser that provides access to the File's contents. Multiple files may be read concurrently.
type FileHeader

FileHeader describes a file within a zip file. See the zip spec for details.

type FileHeader struct {
        // Name is the name of the file.
        // It must be a relative path: it must not start with a drive
        // letter (e.g. C:) or leading slash, and only forward slashes
        // are allowed.
        Name string

        CreatorVersion     uint16
        ReaderVersion      uint16
        Flags              uint16
        Method             uint16
        ModifiedTime       uint16 // MS-DOS time
        ModifiedDate       uint16 // MS-DOS date
        CRC32              uint32
        CompressedSize     uint32 // Deprecated: Use CompressedSize64 instead.
        UncompressedSize   uint32 // Deprecated: Use UncompressedSize64 instead.
        CompressedSize64   uint64
        UncompressedSize64 uint64
        Extra              []byte
        ExternalAttrs      uint32 // Meaning depends on CreatorVersion
        Comment            string
}

func FileInfoHeader

func FileInfoHeader(fi os.FileInfo) (*FileHeader, error)

FileInfoHeader creates a partially-populated FileHeader from an os.FileInfo. Because os.FileInfo's Name method returns only the base name of the file it describes, it may be necessary to modify the Name field of the returned header to provide the full path name of the file.
func (*FileHeader) FileInfo

func (h *FileHeader) FileInfo() os.FileInfo

FileInfo returns an os.FileInfo for the FileHeader.
func (*FileHeader) ModTime

func (h *FileHeader) ModTime() time.Time

ModTime returns the modification time in UTC. The resolution is 2s.
func (*FileHeader) Mode

func (h *FileHeader) Mode() (mode os.FileMode)

Mode returns the permission and mode bits for the FileHeader.
func (*FileHeader) SetModTime

func (h *FileHeader) SetModTime(t time.Time)

SetModTime sets the ModifiedTime and ModifiedDate fields to the given time in UTC. The resolution is 2s.
func (*FileHeader) SetMode

func (h *FileHeader) SetMode(mode os.FileMode)

SetMode changes the permission and mode bits for the FileHeader.
type ReadCloser

type ReadCloser struct {
        Reader
        // contains filtered or unexported fields
}

func OpenReader

func OpenReader(name string) (*ReadCloser, error)

OpenReader will open the Zip file specified by name and return a ReadCloser.
func (*ReadCloser) Close

func (rc *ReadCloser) Close() error

Close closes the Zip file, rendering it unusable for I/O.
type Reader

type Reader struct {
        File    []*File
        Comment string
        // contains filtered or unexported fields
}

▹ Example
func NewReader

func NewReader(r io.ReaderAt, size int64) (*Reader, error)

NewReader returns a new Reader reading from r, which is assumed to have the given size in bytes.
func (*Reader) RegisterDecompressor

func (z *Reader) RegisterDecompressor(method uint16, dcomp Decompressor)

RegisterDecompressor registers or overrides a custom decompressor for a specific method ID. If a decompressor for a given method is not found, Reader will default to looking up the decompressor at the package level.
type Writer

Writer implements a zip file writer.

type Writer struct {
        // contains filtered or unexported fields
}

▹ Example
func NewWriter

func NewWriter(w io.Writer) *Writer

NewWriter returns a new Writer writing a zip file to w.
func (*Writer) Close

func (w *Writer) Close() error

Close finishes writing the zip file by writing the central directory. It does not (and cannot) close the underlying writer.
func (*Writer) Create

func (w *Writer) Create(name string) (io.Writer, error)

Create adds a file to the zip file using the provided name. It returns a Writer to which the file contents should be written. The name must be a relative path: it must not start with a drive letter (e.g. C:) or leading slash, and only forward slashes are allowed. The file's contents must be written to the io.Writer before the next call to Create, CreateHeader, or Close.
func (*Writer) CreateHeader

func (w *Writer) CreateHeader(fh *FileHeader) (io.Writer, error)

CreateHeader adds a file to the zip file using the provided FileHeader for the file metadata. It returns a Writer to which the file contents should be written.

The file's contents must be written to the io.Writer before the next call to Create, CreateHeader, or Close. The provided FileHeader fh must not be modified after a call to CreateHeader.
func (*Writer) Flush

func (w *Writer) Flush() error

Flush flushes any buffered data to the underlying writer. Calling Flush is not normally necessary; calling Close is sufficient.
func (*Writer) RegisterCompressor

func (w *Writer) RegisterCompressor(method uint16, comp Compressor)

RegisterCompressor registers or overrides a custom compressor for a specific method ID. If a compressor for a given method is not found, Writer will default to looking up the compressor at the package level.

▹ Example
func (*Writer) SetOffset

func (w *Writer) SetOffset(n int64)

SetOffset sets the offset of the beginning of the zip data within the underlying writer. It should be used when the zip data is appended to an existing file, such as a binary executable. It must be called before any data is written. 
