
 Package draw

    import "image/draw"

    Overview
    Index
    Examples

Overview ▾

Package draw provides image composition functions.

See "The Go image/draw package" for an introduction to this package: https://golang.org/doc/articles/image_draw.html
Index ▾

    func Draw(dst Image, r image.Rectangle, src image.Image, sp image.Point, op Op)
    func DrawMask(dst Image, r image.Rectangle, src image.Image, sp image.Point, mask image.Image, mp image.Point, op Op)
    type Drawer
    type Image
    type Op
        func (op Op) Draw(dst Image, r image.Rectangle, src image.Image, sp image.Point)
    type Quantizer

Examples

    Drawer (FloydSteinberg)

Package files

draw.go
func Draw

func Draw(dst Image, r image.Rectangle, src image.Image, sp image.Point, op Op)

Draw calls DrawMask with a nil mask.
func DrawMask

func DrawMask(dst Image, r image.Rectangle, src image.Image, sp image.Point, mask image.Image, mp image.Point, op Op)

DrawMask aligns r.Min in dst with sp in src and mp in mask and then replaces the rectangle r in dst with the result of a Porter-Duff composition. A nil mask is treated as opaque.
type Drawer

Drawer contains the Draw method.

type Drawer interface {
        // Draw aligns r.Min in dst with sp in src and then replaces the
        // rectangle r in dst with the result of drawing src on dst.
        Draw(dst Image, r image.Rectangle, src image.Image, sp image.Point)
}

FloydSteinberg is a Drawer that is the Src Op with Floyd-Steinberg error diffusion.

var FloydSteinberg Drawer = floydSteinberg{}

▹ Example (FloydSteinberg)
type Image

Image is an image.Image with a Set method to change a single pixel.

type Image interface {
        image.Image
        Set(x, y int, c color.Color)
}

type Op

Op is a Porter-Duff compositing operator.

type Op int

const (
        // Over specifies ``(src in mask) over dst''.
        Over Op = iota
        // Src specifies ``src in mask''.
        Src
)

func (Op) Draw

func (op Op) Draw(dst Image, r image.Rectangle, src image.Image, sp image.Point)

Draw implements the Drawer interface by calling the Draw function with this Op.
type Quantizer

Quantizer produces a palette for an image.

type Quantizer interface {
        // Quantize appends up to cap(p) - len(p) colors to p and returns the
        // updated palette suitable for converting m to a paletted image.
        Quantize(p color.Palette, m image.Image) color.Palette
}
