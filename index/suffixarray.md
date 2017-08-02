
 Package suffixarray

    import "index/suffixarray"

    Overview
    Index
    Examples

Overview ▾

Package suffixarray implements substring search in logarithmic time using an in-memory suffix array.

Example use:

// create index for some data
index := suffixarray.New(data)

// lookup byte slice s
offsets1 := index.Lookup(s, -1) // the list of all indices where s occurs in data
offsets2 := index.Lookup(s, 3)  // the list of at most 3 indices where s occurs in data

Index ▾

    type Index
        func New(data []byte) *Index
        func (x *Index) Bytes() []byte
        func (x *Index) FindAllIndex(r *regexp.Regexp, n int) (result [][]int)
        func (x *Index) Lookup(s []byte, n int) (result []int)
        func (x *Index) Read(r io.Reader) error
        func (x *Index) Write(w io.Writer) error

Examples

    Index.Lookup

Package files

qsufsort.go suffixarray.go
type Index

Index implements a suffix array for fast substring search.

type Index struct {
        // contains filtered or unexported fields
}

func New

func New(data []byte) *Index

New creates a new Index for data. Index creation time is O(N*log(N)) for N = len(data).
func (*Index) Bytes

func (x *Index) Bytes() []byte

Bytes returns the data over which the index was created. It must not be modified.
func (*Index) FindAllIndex

func (x *Index) FindAllIndex(r *regexp.Regexp, n int) (result [][]int)

FindAllIndex returns a sorted list of non-overlapping matches of the regular expression r, where a match is a pair of indices specifying the matched slice of x.Bytes(). If n < 0, all matches are returned in successive order. Otherwise, at most n matches are returned and they may not be successive. The result is nil if there are no matches, or if n == 0.
func (*Index) Lookup

func (x *Index) Lookup(s []byte, n int) (result []int)

Lookup returns an unsorted list of at most n indices where the byte string s occurs in the indexed data. If n < 0, all occurrences are returned. The result is nil if s is empty, s is not found, or n == 0. Lookup time is O(log(N)*len(s) + len(result)) where N is the size of the indexed data.

▹ Example
func (*Index) Read

func (x *Index) Read(r io.Reader) error

Read reads the index from r into x; x must not be nil.
func (*Index) Write

func (x *Index) Write(w io.Writer) error

Write writes the index x to w. 
