
# Το πακέτο tar

```golang
    import "archive/tar"
```

Περιεχόμενα:
* [Γενικά](#info)
* [Σταθερες](#const)
* [Μεταβλητές](#variables)
* [Συναρτήσεις - Μέθοδοι](#funcs)
* [Παραδείγματα](#examples)


### <a name="info"></a>Γενικά

Το πακέτο tar υλοποιεί την προσβαση σε συμπιεσμενα tar αρχεία. Στοχεύει να καλύψει όλες τις παραλλαγές, συμπεριλαμβανομένων και αυτών που παράγονται από τα GNU και BSD tars.  

Παραπομπές:

http://www.freebsd.org/cgi/man.cgi?query=tar&sektion=5
http://www.gnu.org/software/tar/manual/html_node/Standard.html
http://pubs.opengroup.org/onlinepubs/9699919799/utilities/pax.html

Παρδειγμα:
```golang
package main

import (
	"archive/tar"
	"bytes"
	"fmt"
	"io"
	"log"
	"os"
)

func main() {
	// Create a buffer to write our archive to.
	buf := new(bytes.Buffer)

	// Create a new tar archive.
	tw := tar.NewWriter(buf)

	// Add some files to the archive.
	var files = []struct {
		Name, Body string
	}{
		{"readme.txt", "This archive contains some text files."},
		{"gopher.txt", "Gopher names:\nGeorge\nGeoffrey\nGonzo"},
		{"todo.txt", "Get animal handling license."},
	}
	for _, file := range files {
		hdr := &tar.Header{
			Name: file.Name,
			Mode: 0600,
			Size: int64(len(file.Body)),
		}
		if err := tw.WriteHeader(hdr); err != nil {
			log.Fatalln(err)
		}
		if _, err := tw.Write([]byte(file.Body)); err != nil {
			log.Fatalln(err)
		}
	}
	// Make sure to check the error on Close.
	if err := tw.Close(); err != nil {
		log.Fatalln(err)
	}

	// Open the tar archive for reading.
	r := bytes.NewReader(buf.Bytes())
	tr := tar.NewReader(r)

	// Iterate through the files in the archive.
	for {
		hdr, err := tr.Next()
		if err == io.EOF {
			// end of tar archive
			break
		}
		if err != nil {
			log.Fatalln(err)
		}
		fmt.Printf("Contents of %s:\n", hdr.Name)
		if _, err := io.Copy(os.Stdout, tr); err != nil {
			log.Fatalln(err)
		}
		fmt.Println()
	}

}
```

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

### <a name="const"></a>Σταθερές (Constants)

Σημαίες τύπου κεφαλίδας.

```golang
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
```

### <a name="variables"></a>Μεταβλητές

```golang
var (
        ErrWriteTooLong    = errors.New("archive/tar: write too long")
        ErrFieldTooLong    = errors.New("archive/tar: header field too long")
        ErrWriteAfterClose = errors.New("archive/tar: write after close")
)
```
```golang
var (
        ErrHeader = errors.New("archive/tar: invalid tar header")
)
```
### <a name="funcs"></a> Οι ενσωματωμένες Συναρτήσεις - Μέθοδοι
### type Header

Ο Header αντιπροσωπεύει μια μοναδική κεφαλίδα στο tar αρχείο. Μερικά πεδία μπορεί να μην έχουν συμπληρωθεί. 

```golang
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
```

* **func FileInfoHeader**

```golang
func FileInfoHeader(fi os.FileInfo, link string) (*Header, error)
```

FileInfoHeader creates a partially-populated Header from fi. If fi describes a symlink, FileInfoHeader records link as the link target. If fi describes a directory, a slash is appended to the name. Because os.FileInfo's Name method returns only the base name of the file it describes, it may be necessary to modify the Name field of the returned header to provide the full path name of the file.

* **func (*Header) FileInfo**

```golang
func (h *Header) FileInfo() os.FileInfo
```

FileInfo returns an os.FileInfo for the Header.

### type Reader

A Reader provides sequential access to the contents of a tar archive. A tar archive consists of a sequence of files. The Next method advances to the next file in the archive (including the first), and then it can be treated as an io.Reader to access the file's data.

```golang
type Reader struct {
        // contains filtered or unexported fields
}
```
* **func NewReader**

```golang
func NewReader(r io.Reader) *Reader
```

NewReader creates a new Reader reading from r.

* **func (*Reader) Next**

```golang
func (tr *Reader) Next() (*Header, error)
```

Next advances to the next entry in the tar archive.

io.EOF is returned at the end of the input.
func (*Reader) Read

```golang
func (tr *Reader) Read(b []byte) (int, error)
```

Read reads from the current entry in the tar archive. It returns 0, io.EOF when it reaches the end of that entry, until Next is called to advance to the next entry.

Calling Read on special types like TypeLink, TypeSymLink, TypeChar, TypeBlock, TypeDir, and TypeFifo returns 0, io.EOF regardless of what the Header.Size claims.
type Writer

A Writer provides sequential writing of a tar archive in POSIX.1 format. A tar archive consists of a sequence of files. Call WriteHeader to begin a new file, and then call Write to supply that file's data, writing at most hdr.Size bytes in total.

```golang
type Writer struct {
        // contains filtered or unexported fields
}
```

* **func NewWriter**

```golang
func NewWriter(w io.Writer) *Writer
```

NewWriter creates a new Writer writing to w.

* **func (*Writer) Close**

```golang
func (tw *Writer) Close() error
```

Close closes the tar archive, flushing any unwritten data to the underlying writer.

* **func (*Writer) Flush**

```golang
func (tw *Writer) Flush() error
```

Flush finishes writing the current file (optional).

* **func (*Writer) Write**

```golang
func (tw *Writer) Write(b []byte) (n int, err error)
```

Write writes to the current entry in the tar archive. Write returns the error ErrWriteTooLong if more than hdr.Size bytes are written after WriteHeader.

* **func (*Writer) WriteHeader**

```golang
func (tw *Writer) WriteHeader(hdr *Header) error
```

WriteHeader writes hdr and prepares to accept the file's contents. WriteHeader calls Flush if it is not the first header. Calling after a Close will return ErrWriteAfterClose. 
