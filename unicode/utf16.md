 # Το πακέτο utf16

```golang
 import "unicode/utf16"
```
Περιεχόμενα:
* [Γενικά](#info)
* [Συναρτήσεις - Μέθοδοι](#funcs)

Package utf16 implements encoding and decoding of UTF-16 sequences.

### <a name="funcs"></a>Συναρτήσεις  

* **func Decode**

```golang
func Decode(s []uint16) []rune
```
Decode returns the Unicode code point sequence represented by the UTF-16 encoding s.

* **func DecodeRune**

```golang
func DecodeRune(r1, r2 rune) rune
```
DecodeRune returns the UTF-16 decoding of a surrogate pair. If the pair is not a valid UTF-16 surrogate pair, DecodeRune returns the Unicode replacement code point U+FFFD.

* **func Encode**

```golang
func Encode(s []rune) []uint16
```
Encode returns the UTF-16 encoding of the Unicode code point sequence s.

* **func EncodeRune**
```golang
func EncodeRune(r rune) (r1, r2 rune)
```
EncodeRune returns the UTF-16 surrogate pair r1, r2 for the given rune. If the rune is not a valid Unicode code point or does not need encoding, EncodeRune returns U+FFFD, U+FFFD.

* **func IsSurrogate**

```golang
func IsSurrogate(r rune) bool
```
IsSurrogate reports whether the specified Unicode code point can appear in a surrogate pair. 
