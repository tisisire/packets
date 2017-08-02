
 Package hmac

    import "crypto/hmac"

    Overview
    Index

Overview ▾

Package hmac implements the Keyed-Hash Message Authentication Code (HMAC) as defined in U.S. Federal Information Processing Standards Publication 198. An HMAC is a cryptographic hash that uses a key to sign a message. The receiver verifies the hash by recomputing it using the same key.

Receivers should be careful to use Equal to compare MACs in order to avoid timing side-channels:

// CheckMAC reports whether messageMAC is a valid HMAC tag for message.
func CheckMAC(message, messageMAC, key []byte) bool {
	mac := hmac.New(sha256.New, key)
	mac.Write(message)
	expectedMAC := mac.Sum(nil)
	return hmac.Equal(messageMAC, expectedMAC)
}

Index ▾

    func Equal(mac1, mac2 []byte) bool
    func New(h func() hash.Hash, key []byte) hash.Hash

Package files

hmac.go
func Equal

func Equal(mac1, mac2 []byte) bool

Equal compares two MACs for equality without leaking timing information.
func New

func New(h func() hash.Hash, key []byte) hash.Hash

New returns a new HMAC hash using the given hash.Hash type and key.
