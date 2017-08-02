
 Package ioutil

    import "io/ioutil"

    Overview
    Index
    Examples

Overview ▾

Package ioutil implements some I/O utility functions.
Index ▾

    Variables
    func NopCloser(r io.Reader) io.ReadCloser
    func ReadAll(r io.Reader) ([]byte, error)
    func ReadDir(dirname string) ([]os.FileInfo, error)
    func ReadFile(filename string) ([]byte, error)
    func TempDir(dir, prefix string) (name string, err error)
    func TempFile(dir, prefix string) (f *os.File, err error)
    func WriteFile(filename string, data []byte, perm os.FileMode) error

Examples

    ReadAll
    ReadDir
    TempDir
    TempFile

Package files

ioutil.go tempfile.go
Variables

Discard is an io.Writer on which all Write calls succeed without doing anything.

var Discard io.Writer = devNull(0)

func NopCloser

func NopCloser(r io.Reader) io.ReadCloser

NopCloser returns a ReadCloser with a no-op Close method wrapping the provided Reader r.
func ReadAll

func ReadAll(r io.Reader) ([]byte, error)

ReadAll reads from r until an error or EOF and returns the data it read. A successful call returns err == nil, not err == EOF. Because ReadAll is defined to read from src until EOF, it does not treat an EOF from Read as an error to be reported.

▹ Example
func ReadDir

func ReadDir(dirname string) ([]os.FileInfo, error)

ReadDir reads the directory named by dirname and returns a list of directory entries sorted by filename.

▹ Example
func ReadFile

func ReadFile(filename string) ([]byte, error)

ReadFile reads the file named by filename and returns the contents. A successful call returns err == nil, not err == EOF. Because ReadFile reads the whole file, it does not treat an EOF from Read as an error to be reported.
func TempDir

func TempDir(dir, prefix string) (name string, err error)

TempDir creates a new temporary directory in the directory dir with a name beginning with prefix and returns the path of the new directory. If dir is the empty string, TempDir uses the default directory for temporary files (see os.TempDir). Multiple programs calling TempDir simultaneously will not choose the same directory. It is the caller's responsibility to remove the directory when no longer needed.

▹ Example
func TempFile

func TempFile(dir, prefix string) (f *os.File, err error)

TempFile creates a new temporary file in the directory dir with a name beginning with prefix, opens the file for reading and writing, and returns the resulting *os.File. If dir is the empty string, TempFile uses the default directory for temporary files (see os.TempDir). Multiple programs calling TempFile simultaneously will not choose the same file. The caller can use f.Name() to find the pathname of the file. It is the caller's responsibility to remove the file when no longer needed.

▹ Example
func WriteFile

func WriteFile(filename string, data []byte, perm os.FileMode) error

WriteFile writes data to a file named by filename. If the file does not exist, WriteFile creates it with permissions perm; otherwise WriteFile truncates it before writing. 
