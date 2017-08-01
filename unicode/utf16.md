
 Package utf16

    import "unicode/utf16"

    Overview
    Index

Overview ▾

Package utf16 implements encoding and decoding of UTF-16 sequences.
Index ▾

    func Decode(s []uint16) []rune
    func DecodeRune(r1, r2 rune) rune
    func Encode(s []rune) []uint16
    func EncodeRune(r rune) (r1, r2 rune)
    func IsSurrogate(r rune) bool

Package files

utf16.go
func Decode

func Decode(s []uint16) []rune

Decode returns the Unicode code point sequence represented by the UTF-16 encoding s.
func DecodeRune

func DecodeRune(r1, r2 rune) rune

DecodeRune returns the UTF-16 decoding of a surrogate pair. If the pair is not a valid UTF-16 surrogate pair, DecodeRune returns the Unicode replacement code point U+FFFD.
func Encode

func Encode(s []rune) []uint16

Encode returns the UTF-16 encoding of the Unicode code point sequence s.
func EncodeRune

func EncodeRune(r rune) (r1, r2 rune)

EncodeRune returns the UTF-16 surrogate pair r1, r2 for the given rune. If the rune is not a valid Unicode code point or does not need encoding, EncodeRune returns U+FFFD, U+FFFD.
func IsSurrogate

func IsSurrogate(r rune) bool

IsSurrogate reports whether the specified Unicode code point can appear in a surrogate pair. 
