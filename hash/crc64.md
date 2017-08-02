
 Package crc64

    import "hash/crc64"

    Overview
    Index

Overview ▾

Package crc64 implements the 64-bit cyclic redundancy check, or CRC-64, checksum. See http://en.wikipedia.org/wiki/Cyclic_redundancy_check for information.
Index ▾

    Constants
    func Checksum(data []byte, tab *Table) uint64
    func New(tab *Table) hash.Hash64
    func Update(crc uint64, tab *Table, p []byte) uint64
    type Table
        func MakeTable(poly uint64) *Table

Package files

crc64.go
Constants

Predefined polynomials.

const (
        // The ISO polynomial, defined in ISO 3309 and used in HDLC.
        ISO = 0xD800000000000000

        // The ECMA polynomial, defined in ECMA 182.
        ECMA = 0xC96C5795D7870F42
)

The size of a CRC-64 checksum in bytes.

const Size = 8

func Checksum

func Checksum(data []byte, tab *Table) uint64

Checksum returns the CRC-64 checksum of data using the polynomial represented by the Table.
func New

func New(tab *Table) hash.Hash64

New creates a new hash.Hash64 computing the CRC-64 checksum using the polynomial represented by the Table. Its Sum method will lay the value out in big-endian byte order.
func Update

func Update(crc uint64, tab *Table, p []byte) uint64

Update returns the result of adding the bytes in p to the crc.
type Table

Table is a 256-word table representing the polynomial for efficient processing.

type Table [256]uint64

func MakeTable

func MakeTable(poly uint64) *Table

MakeTable returns a Table constructed from the specified polynomial. The contents of this Table must not be modified. 
