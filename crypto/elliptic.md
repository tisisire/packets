
 Package elliptic

    import "crypto/elliptic"

    Overview
    Index

Overview ▾

Package elliptic implements several standard elliptic curves over prime fields.
Index ▾

    func GenerateKey(curve Curve, rand io.Reader) (priv []byte, x, y *big.Int, err error)
    func Marshal(curve Curve, x, y *big.Int) []byte
    func Unmarshal(curve Curve, data []byte) (x, y *big.Int)
    type Curve
        func P224() Curve
        func P256() Curve
        func P384() Curve
        func P521() Curve
    type CurveParams
        func (curve *CurveParams) Add(x1, y1, x2, y2 *big.Int) (*big.Int, *big.Int)
        func (curve *CurveParams) Double(x1, y1 *big.Int) (*big.Int, *big.Int)
        func (curve *CurveParams) IsOnCurve(x, y *big.Int) bool
        func (curve *CurveParams) Params() *CurveParams
        func (curve *CurveParams) ScalarBaseMult(k []byte) (*big.Int, *big.Int)
        func (curve *CurveParams) ScalarMult(Bx, By *big.Int, k []byte) (*big.Int, *big.Int)

Package files

elliptic.go p224.go p256_amd64.go
func GenerateKey

func GenerateKey(curve Curve, rand io.Reader) (priv []byte, x, y *big.Int, err error)

GenerateKey returns a public/private key pair. The private key is generated using the given reader, which must return random data.
func Marshal

func Marshal(curve Curve, x, y *big.Int) []byte

Marshal converts a point into the form specified in section 4.3.6 of ANSI X9.62.
func Unmarshal

func Unmarshal(curve Curve, data []byte) (x, y *big.Int)

Unmarshal converts a point, serialized by Marshal, into an x, y pair. It is an error if the point is not on the curve. On error, x = nil.
type Curve

A Curve represents a short-form Weierstrass curve with a=-3. See http://www.hyperelliptic.org/EFD/g1p/auto-shortw.html

type Curve interface {
        // Params returns the parameters for the curve.
        Params() *CurveParams
        // IsOnCurve reports whether the given (x,y) lies on the curve.
        IsOnCurve(x, y *big.Int) bool
        // Add returns the sum of (x1,y1) and (x2,y2)
        Add(x1, y1, x2, y2 *big.Int) (x, y *big.Int)
        // Double returns 2*(x,y)
        Double(x1, y1 *big.Int) (x, y *big.Int)
        // ScalarMult returns k*(Bx,By) where k is a number in big-endian form.
        ScalarMult(x1, y1 *big.Int, k []byte) (x, y *big.Int)
        // ScalarBaseMult returns k*G, where G is the base point of the group
        // and k is an integer in big-endian form.
        ScalarBaseMult(k []byte) (x, y *big.Int)
}

func P224

func P224() Curve

P224 returns a Curve which implements P-224 (see FIPS 186-3, section D.2.2).

The cryptographic operations are implemented using constant-time algorithms.
func P256

func P256() Curve

P256 returns a Curve which implements P-256 (see FIPS 186-3, section D.2.3)

The cryptographic operations are implemented using constant-time algorithms.
func P384

func P384() Curve

P384 returns a Curve which implements P-384 (see FIPS 186-3, section D.2.4)

The cryptographic operations do not use constant-time algorithms.
func P521

func P521() Curve

P521 returns a Curve which implements P-521 (see FIPS 186-3, section D.2.5)

The cryptographic operations do not use constant-time algorithms.
type CurveParams

CurveParams contains the parameters of an elliptic curve and also provides a generic, non-constant time implementation of Curve.

type CurveParams struct {
        P       *big.Int // the order of the underlying field
        N       *big.Int // the order of the base point
        B       *big.Int // the constant of the curve equation
        Gx, Gy  *big.Int // (x,y) of the base point
        BitSize int      // the size of the underlying field
        Name    string   // the canonical name of the curve
}

func (*CurveParams) Add

func (curve *CurveParams) Add(x1, y1, x2, y2 *big.Int) (*big.Int, *big.Int)

func (*CurveParams) Double

func (curve *CurveParams) Double(x1, y1 *big.Int) (*big.Int, *big.Int)

func (*CurveParams) IsOnCurve

func (curve *CurveParams) IsOnCurve(x, y *big.Int) bool

func (*CurveParams) Params

func (curve *CurveParams) Params() *CurveParams

func (*CurveParams) ScalarBaseMult

func (curve *CurveParams) ScalarBaseMult(k []byte) (*big.Int, *big.Int)

func (*CurveParams) ScalarMult

func (curve *CurveParams) ScalarMult(Bx, By *big.Int, k []byte) (*big.Int, *big.Int)
