
 Package hash

    import "hash"

    Overview
    Index
    Subdirectories

Overview ▾

Package hash provides interfaces for hash functions.
Index ▾

    type Hash
    type Hash32
    type Hash64

Package files

hash.go
type Hash

Hash is the common interface implemented by all hash functions.

type Hash interface {
        // Write (via the embedded io.Writer interface) adds more data to the running hash.
        // It never returns an error.
        io.Writer

        // Sum appends the current hash to b and returns the resulting slice.
        // It does not change the underlying hash state.
        Sum(b []byte) []byte

        // Reset resets the Hash to its initial state.
        Reset()

        // Size returns the number of bytes Sum will return.
        Size() int

        // BlockSize returns the hash's underlying block size.
        // The Write method must be able to accept any amount
        // of data, but it may operate more efficiently if all writes
        // are a multiple of the block size.
        BlockSize() int
}

type Hash32

Hash32 is the common interface implemented by all 32-bit hash functions.

type Hash32 interface {
        Hash
        Sum32() uint32
}

type Hash64

Hash64 is the common interface implemented by all 64-bit hash functions.

type Hash64 interface {
        Hash
        Sum64() uint64
}
