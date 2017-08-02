
 Package pem

    import "encoding/pem"

    Overview
    Index
    Examples

Overview ▾

Package pem implements the PEM data encoding, which originated in Privacy Enhanced Mail. The most common use of PEM encoding today is in TLS keys and certificates. See RFC 1421.
Index ▾

    func Encode(out io.Writer, b *Block) error
    func EncodeToMemory(b *Block) []byte
    type Block
        func Decode(data []byte) (p *Block, rest []byte)

Examples

    Decode

Package files

pem.go
func Encode

func Encode(out io.Writer, b *Block) error

func EncodeToMemory

func EncodeToMemory(b *Block) []byte

type Block

A Block represents a PEM encoded structure.

The encoded form is:

-----BEGIN Type-----
Headers
base64-encoded Bytes
-----END Type-----

where Headers is a possibly empty sequence of Key: Value lines.

type Block struct {
        Type    string            // The type, taken from the preamble (i.e. "RSA PRIVATE KEY").
        Headers map[string]string // Optional headers.
        Bytes   []byte            // The decoded bytes of the contents. Typically a DER encoded ASN.1 structure.
}

func Decode

func Decode(data []byte) (p *Block, rest []byte)

Decode will find the next PEM formatted block (certificate, private key etc) in the input. It returns that block and the remainder of the input. If no PEM data is found, p is nil and the whole of the input is returned in rest.

▹ Example
