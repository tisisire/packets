
 Package cmplx

    import "math/cmplx"

    Overview
    Index
    Examples

Overview ▾

Package cmplx provides basic constants and mathematical functions for complex numbers.
Index ▾

    func Abs(x complex128) float64
    func Acos(x complex128) complex128
    func Acosh(x complex128) complex128
    func Asin(x complex128) complex128
    func Asinh(x complex128) complex128
    func Atan(x complex128) complex128
    func Atanh(x complex128) complex128
    func Conj(x complex128) complex128
    func Cos(x complex128) complex128
    func Cosh(x complex128) complex128
    func Cot(x complex128) complex128
    func Exp(x complex128) complex128
    func Inf() complex128
    func IsInf(x complex128) bool
    func IsNaN(x complex128) bool
    func Log(x complex128) complex128
    func Log10(x complex128) complex128
    func NaN() complex128
    func Phase(x complex128) float64
    func Polar(x complex128) (r, θ float64)
    func Pow(x, y complex128) complex128
    func Rect(r, θ float64) complex128
    func Sin(x complex128) complex128
    func Sinh(x complex128) complex128
    func Sqrt(x complex128) complex128
    func Tan(x complex128) complex128
    func Tanh(x complex128) complex128

Examples

    Abs
    Exp
    Polar

Package files

abs.go asin.go conj.go exp.go isinf.go isnan.go log.go phase.go polar.go pow.go rect.go sin.go sqrt.go tan.go
func Abs

func Abs(x complex128) float64

Abs returns the absolute value (also called the modulus) of x.

▹ Example
func Acos

func Acos(x complex128) complex128

Acos returns the inverse cosine of x.
func Acosh

func Acosh(x complex128) complex128

Acosh returns the inverse hyperbolic cosine of x.
func Asin

func Asin(x complex128) complex128

Asin returns the inverse sine of x.
func Asinh

func Asinh(x complex128) complex128

Asinh returns the inverse hyperbolic sine of x.
func Atan

func Atan(x complex128) complex128

Atan returns the inverse tangent of x.
func Atanh

func Atanh(x complex128) complex128

Atanh returns the inverse hyperbolic tangent of x.
func Conj

func Conj(x complex128) complex128

Conj returns the complex conjugate of x.
func Cos

func Cos(x complex128) complex128

Cos returns the cosine of x.
func Cosh

func Cosh(x complex128) complex128

Cosh returns the hyperbolic cosine of x.
func Cot

func Cot(x complex128) complex128

Cot returns the cotangent of x.
func Exp

func Exp(x complex128) complex128

Exp returns e**x, the base-e exponential of x.

▹ Example
func Inf

func Inf() complex128

Inf returns a complex infinity, complex(+Inf, +Inf).
func IsInf

func IsInf(x complex128) bool

IsInf returns true if either real(x) or imag(x) is an infinity.
func IsNaN

func IsNaN(x complex128) bool

IsNaN returns true if either real(x) or imag(x) is NaN and neither is an infinity.
func Log

func Log(x complex128) complex128

Log returns the natural logarithm of x.
func Log10

func Log10(x complex128) complex128

Log10 returns the decimal logarithm of x.
func NaN

func NaN() complex128

NaN returns a complex “not-a-number” value.
func Phase

func Phase(x complex128) float64

Phase returns the phase (also called the argument) of x. The returned value is in the range [-Pi, Pi].
func Polar

func Polar(x complex128) (r, θ float64)

Polar returns the absolute value r and phase θ of x, such that x = r * e**θi. The phase is in the range [-Pi, Pi].

▹ Example
func Pow

func Pow(x, y complex128) complex128

Pow returns x**y, the base-x exponential of y. For generalized compatibility with math.Pow:

Pow(0, ±0) returns 1+0i
Pow(0, c) for real(c)<0 returns Inf+0i if imag(c) is zero, otherwise Inf+Inf i.

func Rect

func Rect(r, θ float64) complex128

Rect returns the complex number x with polar coordinates r, θ.
func Sin

func Sin(x complex128) complex128

Sin returns the sine of x.
func Sinh

func Sinh(x complex128) complex128

Sinh returns the hyperbolic sine of x.
func Sqrt

func Sqrt(x complex128) complex128

Sqrt returns the square root of x. The result r is chosen so that real(r) ≥ 0 and imag(r) has the same sign as imag(x).
func Tan

func Tan(x complex128) complex128

Tan returns the tangent of x.
func Tanh

func Tanh(x complex128) complex128

Tanh returns the hyperbolic tangent of x. 
