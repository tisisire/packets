
 Package png

    import "image/png"

    Overview
    Index
    Examples

Overview ▾

Package png implements a PNG image decoder and encoder.

The PNG specification is at http://www.w3.org/TR/PNG/.
Index ▾

    func Decode(r io.Reader) (image.Image, error)
    func DecodeConfig(r io.Reader) (image.Config, error)
    func Encode(w io.Writer, m image.Image) error
    type CompressionLevel
    type Encoder
        func (enc *Encoder) Encode(w io.Writer, m image.Image) error
    type FormatError
        func (e FormatError) Error() string
    type UnsupportedError
        func (e UnsupportedError) Error() string

Examples

    Decode
    Encode

Package files

paeth.go reader.go writer.go
func Decode

func Decode(r io.Reader) (image.Image, error)

Decode reads a PNG image from r and returns it as an image.Image. The type of Image returned depends on the PNG contents.

▹ Example
func DecodeConfig

func DecodeConfig(r io.Reader) (image.Config, error)

DecodeConfig returns the color model and dimensions of a PNG image without decoding the entire image.
func Encode

func Encode(w io.Writer, m image.Image) error

Encode writes the Image m to w in PNG format. Any Image may be encoded, but images that are not image.NRGBA might be encoded lossily.

▹ Example
type CompressionLevel

type CompressionLevel int

const (
        DefaultCompression CompressionLevel = 0
        NoCompression      CompressionLevel = -1
        BestSpeed          CompressionLevel = -2
        BestCompression    CompressionLevel = -3
)

type Encoder

Encoder configures encoding PNG images.

type Encoder struct {
        CompressionLevel CompressionLevel
}

func (*Encoder) Encode

func (enc *Encoder) Encode(w io.Writer, m image.Image) error

Encode writes the Image m to w in PNG format.
type FormatError

A FormatError reports that the input is not a valid PNG.

type FormatError string

func (FormatError) Error

func (e FormatError) Error() string

type UnsupportedError

An UnsupportedError reports that the input uses a valid but unimplemented PNG feature.

type UnsupportedError string

func (UnsupportedError) Error

func (e UnsupportedError) Error() string
