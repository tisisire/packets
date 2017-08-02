
 Package lzw

    import "compress/lzw"

    Overview
    Index

Overview ▾

Package lzw implements the Lempel-Ziv-Welch compressed data format, described in T. A. Welch, “A Technique for High-Performance Data Compression”, Computer, 17(6) (June 1984), pp 8-19.

In particular, it implements LZW as used by the GIF and PDF file formats, which means variable-width codes up to 12 bits and the first two non-literal codes are a clear code and an EOF code.

The TIFF file format uses a similar but incompatible version of the LZW algorithm. See the golang.org/x/image/tiff/lzw package for an implementation.
Index ▾

    func NewReader(r io.Reader, order Order, litWidth int) io.ReadCloser
    func NewWriter(w io.Writer, order Order, litWidth int) io.WriteCloser
    type Order

Package files

reader.go writer.go
func NewReader

func NewReader(r io.Reader, order Order, litWidth int) io.ReadCloser

NewReader creates a new io.ReadCloser. Reads from the returned io.ReadCloser read and decompress data from r. If r does not also implement io.ByteReader, the decompressor may read more data than necessary from r. It is the caller's responsibility to call Close on the ReadCloser when finished reading. The number of bits to use for literal codes, litWidth, must be in the range [2,8] and is typically 8. It must equal the litWidth used during compression.
func NewWriter

func NewWriter(w io.Writer, order Order, litWidth int) io.WriteCloser

NewWriter creates a new io.WriteCloser. Writes to the returned io.WriteCloser are compressed and written to w. It is the caller's responsibility to call Close on the WriteCloser when finished writing. The number of bits to use for literal codes, litWidth, must be in the range [2,8] and is typically 8. Input bytes must be less than 1<<litWidth.
type Order

Order specifies the bit ordering in an LZW data stream.

type Order int

const (
        // LSB means Least Significant Bits first, as used in the GIF file format.
        LSB Order = iota
        // MSB means Most Significant Bits first, as used in the TIFF and PDF
        // file formats.
        MSB
)
