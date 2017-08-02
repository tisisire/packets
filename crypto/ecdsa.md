
 Package ecdsa

    import "crypto/ecdsa"

    Overview
    Index

Overview ▾

Package ecdsa implements the Elliptic Curve Digital Signature Algorithm, as defined in FIPS 186-3.

This implementation derives the nonce from an AES-CTR CSPRNG keyed by ChopMD(256, SHA2-512(priv.D || entropy || hash)). The CSPRNG key is IRO by a result of Coron; the AES-CTR stream is IRO under standard assumptions.
Index ▾

    func Sign(rand io.Reader, priv *PrivateKey, hash []byte) (r, s *big.Int, err error)
    func Verify(pub *PublicKey, hash []byte, r, s *big.Int) bool
    type PrivateKey
        func GenerateKey(c elliptic.Curve, rand io.Reader) (*PrivateKey, error)
        func (priv *PrivateKey) Public() crypto.PublicKey
        func (priv *PrivateKey) Sign(rand io.Reader, msg []byte, opts crypto.SignerOpts) ([]byte, error)
    type PublicKey

Package files

ecdsa.go
func Sign

func Sign(rand io.Reader, priv *PrivateKey, hash []byte) (r, s *big.Int, err error)

Sign signs a hash (which should be the result of hashing a larger message) using the private key, priv. If the hash is longer than the bit-length of the private key's curve order, the hash will be truncated to that length. It returns the signature as a pair of integers. The security of the private key depends on the entropy of rand.
func Verify

func Verify(pub *PublicKey, hash []byte, r, s *big.Int) bool

Verify verifies the signature in r, s of hash using the public key, pub. Its return value records whether the signature is valid.
type PrivateKey

PrivateKey represents a ECDSA private key.

type PrivateKey struct {
        PublicKey
        D *big.Int
}

func GenerateKey

func GenerateKey(c elliptic.Curve, rand io.Reader) (*PrivateKey, error)

GenerateKey generates a public and private key pair.
func (*PrivateKey) Public

func (priv *PrivateKey) Public() crypto.PublicKey

Public returns the public key corresponding to priv.
func (*PrivateKey) Sign

func (priv *PrivateKey) Sign(rand io.Reader, msg []byte, opts crypto.SignerOpts) ([]byte, error)

Sign signs msg with priv, reading randomness from rand. This method is intended to support keys where the private part is kept in, for example, a hardware module. Common uses should use the Sign function in this package directly.
type PublicKey

PublicKey represents an ECDSA public key.

type PublicKey struct {
        elliptic.Curve
        X, Y *big.Int
}
