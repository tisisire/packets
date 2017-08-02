
 Package tar

    import "archive/tar"

    Overview
    Index
    Examples

Overview ▾

Package tar implements access to tar archives. It aims to cover most of the variations, including those produced by GNU and BSD tars.

References:

http://www.freebsd.org/cgi/man.cgi?query=tar&sektion=5
http://www.gnu.org/software/tar/manual/html_node/Standard.html
http://pubs.opengroup.org/onlinepubs/9699919799/utilities/pax.html

▹ Example
Index ▾

    Constants
    Variables
    type Header
        func FileInfoHeader(fi os.FileInfo, link string) (*Header, error)
        func (h *Header) FileInfo() os.FileInfo
    type Reader
        func NewReader(r io.Reader) *Reader
        func (tr *Reader) Next() (*Header, error)
        func (tr *Reader) Read(b []byte) (int, error)
    type Writer
        func NewWriter(w io.Writer) *Writer
        func (tw *Writer) Close() error
        func (tw *Writer) Flush() error
        func (tw *Writer) Write(b []byte) (n int, err error)
        func (tw *Writer) WriteHeader(hdr *Header) error

Examples

    Package

Package files

common.go format.go reader.go stat_atim.go stat_unix.go strconv.go writer.go
Constants

Header type flags.

const (
        TypeReg           = '0'    // regular file
        TypeRegA          = '\x00' // regular file
        TypeLink          = '1'    // hard link
        TypeSymlink       = '2'    // symbolic link
        TypeChar          = '3'    // character device node
        TypeBlock         = '4'    // block device node
        TypeDir           = '5'    // directory
        TypeFifo          = '6'    // fifo node
        TypeCont          = '7'    // reserved
        TypeXHeader       = 'x'    // extended header
        TypeXGlobalHeader = 'g'    // global extended header
        TypeGNULongName   = 'L'    // Next file has a long name
        TypeGNULongLink   = 'K'    // Next file symlinks to a file w/ a long name
        TypeGNUSparse     = 'S'    // sparse file
)

Variables

var (
        ErrWriteTooLong    = errors.New("archive/tar: write too long")
        ErrFieldTooLong    = errors.New("archive/tar: header field too long")
        ErrWriteAfterClose = errors.New("archive/tar: write after close")
)

var (
        ErrHeader = errors.New("archive/tar: invalid tar header")
)

type Header

A Header represents a single header in a tar archive. Some fields may not be populated.

type Header struct {
        Name       string    // name of header file entry
        Mode       int64     // permission and mode bits
        Uid        int       // user id of owner
        Gid        int       // group id of owner
        Size       int64     // length in bytes
        ModTime    time.Time // modified time
        Typeflag   byte      // type of header entry
        Linkname   string    // target name of link
        Uname      string    // user name of owner
        Gname      string    // group name of owner
        Devmajor   int64     // major number of character or block device
        Devminor   int64     // minor number of character or block device
        AccessTime time.Time // access time
        ChangeTime time.Time // status change time
        Xattrs     map[string]string
}

func FileInfoHeader

func FileInfoHeader(fi os.FileInfo, link string) (*Header, error)

FileInfoHeader creates a partially-populated Header from fi. If fi describes a symlink, FileInfoHeader records link as the link target. If fi describes a directory, a slash is appended to the name. Because os.FileInfo's Name method returns only the base name of the file it describes, it may be necessary to modify the Name field of the returned header to provide the full path name of the file.
func (*Header) FileInfo

func (h *Header) FileInfo() os.FileInfo

FileInfo returns an os.FileInfo for the Header.
type Reader

A Reader provides sequential access to the contents of a tar archive. A tar archive consists of a sequence of files. The Next method advances to the next file in the archive (including the first), and then it can be treated as an io.Reader to access the file's data.

type Reader struct {
        // contains filtered or unexported fields
}

func NewReader

func NewReader(r io.Reader) *Reader

NewReader creates a new Reader reading from r.
func (*Reader) Next

func (tr *Reader) Next() (*Header, error)

Next advances to the next entry in the tar archive.

io.EOF is returned at the end of the input.
func (*Reader) Read

func (tr *Reader) Read(b []byte) (int, error)

Read reads from the current entry in the tar archive. It returns 0, io.EOF when it reaches the end of that entry, until Next is called to advance to the next entry.

Calling Read on special types like TypeLink, TypeSymLink, TypeChar, TypeBlock, TypeDir, and TypeFifo returns 0, io.EOF regardless of what the Header.Size claims.
type Writer

A Writer provides sequential writing of a tar archive in POSIX.1 format. A tar archive consists of a sequence of files. Call WriteHeader to begin a new file, and then call Write to supply that file's data, writing at most hdr.Size bytes in total.

type Writer struct {
        // contains filtered or unexported fields
}

func NewWriter

func NewWriter(w io.Writer) *Writer

NewWriter creates a new Writer writing to w.
func (*Writer) Close

func (tw *Writer) Close() error

Close closes the tar archive, flushing any unwritten data to the underlying writer.
func (*Writer) Flush

func (tw *Writer) Flush() error

Flush finishes writing the current file (optional).
func (*Writer) Write

func (tw *Writer) Write(b []byte) (n int, err error)

Write writes to the current entry in the tar archive. Write returns the error ErrWriteTooLong if more than hdr.Size bytes are written after WriteHeader.
func (*Writer) WriteHeader

func (tw *Writer) WriteHeader(hdr *Header) error

WriteHeader writes hdr and prepares to accept the file's contents. WriteHeader calls Flush if it is not the first header. Calling after a Close will return ErrWriteAfterClose. 
