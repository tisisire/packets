
 Package ascii85

    import "encoding/ascii85"

    Overview
    Index

Overview ▾

Package ascii85 implements the ascii85 data encoding as used in the btoa tool and Adobe's PostScript and PDF document formats.
Index ▾

    func Decode(dst, src []byte, flush bool) (ndst, nsrc int, err error)
    func Encode(dst, src []byte) int
    func MaxEncodedLen(n int) int
    func NewDecoder(r io.Reader) io.Reader
    func NewEncoder(w io.Writer) io.WriteCloser
    type CorruptInputError
        func (e CorruptInputError) Error() string

Package files

ascii85.go
func Decode

func Decode(dst, src []byte, flush bool) (ndst, nsrc int, err error)

Decode decodes src into dst, returning both the number of bytes written to dst and the number consumed from src. If src contains invalid ascii85 data, Decode will return the number of bytes successfully written and a CorruptInputError. Decode ignores space and control characters in src. Often, ascii85-encoded data is wrapped in <~ and ~> symbols. Decode expects these to have been stripped by the caller.

If flush is true, Decode assumes that src represents the end of the input stream and processes it completely rather than wait for the completion of another 32-bit block.

NewDecoder wraps an io.Reader interface around Decode.
func Encode

func Encode(dst, src []byte) int

Encode encodes src into at most MaxEncodedLen(len(src)) bytes of dst, returning the actual number of bytes written.

The encoding handles 4-byte chunks, using a special encoding for the last fragment, so Encode is not appropriate for use on individual blocks of a large data stream. Use NewEncoder() instead.

Often, ascii85-encoded data is wrapped in <~ and ~> symbols. Encode does not add these.
func MaxEncodedLen

func MaxEncodedLen(n int) int

MaxEncodedLen returns the maximum length of an encoding of n source bytes.
func NewDecoder

func NewDecoder(r io.Reader) io.Reader

NewDecoder constructs a new ascii85 stream decoder.
func NewEncoder

func NewEncoder(w io.Writer) io.WriteCloser

NewEncoder returns a new ascii85 stream encoder. Data written to the returned writer will be encoded and then written to w. Ascii85 encodings operate in 32-bit blocks; when finished writing, the caller must Close the returned encoder to flush any trailing partial block.
type CorruptInputError

type CorruptInputError int64

func (CorruptInputError) Error

func (e CorruptInputError) Error() string
