
 Package aes

    import "crypto/aes"

    Overview
    Index

Overview ▾

Package aes implements AES encryption (formerly Rijndael), as defined in U.S. Federal Information Processing Standards Publication 197.

The AES operations in this package are not implemented using constant-time algorithms. An exception is when running on systems with enabled hardware support for AES that makes these operations constant-time. Examples include amd64 systems using AES-NI extensions and s390x systems using Message-Security-Assist extensions. On such systems, when the result of NewCipher is passed to cipher.NewGCM, the GHASH operation used by GCM is also constant-time.
Index ▾

    Constants
    func NewCipher(key []byte) (cipher.Block, error)
    type KeySizeError
        func (k KeySizeError) Error() string

Package files

aes_gcm.go block.go cipher.go cipher_amd64.go const.go modes.go
Constants

The AES block size in bytes.

const BlockSize = 16

func NewCipher

func NewCipher(key []byte) (cipher.Block, error)

NewCipher creates and returns a new cipher.Block. The key argument should be the AES key, either 16, 24, or 32 bytes to select AES-128, AES-192, or AES-256.
type KeySizeError

type KeySizeError int

func (KeySizeError) Error

func (k KeySizeError) Error() string
