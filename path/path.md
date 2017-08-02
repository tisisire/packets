
 Package path

    import "path"

    Overview
    Index
    Examples
    Subdirectories

Overview ▾

Package path implements utility routines for manipulating slash-separated paths.

To manipulate operating system paths, use the path/filepath package.
Index ▾

    Variables
    func Base(path string) string
    func Clean(path string) string
    func Dir(path string) string
    func Ext(path string) string
    func IsAbs(path string) bool
    func Join(elem ...string) string
    func Match(pattern, name string) (matched bool, err error)
    func Split(path string) (dir, file string)

Examples

    Base
    Clean
    Dir
    Ext
    IsAbs
    Join
    Split

Package files

match.go path.go
Variables

ErrBadPattern indicates a globbing pattern was malformed.

var ErrBadPattern = errors.New("syntax error in pattern")

func Base

func Base(path string) string

Base returns the last element of path. Trailing slashes are removed before extracting the last element. If the path is empty, Base returns ".". If the path consists entirely of slashes, Base returns "/".

▹ Example
func Clean

func Clean(path string) string

Clean returns the shortest path name equivalent to path by purely lexical processing. It applies the following rules iteratively until no further processing can be done:

1. Replace multiple slashes with a single slash.
2. Eliminate each . path name element (the current directory).
3. Eliminate each inner .. path name element (the parent directory)
   along with the non-.. element that precedes it.
4. Eliminate .. elements that begin a rooted path:
   that is, replace "/.." by "/" at the beginning of a path.

The returned path ends in a slash only if it is the root "/".

If the result of this process is an empty string, Clean returns the string ".".

See also Rob Pike, “Lexical File Names in Plan 9 or Getting Dot-Dot Right,” https://9p.io/sys/doc/lexnames.html

▹ Example
func Dir

func Dir(path string) string

Dir returns all but the last element of path, typically the path's directory. After dropping the final element using Split, the path is Cleaned and trailing slashes are removed. If the path is empty, Dir returns ".". If the path consists entirely of slashes followed by non-slash bytes, Dir returns a single slash. In any other case, the returned path does not end in a slash.

▹ Example
func Ext

func Ext(path string) string

Ext returns the file name extension used by path. The extension is the suffix beginning at the final dot in the final slash-separated element of path; it is empty if there is no dot.

▹ Example
func IsAbs

func IsAbs(path string) bool

IsAbs reports whether the path is absolute.

▹ Example
func Join

func Join(elem ...string) string

Join joins any number of path elements into a single path, adding a separating slash if necessary. The result is Cleaned; in particular, all empty strings are ignored.

▹ Example
func Match

func Match(pattern, name string) (matched bool, err error)

Match reports whether name matches the shell file name pattern. The pattern syntax is:

pattern:
	{ term }
term:
	'*'         matches any sequence of non-/ characters
	'?'         matches any single non-/ character
	'[' [ '^' ] { character-range } ']'
	            character class (must be non-empty)
	c           matches character c (c != '*', '?', '\\', '[')
	'\\' c      matches character c

character-range:
	c           matches character c (c != '\\', '-', ']')
	'\\' c      matches character c
	lo '-' hi   matches character c for lo <= c <= hi

Match requires pattern to match all of name, not just a substring. The only possible returned error is ErrBadPattern, when pattern is malformed.
func Split

func Split(path string) (dir, file string)

Split splits path immediately following the final slash, separating it into a directory and file name component. If there is no slash in path, Split returns an empty dir and file set to path. The returned values have the property that path = dir+file.

▹ Example
