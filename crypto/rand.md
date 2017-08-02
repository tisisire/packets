
 Package rand

    import "crypto/rand"

    Overview
    Index
    Examples

Overview ▾

Package rand implements a cryptographically secure pseudorandom number generator.
Index ▾

    Variables
    func Int(rand io.Reader, max *big.Int) (n *big.Int, err error)
    func Prime(rand io.Reader, bits int) (p *big.Int, err error)
    func Read(b []byte) (n int, err error)

Examples

    Read

Package files

eagain.go rand.go rand_linux.go rand_unix.go util.go
Variables

Reader is a global, shared instance of a cryptographically strong pseudo-random generator.

On Linux, Reader uses getrandom(2) if available, /dev/urandom otherwise. On OpenBSD, Reader uses getentropy(2). On other Unix-like systems, Reader reads from /dev/urandom. On Windows systems, Reader uses the CryptGenRandom API.

var Reader io.Reader

func Int

func Int(rand io.Reader, max *big.Int) (n *big.Int, err error)

Int returns a uniform random value in [0, max). It panics if max <= 0.
func Prime

func Prime(rand io.Reader, bits int) (p *big.Int, err error)

Prime returns a number, p, of the given size, such that p is prime with high probability. Prime will return error for any error returned by rand.Read or if bits < 2.
func Read

func Read(b []byte) (n int, err error)

Read is a helper function that calls Reader.Read using io.ReadFull. On return, n == len(b) if and only if err == nil.

▹ Example
