
 Package sha1

    import "crypto/sha1"

    Overview
    Index
    Examples

Overview ▾

Package sha1 implements the SHA1 hash algorithm as defined in RFC 3174.
Index ▾

    Constants
    func New() hash.Hash
    func Sum(data []byte) [Size]byte

Examples

    New
    New (File)
    Sum

Package files

sha1.go sha1block.go sha1block_amd64.go
Constants

The blocksize of SHA1 in bytes.

const BlockSize = 64

The size of a SHA1 checksum in bytes.

const Size = 20

func New

func New() hash.Hash

New returns a new hash.Hash computing the SHA1 checksum.

▹ Example

▹ Example (File)
func Sum

func Sum(data []byte) [Size]byte

Sum returns the SHA1 checksum of the data.

▹ Example
