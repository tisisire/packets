
 Package mime

    import "mime"

    Overview
    Index
    Examples
    Subdirectories

Overview ▾

Package mime implements parts of the MIME spec.
Index ▾

    Constants
    func AddExtensionType(ext, typ string) error
    func ExtensionsByType(typ string) ([]string, error)
    func FormatMediaType(t string, param map[string]string) string
    func ParseMediaType(v string) (mediatype string, params map[string]string, err error)
    func TypeByExtension(ext string) string
    type WordDecoder
        func (d *WordDecoder) Decode(word string) (string, error)
        func (d *WordDecoder) DecodeHeader(header string) (string, error)
    type WordEncoder
        func (e WordEncoder) Encode(charset, s string) string

Examples

    WordDecoder.Decode
    WordDecoder.DecodeHeader
    WordEncoder.Encode

Package files

encodedword.go grammar.go mediatype.go type.go type_unix.go
Constants

const (
        // BEncoding represents Base64 encoding scheme as defined by RFC 2045.
        BEncoding = WordEncoder('b')
        // QEncoding represents the Q-encoding scheme as defined by RFC 2047.
        QEncoding = WordEncoder('q')
)

func AddExtensionType

func AddExtensionType(ext, typ string) error

AddExtensionType sets the MIME type associated with the extension ext to typ. The extension should begin with a leading dot, as in ".html".
func ExtensionsByType

func ExtensionsByType(typ string) ([]string, error)

ExtensionsByType returns the extensions known to be associated with the MIME type typ. The returned extensions will each begin with a leading dot, as in ".html". When typ has no associated extensions, ExtensionsByType returns an nil slice.
func FormatMediaType

func FormatMediaType(t string, param map[string]string) string

FormatMediaType serializes mediatype t and the parameters param as a media type conforming to RFC 2045 and RFC 2616. The type and parameter names are written in lower-case. When any of the arguments result in a standard violation then FormatMediaType returns the empty string.
func ParseMediaType

func ParseMediaType(v string) (mediatype string, params map[string]string, err error)

ParseMediaType parses a media type value and any optional parameters, per RFC 1521. Media types are the values in Content-Type and Content-Disposition headers (RFC 2183). On success, ParseMediaType returns the media type converted to lowercase and trimmed of white space and a non-nil map. The returned map, params, maps from the lowercase attribute to the attribute value with its case preserved.
func TypeByExtension

func TypeByExtension(ext string) string

TypeByExtension returns the MIME type associated with the file extension ext. The extension ext should begin with a leading dot, as in ".html". When ext has no associated type, TypeByExtension returns "".

Extensions are looked up first case-sensitively, then case-insensitively.

The built-in table is small but on unix it is augmented by the local system's mime.types file(s) if available under one or more of these names:

/etc/mime.types
/etc/apache2/mime.types
/etc/apache/mime.types

On Windows, MIME types are extracted from the registry.

Text types have the charset parameter set to "utf-8" by default.
type WordDecoder

A WordDecoder decodes MIME headers containing RFC 2047 encoded-words.

type WordDecoder struct {
        // CharsetReader, if non-nil, defines a function to generate
        // charset-conversion readers, converting from the provided
        // charset into UTF-8.
        // Charsets are always lower-case. utf-8, iso-8859-1 and us-ascii charsets
        // are handled by default.
        // One of the the CharsetReader's result values must be non-nil.
        CharsetReader func(charset string, input io.Reader) (io.Reader, error)
}

func (*WordDecoder) Decode

func (d *WordDecoder) Decode(word string) (string, error)

Decode decodes an RFC 2047 encoded-word.

▹ Example
func (*WordDecoder) DecodeHeader

func (d *WordDecoder) DecodeHeader(header string) (string, error)

DecodeHeader decodes all encoded-words of the given string. It returns an error if and only if CharsetReader of d returns an error.

▹ Example
type WordEncoder

A WordEncoder is an RFC 2047 encoded-word encoder.

type WordEncoder byte

func (WordEncoder) Encode

func (e WordEncoder) Encode(charset, s string) string

Encode returns the encoded-word form of s. If s is ASCII without special characters, it is returned unchanged. The provided charset is the IANA charset name of s. It is case insensitive.

▹ Example
