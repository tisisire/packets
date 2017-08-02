
 Package md5

    import "crypto/md5"

    Overview
    Index
    Examples

Overview ▾

Package md5 implements the MD5 hash algorithm as defined in RFC 1321.
Index ▾

    Constants
    func New() hash.Hash
    func Sum(data []byte) [Size]byte

Examples

    New
    New (File)
    Sum

Package files

md5.go md5block.go md5block_decl.go
Constants

The blocksize of MD5 in bytes.

const BlockSize = 64

The size of an MD5 checksum in bytes.

const Size = 16

func New

func New() hash.Hash

New returns a new hash.Hash computing the MD5 checksum.

▹ Example

▹ Example (File)
func Sum

func Sum(data []byte) [Size]byte

Sum returns the MD5 checksum of the data.

▹ Example
