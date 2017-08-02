
 Package jpeg

    import "image/jpeg"

    Overview
    Index

Overview ▾

Package jpeg implements a JPEG image decoder and encoder.

JPEG is defined in ITU-T T.81: http://www.w3.org/Graphics/JPEG/itu-t81.pdf.
Index ▾

    Constants
    func Decode(r io.Reader) (image.Image, error)
    func DecodeConfig(r io.Reader) (image.Config, error)
    func Encode(w io.Writer, m image.Image, o *Options) error
    type FormatError
        func (e FormatError) Error() string
    type Options
    type Reader
    type UnsupportedError
        func (e UnsupportedError) Error() string

Package files

fdct.go huffman.go idct.go reader.go scan.go writer.go
Constants

DefaultQuality is the default quality encoding parameter.

const DefaultQuality = 75

func Decode

func Decode(r io.Reader) (image.Image, error)

Decode reads a JPEG image from r and returns it as an image.Image.
func DecodeConfig

func DecodeConfig(r io.Reader) (image.Config, error)

DecodeConfig returns the color model and dimensions of a JPEG image without decoding the entire image.
func Encode

func Encode(w io.Writer, m image.Image, o *Options) error

Encode writes the Image m to w in JPEG 4:2:0 baseline format with the given options. Default parameters are used if a nil *Options is passed.
type FormatError

A FormatError reports that the input is not a valid JPEG.

type FormatError string

func (FormatError) Error

func (e FormatError) Error() string

type Options

Options are the encoding parameters. Quality ranges from 1 to 100 inclusive, higher is better.

type Options struct {
        Quality int
}

type Reader

Deprecated: Reader is deprecated.

type Reader interface {
        io.ByteReader
        io.Reader
}

type UnsupportedError

An UnsupportedError reports that the input uses a valid but unimplemented JPEG feature.

type UnsupportedError string

func (UnsupportedError) Error

func (e UnsupportedError) Error() string
