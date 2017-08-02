
 Package base32

    import "encoding/base32"

    Overview
    Index
    Examples

Overview ▾

Package base32 implements base32 encoding as specified by RFC 4648.
Index ▾

    Variables
    func NewDecoder(enc *Encoding, r io.Reader) io.Reader
    func NewEncoder(enc *Encoding, w io.Writer) io.WriteCloser
    type CorruptInputError
        func (e CorruptInputError) Error() string
    type Encoding
        func NewEncoding(encoder string) *Encoding
        func (enc *Encoding) Decode(dst, src []byte) (n int, err error)
        func (enc *Encoding) DecodeString(s string) ([]byte, error)
        func (enc *Encoding) DecodedLen(n int) int
        func (enc *Encoding) Encode(dst, src []byte)
        func (enc *Encoding) EncodeToString(src []byte) string
        func (enc *Encoding) EncodedLen(n int) int

Examples

    Encoding.DecodeString
    Encoding.EncodeToString
    NewEncoder

Package files

base32.go
Variables

HexEncoding is the “Extended Hex Alphabet” defined in RFC 4648. It is typically used in DNS.

var HexEncoding = NewEncoding(encodeHex)

StdEncoding is the standard base32 encoding, as defined in RFC 4648.

var StdEncoding = NewEncoding(encodeStd)

func NewDecoder

func NewDecoder(enc *Encoding, r io.Reader) io.Reader

NewDecoder constructs a new base32 stream decoder.
func NewEncoder

func NewEncoder(enc *Encoding, w io.Writer) io.WriteCloser

NewEncoder returns a new base32 stream encoder. Data written to the returned writer will be encoded using enc and then written to w. Base32 encodings operate in 5-byte blocks; when finished writing, the caller must Close the returned encoder to flush any partially written blocks.

▹ Example
type CorruptInputError

type CorruptInputError int64

func (CorruptInputError) Error

func (e CorruptInputError) Error() string

type Encoding

An Encoding is a radix 32 encoding/decoding scheme, defined by a 32-character alphabet. The most common is the "base32" encoding introduced for SASL GSSAPI and standardized in RFC 4648. The alternate "base32hex" encoding is used in DNSSEC.

type Encoding struct {
        // contains filtered or unexported fields
}

func NewEncoding

func NewEncoding(encoder string) *Encoding

NewEncoding returns a new Encoding defined by the given alphabet, which must be a 32-byte string.
func (*Encoding) Decode

func (enc *Encoding) Decode(dst, src []byte) (n int, err error)

Decode decodes src using the encoding enc. It writes at most DecodedLen(len(src)) bytes to dst and returns the number of bytes written. If src contains invalid base32 data, it will return the number of bytes successfully written and CorruptInputError. New line characters (\r and \n) are ignored.
func (*Encoding) DecodeString

func (enc *Encoding) DecodeString(s string) ([]byte, error)

DecodeString returns the bytes represented by the base32 string s.

▹ Example
func (*Encoding) DecodedLen

func (enc *Encoding) DecodedLen(n int) int

DecodedLen returns the maximum length in bytes of the decoded data corresponding to n bytes of base32-encoded data.
func (*Encoding) Encode

func (enc *Encoding) Encode(dst, src []byte)

Encode encodes src using the encoding enc, writing EncodedLen(len(src)) bytes to dst.

The encoding pads the output to a multiple of 8 bytes, so Encode is not appropriate for use on individual blocks of a large data stream. Use NewEncoder() instead.
func (*Encoding) EncodeToString

func (enc *Encoding) EncodeToString(src []byte) string

EncodeToString returns the base32 encoding of src.

▹ Example
func (*Encoding) EncodedLen

func (enc *Encoding) EncodedLen(n int) int

EncodedLen returns the length in bytes of the base32 encoding of an input buffer of length n.
