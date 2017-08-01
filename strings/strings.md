
 Package strings

    import "strings"

    Overview
    Index
    Examples

Overview ▾

Package strings implements simple functions to manipulate UTF-8 encoded strings.

For information about UTF-8 strings in Go, see https://blog.golang.org/strings.
Index ▾

    func Compare(a, b string) int
    func Contains(s, substr string) bool
    func ContainsAny(s, chars string) bool
    func ContainsRune(s string, r rune) bool
    func Count(s, sep string) int
    func EqualFold(s, t string) bool
    func Fields(s string) []string
    func FieldsFunc(s string, f func(rune) bool) []string
    func HasPrefix(s, prefix string) bool
    func HasSuffix(s, suffix string) bool
    func Index(s, sep string) int
    func IndexAny(s, chars string) int
    func IndexByte(s string, c byte) int
    func IndexFunc(s string, f func(rune) bool) int
    func IndexRune(s string, r rune) int
    func Join(a []string, sep string) string
    func LastIndex(s, sep string) int
    func LastIndexAny(s, chars string) int
    func LastIndexByte(s string, c byte) int
    func LastIndexFunc(s string, f func(rune) bool) int
    func Map(mapping func(rune) rune, s string) string
    func Repeat(s string, count int) string
    func Replace(s, old, new string, n int) string
    func Split(s, sep string) []string
    func SplitAfter(s, sep string) []string
    func SplitAfterN(s, sep string, n int) []string
    func SplitN(s, sep string, n int) []string
    func Title(s string) string
    func ToLower(s string) string
    func ToLowerSpecial(c unicode.SpecialCase, s string) string
    func ToTitle(s string) string
    func ToTitleSpecial(c unicode.SpecialCase, s string) string
    func ToUpper(s string) string
    func ToUpperSpecial(c unicode.SpecialCase, s string) string
    func Trim(s string, cutset string) string
    func TrimFunc(s string, f func(rune) bool) string
    func TrimLeft(s string, cutset string) string
    func TrimLeftFunc(s string, f func(rune) bool) string
    func TrimPrefix(s, prefix string) string
    func TrimRight(s string, cutset string) string
    func TrimRightFunc(s string, f func(rune) bool) string
    func TrimSpace(s string) string
    func TrimSuffix(s, suffix string) string
    type Reader
        func NewReader(s string) *Reader
        func (r *Reader) Len() int
        func (r *Reader) Read(b []byte) (n int, err error)
        func (r *Reader) ReadAt(b []byte, off int64) (n int, err error)
        func (r *Reader) ReadByte() (byte, error)
        func (r *Reader) ReadRune() (ch rune, size int, err error)
        func (r *Reader) Reset(s string)
        func (r *Reader) Seek(offset int64, whence int) (int64, error)
        func (r *Reader) Size() int64
        func (r *Reader) UnreadByte() error
        func (r *Reader) UnreadRune() error
        func (r *Reader) WriteTo(w io.Writer) (n int64, err error)
    type Replacer
        func NewReplacer(oldnew ...string) *Replacer
        func (r *Replacer) Replace(s string) string
        func (r *Replacer) WriteString(w io.Writer, s string) (n int, err error)
    Bugs

Examples

    Contains
    ContainsAny
    Count
    EqualFold
    Fields
    FieldsFunc
    HasPrefix
    HasSuffix
    Index
    IndexAny
    IndexFunc
    IndexRune
    Join
    LastIndex
    Map
    NewReplacer
    Repeat
    Replace
    Split
    SplitAfter
    SplitAfterN
    SplitN
    Title
    ToLower
    ToTitle
    ToUpper
    Trim
    TrimPrefix
    TrimSpace
    TrimSuffix

Package files

compare.go reader.go replace.go search.go strings.go strings_amd64.go strings_decl.go
func Compare

func Compare(a, b string) int

Compare returns an integer comparing two strings lexicographically. The result will be 0 if a==b, -1 if a < b, and +1 if a > b.

Compare is included only for symmetry with package bytes. It is usually clearer and always faster to use the built-in string comparison operators ==, <, >, and so on.
func Contains

func Contains(s, substr string) bool

Contains reports whether substr is within s.

▹ Example
func ContainsAny

func ContainsAny(s, chars string) bool

ContainsAny reports whether any Unicode code points in chars are within s.

▹ Example
func ContainsRune

func ContainsRune(s string, r rune) bool

ContainsRune reports whether the Unicode code point r is within s.
func Count

func Count(s, sep string) int

Count counts the number of non-overlapping instances of sep in s. If sep is an empty string, Count returns 1 + the number of Unicode code points in s.

▹ Example
func EqualFold

func EqualFold(s, t string) bool

EqualFold reports whether s and t, interpreted as UTF-8 strings, are equal under Unicode case-folding.

▹ Example
func Fields

func Fields(s string) []string

Fields splits the string s around each instance of one or more consecutive white space characters, as defined by unicode.IsSpace, returning an array of substrings of s or an empty list if s contains only white space.

▹ Example
func FieldsFunc

func FieldsFunc(s string, f func(rune) bool) []string

FieldsFunc splits the string s at each run of Unicode code points c satisfying f(c) and returns an array of slices of s. If all code points in s satisfy f(c) or the string is empty, an empty slice is returned. FieldsFunc makes no guarantees about the order in which it calls f(c). If f does not return consistent results for a given c, FieldsFunc may crash.

▹ Example
func HasPrefix

func HasPrefix(s, prefix string) bool

HasPrefix tests whether the string s begins with prefix.

▹ Example
func HasSuffix

func HasSuffix(s, suffix string) bool

HasSuffix tests whether the string s ends with suffix.

▹ Example
func Index

func Index(s, sep string) int

Index returns the index of the first instance of sep in s, or -1 if sep is not present in s.

▹ Example
func IndexAny

func IndexAny(s, chars string) int

IndexAny returns the index of the first instance of any Unicode code point from chars in s, or -1 if no Unicode code point from chars is present in s.

▹ Example
func IndexByte

func IndexByte(s string, c byte) int

IndexByte returns the index of the first instance of c in s, or -1 if c is not present in s.
func IndexFunc

func IndexFunc(s string, f func(rune) bool) int

IndexFunc returns the index into s of the first Unicode code point satisfying f(c), or -1 if none do.

▹ Example
func IndexRune

func IndexRune(s string, r rune) int

IndexRune returns the index of the first instance of the Unicode code point r, or -1 if rune is not present in s. If r is utf8.RuneError, it returns the first instance of any invalid UTF-8 byte sequence.

▹ Example
func Join

func Join(a []string, sep string) string

Join concatenates the elements of a to create a single string. The separator string sep is placed between elements in the resulting string.

▹ Example
func LastIndex

func LastIndex(s, sep string) int

LastIndex returns the index of the last instance of sep in s, or -1 if sep is not present in s.

▹ Example
func LastIndexAny

func LastIndexAny(s, chars string) int

LastIndexAny returns the index of the last instance of any Unicode code point from chars in s, or -1 if no Unicode code point from chars is present in s.
func LastIndexByte

func LastIndexByte(s string, c byte) int

LastIndexByte returns the index of the last instance of c in s, or -1 if c is not present in s.
func LastIndexFunc

func LastIndexFunc(s string, f func(rune) bool) int

LastIndexFunc returns the index into s of the last Unicode code point satisfying f(c), or -1 if none do.
func Map

func Map(mapping func(rune) rune, s string) string

Map returns a copy of the string s with all its characters modified according to the mapping function. If mapping returns a negative value, the character is dropped from the string with no replacement.

▹ Example
func Repeat

func Repeat(s string, count int) string

Repeat returns a new string consisting of count copies of the string s.

It panics if count is negative or if the result of (len(s) * count) overflows.

▹ Example
func Replace

func Replace(s, old, new string, n int) string

Replace returns a copy of the string s with the first n non-overlapping instances of old replaced by new. If old is empty, it matches at the beginning of the string and after each UTF-8 sequence, yielding up to k+1 replacements for a k-rune string. If n < 0, there is no limit on the number of replacements.

▹ Example
func Split

func Split(s, sep string) []string

Split slices s into all substrings separated by sep and returns a slice of the substrings between those separators. If sep is empty, Split splits after each UTF-8 sequence. It is equivalent to SplitN with a count of -1.

▹ Example
func SplitAfter

func SplitAfter(s, sep string) []string

SplitAfter slices s into all substrings after each instance of sep and returns a slice of those substrings. If sep is empty, SplitAfter splits after each UTF-8 sequence. It is equivalent to SplitAfterN with a count of -1.

▹ Example
func SplitAfterN

func SplitAfterN(s, sep string, n int) []string

SplitAfterN slices s into substrings after each instance of sep and returns a slice of those substrings. If sep is empty, SplitAfterN splits after each UTF-8 sequence. The count determines the number of substrings to return:

n > 0: at most n substrings; the last substring will be the unsplit remainder.
n == 0: the result is nil (zero substrings)
n < 0: all substrings

▹ Example
func SplitN

func SplitN(s, sep string, n int) []string

SplitN slices s into substrings separated by sep and returns a slice of the substrings between those separators. If sep is empty, SplitN splits after each UTF-8 sequence. The count determines the number of substrings to return:

n > 0: at most n substrings; the last substring will be the unsplit remainder.
n == 0: the result is nil (zero substrings)
n < 0: all substrings

▹ Example
func Title

func Title(s string) string

Title returns a copy of the string s with all Unicode letters that begin words mapped to their title case.

BUG(rsc): The rule Title uses for word boundaries does not handle Unicode punctuation properly.

▹ Example
func ToLower

func ToLower(s string) string

ToLower returns a copy of the string s with all Unicode letters mapped to their lower case.

▹ Example
func ToLowerSpecial

func ToLowerSpecial(c unicode.SpecialCase, s string) string

ToLowerSpecial returns a copy of the string s with all Unicode letters mapped to their lower case, giving priority to the special casing rules.
func ToTitle

func ToTitle(s string) string

ToTitle returns a copy of the string s with all Unicode letters mapped to their title case.

▹ Example
func ToTitleSpecial

func ToTitleSpecial(c unicode.SpecialCase, s string) string

ToTitleSpecial returns a copy of the string s with all Unicode letters mapped to their title case, giving priority to the special casing rules.
func ToUpper

func ToUpper(s string) string

ToUpper returns a copy of the string s with all Unicode letters mapped to their upper case.

▹ Example
func ToUpperSpecial

func ToUpperSpecial(c unicode.SpecialCase, s string) string

ToUpperSpecial returns a copy of the string s with all Unicode letters mapped to their upper case, giving priority to the special casing rules.
func Trim

func Trim(s string, cutset string) string

Trim returns a slice of the string s with all leading and trailing Unicode code points contained in cutset removed.

▹ Example
func TrimFunc

func TrimFunc(s string, f func(rune) bool) string

TrimFunc returns a slice of the string s with all leading and trailing Unicode code points c satisfying f(c) removed.
func TrimLeft

func TrimLeft(s string, cutset string) string

TrimLeft returns a slice of the string s with all leading Unicode code points contained in cutset removed.
func TrimLeftFunc

func TrimLeftFunc(s string, f func(rune) bool) string

TrimLeftFunc returns a slice of the string s with all leading Unicode code points c satisfying f(c) removed.
func TrimPrefix

func TrimPrefix(s, prefix string) string

TrimPrefix returns s without the provided leading prefix string. If s doesn't start with prefix, s is returned unchanged.

▹ Example
func TrimRight

func TrimRight(s string, cutset string) string

TrimRight returns a slice of the string s, with all trailing Unicode code points contained in cutset removed.
func TrimRightFunc

func TrimRightFunc(s string, f func(rune) bool) string

TrimRightFunc returns a slice of the string s with all trailing Unicode code points c satisfying f(c) removed.
func TrimSpace

func TrimSpace(s string) string

TrimSpace returns a slice of the string s, with all leading and trailing white space removed, as defined by Unicode.

▹ Example
func TrimSuffix

func TrimSuffix(s, suffix string) string

TrimSuffix returns s without the provided trailing suffix string. If s doesn't end with suffix, s is returned unchanged.

▹ Example
type Reader

A Reader implements the io.Reader, io.ReaderAt, io.Seeker, io.WriterTo, io.ByteScanner, and io.RuneScanner interfaces by reading from a string.

type Reader struct {
        // contains filtered or unexported fields
}

func NewReader

func NewReader(s string) *Reader

NewReader returns a new Reader reading from s. It is similar to bytes.NewBufferString but more efficient and read-only.
func (*Reader) Len

func (r *Reader) Len() int

Len returns the number of bytes of the unread portion of the string.
func (*Reader) Read

func (r *Reader) Read(b []byte) (n int, err error)

func (*Reader) ReadAt

func (r *Reader) ReadAt(b []byte, off int64) (n int, err error)

func (*Reader) ReadByte

func (r *Reader) ReadByte() (byte, error)

func (*Reader) ReadRune

func (r *Reader) ReadRune() (ch rune, size int, err error)

func (*Reader) Reset

func (r *Reader) Reset(s string)

Reset resets the Reader to be reading from s.
func (*Reader) Seek

func (r *Reader) Seek(offset int64, whence int) (int64, error)

Seek implements the io.Seeker interface.
func (*Reader) Size

func (r *Reader) Size() int64

Size returns the original length of the underlying string. Size is the number of bytes available for reading via ReadAt. The returned value is always the same and is not affected by calls to any other method.
func (*Reader) UnreadByte

func (r *Reader) UnreadByte() error

func (*Reader) UnreadRune

func (r *Reader) UnreadRune() error

func (*Reader) WriteTo

func (r *Reader) WriteTo(w io.Writer) (n int64, err error)

WriteTo implements the io.WriterTo interface.
type Replacer

Replacer replaces a list of strings with replacements. It is safe for concurrent use by multiple goroutines.

type Replacer struct {
        // contains filtered or unexported fields
}

func NewReplacer

func NewReplacer(oldnew ...string) *Replacer

NewReplacer returns a new Replacer from a list of old, new string pairs. Replacements are performed in order, without overlapping matches.

▹ Example
func (*Replacer) Replace

func (r *Replacer) Replace(s string) string

Replace returns a copy of s with all replacements performed.
func (*Replacer) WriteString

func (r *Replacer) WriteString(w io.Writer, s string) (n int, err error)

WriteString writes s to w with all replacements performed.
Bugs

    ☞

    The rule Title uses for word boundaries does not handle Unicode punctuation properly.
