 # Το πακέτο bzip2

```golang
 import "compress/bzip2"
```
Περιεχόμενα:
* [Γενικά](#info)
* [Σταθερες](#const)
* [Μεταβλητές](#variables)
* [Συναρτήσεις - Μέθοδοι](#funcs)
* [Παραδείγματα](#examples)  


### <a name="info"></a>Γενικά 

Package bzip2 implements bzip2 decompression.
### <a name="funcs"></a>Συναρτήσεις 
* **func NewReader**
```golang
func NewReader(r io.Reader) io.Reader
```
NewReader returns an io.Reader which decompresses bzip2 data from r. If r does not also implement io.ByteReader, the decompressor may read more data than necessary from r.
### type StructuralError

A StructuralError is returned when the bzip2 data is found to be syntactically invalid.
```golang
type StructuralError string
```
* **func (StructuralError) Error**

```golang
func (s StructuralError) Error() string
```
