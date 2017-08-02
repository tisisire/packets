
 Package bzip2

    import "compress/bzip2"

    Overview
    Index

Overview ▾

Package bzip2 implements bzip2 decompression.
Index ▾

    func NewReader(r io.Reader) io.Reader
    type StructuralError
        func (s StructuralError) Error() string

Package files

bit_reader.go bzip2.go huffman.go move_to_front.go
func NewReader

func NewReader(r io.Reader) io.Reader

NewReader returns an io.Reader which decompresses bzip2 data from r. If r does not also implement io.ByteReader, the decompressor may read more data than necessary from r.
type StructuralError

A StructuralError is returned when the bzip2 data is found to be syntactically invalid.

type StructuralError string

func (StructuralError) Error

func (s StructuralError) Error() string
