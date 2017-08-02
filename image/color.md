
 Package color

    import "image/color"

    Overview
    Index
    Subdirectories

Overview ▾

Package color implements a basic color library.
Index ▾

    Variables
    func CMYKToRGB(c, m, y, k uint8) (uint8, uint8, uint8)
    func RGBToCMYK(r, g, b uint8) (uint8, uint8, uint8, uint8)
    func RGBToYCbCr(r, g, b uint8) (uint8, uint8, uint8)
    func YCbCrToRGB(y, cb, cr uint8) (uint8, uint8, uint8)
    type Alpha
        func (c Alpha) RGBA() (r, g, b, a uint32)
    type Alpha16
        func (c Alpha16) RGBA() (r, g, b, a uint32)
    type CMYK
        func (c CMYK) RGBA() (uint32, uint32, uint32, uint32)
    type Color
    type Gray
        func (c Gray) RGBA() (r, g, b, a uint32)
    type Gray16
        func (c Gray16) RGBA() (r, g, b, a uint32)
    type Model
        func ModelFunc(f func(Color) Color) Model
    type NRGBA
        func (c NRGBA) RGBA() (r, g, b, a uint32)
    type NRGBA64
        func (c NRGBA64) RGBA() (r, g, b, a uint32)
    type NYCbCrA
        func (c NYCbCrA) RGBA() (uint32, uint32, uint32, uint32)
    type Palette
        func (p Palette) Convert(c Color) Color
        func (p Palette) Index(c Color) int
    type RGBA
        func (c RGBA) RGBA() (r, g, b, a uint32)
    type RGBA64
        func (c RGBA64) RGBA() (r, g, b, a uint32)
    type YCbCr
        func (c YCbCr) RGBA() (uint32, uint32, uint32, uint32)

Package files

color.go ycbcr.go
Variables

Standard colors.

var (
        Black       = Gray16{0}
        White       = Gray16{0xffff}
        Transparent = Alpha16{0}
        Opaque      = Alpha16{0xffff}
)

func CMYKToRGB

func CMYKToRGB(c, m, y, k uint8) (uint8, uint8, uint8)

CMYKToRGB converts a CMYK quadruple to an RGB triple.
func RGBToCMYK

func RGBToCMYK(r, g, b uint8) (uint8, uint8, uint8, uint8)

RGBToCMYK converts an RGB triple to a CMYK quadruple.
func RGBToYCbCr

func RGBToYCbCr(r, g, b uint8) (uint8, uint8, uint8)

RGBToYCbCr converts an RGB triple to a Y'CbCr triple.
func YCbCrToRGB

func YCbCrToRGB(y, cb, cr uint8) (uint8, uint8, uint8)

YCbCrToRGB converts a Y'CbCr triple to an RGB triple.
type Alpha

Alpha represents an 8-bit alpha color.

type Alpha struct {
        A uint8
}

func (Alpha) RGBA

func (c Alpha) RGBA() (r, g, b, a uint32)

type Alpha16

Alpha16 represents a 16-bit alpha color.

type Alpha16 struct {
        A uint16
}

func (Alpha16) RGBA

func (c Alpha16) RGBA() (r, g, b, a uint32)

type CMYK

CMYK represents a fully opaque CMYK color, having 8 bits for each of cyan, magenta, yellow and black.

It is not associated with any particular color profile.

type CMYK struct {
        C, M, Y, K uint8
}

func (CMYK) RGBA

func (c CMYK) RGBA() (uint32, uint32, uint32, uint32)

type Color

Color can convert itself to alpha-premultiplied 16-bits per channel RGBA. The conversion may be lossy.

type Color interface {
        // RGBA returns the alpha-premultiplied red, green, blue and alpha values
        // for the color. Each value ranges within [0, 0xffff], but is represented
        // by a uint32 so that multiplying by a blend factor up to 0xffff will not
        // overflow.
        //
        // An alpha-premultiplied color component c has been scaled by alpha (a),
        // so has valid values 0 <= c <= a.
        RGBA() (r, g, b, a uint32)
}

type Gray

Gray represents an 8-bit grayscale color.

type Gray struct {
        Y uint8
}

func (Gray) RGBA

func (c Gray) RGBA() (r, g, b, a uint32)

type Gray16

Gray16 represents a 16-bit grayscale color.

type Gray16 struct {
        Y uint16
}

func (Gray16) RGBA

func (c Gray16) RGBA() (r, g, b, a uint32)

type Model

Model can convert any Color to one from its own color model. The conversion may be lossy.

type Model interface {
        Convert(c Color) Color
}

Models for the standard color types.

var (
        RGBAModel    Model = ModelFunc(rgbaModel)
        RGBA64Model  Model = ModelFunc(rgba64Model)
        NRGBAModel   Model = ModelFunc(nrgbaModel)
        NRGBA64Model Model = ModelFunc(nrgba64Model)
        AlphaModel   Model = ModelFunc(alphaModel)
        Alpha16Model Model = ModelFunc(alpha16Model)
        GrayModel    Model = ModelFunc(grayModel)
        Gray16Model  Model = ModelFunc(gray16Model)
)

CMYKModel is the Model for CMYK colors.

var CMYKModel Model = ModelFunc(cmykModel)

NYCbCrAModel is the Model for non-alpha-premultiplied Y'CbCr-with-alpha colors.

var NYCbCrAModel Model = ModelFunc(nYCbCrAModel)

YCbCrModel is the Model for Y'CbCr colors.

var YCbCrModel Model = ModelFunc(yCbCrModel)

func ModelFunc

func ModelFunc(f func(Color) Color) Model

ModelFunc returns a Model that invokes f to implement the conversion.
type NRGBA

NRGBA represents a non-alpha-premultiplied 32-bit color.

type NRGBA struct {
        R, G, B, A uint8
}

func (NRGBA) RGBA

func (c NRGBA) RGBA() (r, g, b, a uint32)

type NRGBA64

NRGBA64 represents a non-alpha-premultiplied 64-bit color, having 16 bits for each of red, green, blue and alpha.

type NRGBA64 struct {
        R, G, B, A uint16
}

func (NRGBA64) RGBA

func (c NRGBA64) RGBA() (r, g, b, a uint32)

type NYCbCrA

NYCbCrA represents a non-alpha-premultiplied Y'CbCr-with-alpha color, having 8 bits each for one luma, two chroma and one alpha component.

type NYCbCrA struct {
        YCbCr
        A uint8
}

func (NYCbCrA) RGBA

func (c NYCbCrA) RGBA() (uint32, uint32, uint32, uint32)

type Palette

Palette is a palette of colors.

type Palette []Color

func (Palette) Convert

func (p Palette) Convert(c Color) Color

Convert returns the palette color closest to c in Euclidean R,G,B space.
func (Palette) Index

func (p Palette) Index(c Color) int

Index returns the index of the palette color closest to c in Euclidean R,G,B,A space.
type RGBA

RGBA represents a traditional 32-bit alpha-premultiplied color, having 8 bits for each of red, green, blue and alpha.

An alpha-premultiplied color component C has been scaled by alpha (A), so has valid values 0 <= C <= A.

type RGBA struct {
        R, G, B, A uint8
}

func (RGBA) RGBA

func (c RGBA) RGBA() (r, g, b, a uint32)

type RGBA64

RGBA64 represents a 64-bit alpha-premultiplied color, having 16 bits for each of red, green, blue and alpha.

An alpha-premultiplied color component C has been scaled by alpha (A), so has valid values 0 <= C <= A.

type RGBA64 struct {
        R, G, B, A uint16
}

func (RGBA64) RGBA

func (c RGBA64) RGBA() (r, g, b, a uint32)

type YCbCr

YCbCr represents a fully opaque 24-bit Y'CbCr color, having 8 bits each for one luma and two chroma components.

JPEG, VP8, the MPEG family and other codecs use this color model. Such codecs often use the terms YUV and Y'CbCr interchangeably, but strictly speaking, the term YUV applies only to analog video signals, and Y' (luma) is Y (luminance) after applying gamma correction.

Conversion between RGB and Y'CbCr is lossy and there are multiple, slightly different formulae for converting between the two. This package follows the JFIF specification at http://www.w3.org/Graphics/JPEG/jfif3.pdf.

type YCbCr struct {
        Y, Cb, Cr uint8
}

func (YCbCr) RGBA

func (c YCbCr) RGBA() (uint32, uint32, uint32, uint32)
