
 Package des

    import "crypto/des"

    Overview
    Index
    Examples

Overview ▾

Package des implements the Data Encryption Standard (DES) and the Triple Data Encryption Algorithm (TDEA) as defined in U.S. Federal Information Processing Standards Publication 46-3.
Index ▾

    Constants
    func NewCipher(key []byte) (cipher.Block, error)
    func NewTripleDESCipher(key []byte) (cipher.Block, error)
    type KeySizeError
        func (k KeySizeError) Error() string

Examples

    NewTripleDESCipher

Package files

block.go cipher.go const.go
Constants

The DES block size in bytes.

const BlockSize = 8

func NewCipher

func NewCipher(key []byte) (cipher.Block, error)

NewCipher creates and returns a new cipher.Block.
func NewTripleDESCipher

func NewTripleDESCipher(key []byte) (cipher.Block, error)

NewTripleDESCipher creates and returns a new cipher.Block.

▹ Example
type KeySizeError

type KeySizeError int

func (KeySizeError) Error

func (k KeySizeError) Error() string
