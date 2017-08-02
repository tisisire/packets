
 Package rc4

    import "crypto/rc4"

    Overview
    Index

Overview ▾

Package rc4 implements RC4 encryption, as defined in Bruce Schneier's Applied Cryptography.
Index ▾

    type Cipher
        func NewCipher(key []byte) (*Cipher, error)
        func (c *Cipher) Reset()
        func (c *Cipher) XORKeyStream(dst, src []byte)
    type KeySizeError
        func (k KeySizeError) Error() string
    Bugs

Package files

rc4.go rc4_asm.go
type Cipher

A Cipher is an instance of RC4 using a particular key.

type Cipher struct {
        // contains filtered or unexported fields
}

func NewCipher

func NewCipher(key []byte) (*Cipher, error)

NewCipher creates and returns a new Cipher. The key argument should be the RC4 key, at least 1 byte and at most 256 bytes.
func (*Cipher) Reset

func (c *Cipher) Reset()

Reset zeros the key data so that it will no longer appear in the process's memory.
func (*Cipher) XORKeyStream

func (c *Cipher) XORKeyStream(dst, src []byte)

XORKeyStream sets dst to the result of XORing src with the key stream. Dst and src may be the same slice but otherwise should not overlap.
type KeySizeError

type KeySizeError int

func (KeySizeError) Error

func (k KeySizeError) Error() string

Bugs

    ☞

    RC4 is in common use but has design weaknesses that make it a poor choice for new protocols.
