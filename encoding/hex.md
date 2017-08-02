
 Package hex

    import "encoding/hex"

    Overview
    Index
    Examples

Overview ▾

Package hex implements hexadecimal encoding and decoding.
Index ▾

    Variables
    func Decode(dst, src []byte) (int, error)
    func DecodeString(s string) ([]byte, error)
    func DecodedLen(x int) int
    func Dump(data []byte) string
    func Dumper(w io.Writer) io.WriteCloser
    func Encode(dst, src []byte) int
    func EncodeToString(src []byte) string
    func EncodedLen(n int) int
    type InvalidByteError
        func (e InvalidByteError) Error() string

Examples

    Decode
    DecodeString
    Dump
    Dumper
    Encode
    EncodeToString

Package files

hex.go
Variables

ErrLength results from decoding an odd length slice.

var ErrLength = errors.New("encoding/hex: odd length hex string")

func Decode

func Decode(dst, src []byte) (int, error)

Decode decodes src into DecodedLen(len(src)) bytes, returning the actual number of bytes written to dst.

Decode expects that src contain only hexadecimal characters and that src should have an even length.

▹ Example
func DecodeString

func DecodeString(s string) ([]byte, error)

DecodeString returns the bytes represented by the hexadecimal string s.

▹ Example
func DecodedLen

func DecodedLen(x int) int

DecodedLen returns the length of a decoding of x source bytes. Specifically, it returns x / 2.
func Dump

func Dump(data []byte) string

Dump returns a string that contains a hex dump of the given data. The format of the hex dump matches the output of `hexdump -C` on the command line.

▹ Example
func Dumper

func Dumper(w io.Writer) io.WriteCloser

Dumper returns a WriteCloser that writes a hex dump of all written data to w. The format of the dump matches the output of `hexdump -C` on the command line.

▹ Example
func Encode

func Encode(dst, src []byte) int

Encode encodes src into EncodedLen(len(src)) bytes of dst. As a convenience, it returns the number of bytes written to dst, but this value is always EncodedLen(len(src)). Encode implements hexadecimal encoding.

▹ Example
func EncodeToString

func EncodeToString(src []byte) string

EncodeToString returns the hexadecimal encoding of src.

▹ Example
func EncodedLen

func EncodedLen(n int) int

EncodedLen returns the length of an encoding of n source bytes. Specifically, it returns n * 2.
type InvalidByteError

InvalidByteError values describe errors resulting from an invalid byte in a hex string.

type InvalidByteError byte

func (InvalidByteError) Error

func (e InvalidByteError) Error() string
