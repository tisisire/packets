
 Package list

    import "container/list"

    Overview
    Index
    Examples

Overview ▾

Package list implements a doubly linked list.

To iterate over a list (where l is a *List):

for e := l.Front(); e != nil; e = e.Next() {
	// do something with e.Value
}

▹ Example
Index ▾

    type Element
        func (e *Element) Next() *Element
        func (e *Element) Prev() *Element
    type List
        func New() *List
        func (l *List) Back() *Element
        func (l *List) Front() *Element
        func (l *List) Init() *List
        func (l *List) InsertAfter(v interface{}, mark *Element) *Element
        func (l *List) InsertBefore(v interface{}, mark *Element) *Element
        func (l *List) Len() int
        func (l *List) MoveAfter(e, mark *Element)
        func (l *List) MoveBefore(e, mark *Element)
        func (l *List) MoveToBack(e *Element)
        func (l *List) MoveToFront(e *Element)
        func (l *List) PushBack(v interface{}) *Element
        func (l *List) PushBackList(other *List)
        func (l *List) PushFront(v interface{}) *Element
        func (l *List) PushFrontList(other *List)
        func (l *List) Remove(e *Element) interface{}

Examples

    Package

Package files

list.go
type Element

Element is an element of a linked list.

type Element struct {

        // The value stored with this element.
        Value interface{}
        // contains filtered or unexported fields
}

func (*Element) Next

func (e *Element) Next() *Element

Next returns the next list element or nil.
func (*Element) Prev

func (e *Element) Prev() *Element

Prev returns the previous list element or nil.
type List

List represents a doubly linked list. The zero value for List is an empty list ready to use.

type List struct {
        // contains filtered or unexported fields
}

func New

func New() *List

New returns an initialized list.
func (*List) Back

func (l *List) Back() *Element

Back returns the last element of list l or nil.
func (*List) Front

func (l *List) Front() *Element

Front returns the first element of list l or nil.
func (*List) Init

func (l *List) Init() *List

Init initializes or clears list l.
func (*List) InsertAfter

func (l *List) InsertAfter(v interface{}, mark *Element) *Element

InsertAfter inserts a new element e with value v immediately after mark and returns e. If mark is not an element of l, the list is not modified.
func (*List) InsertBefore

func (l *List) InsertBefore(v interface{}, mark *Element) *Element

InsertBefore inserts a new element e with value v immediately before mark and returns e. If mark is not an element of l, the list is not modified.
func (*List) Len

func (l *List) Len() int

Len returns the number of elements of list l. The complexity is O(1).
func (*List) MoveAfter

func (l *List) MoveAfter(e, mark *Element)

MoveAfter moves element e to its new position after mark. If e or mark is not an element of l, or e == mark, the list is not modified.
func (*List) MoveBefore

func (l *List) MoveBefore(e, mark *Element)

MoveBefore moves element e to its new position before mark. If e or mark is not an element of l, or e == mark, the list is not modified.
func (*List) MoveToBack

func (l *List) MoveToBack(e *Element)

MoveToBack moves element e to the back of list l. If e is not an element of l, the list is not modified.
func (*List) MoveToFront

func (l *List) MoveToFront(e *Element)

MoveToFront moves element e to the front of list l. If e is not an element of l, the list is not modified.
func (*List) PushBack

func (l *List) PushBack(v interface{}) *Element

PushBack inserts a new element e with value v at the back of list l and returns e.
func (*List) PushBackList

func (l *List) PushBackList(other *List)

PushBackList inserts a copy of an other list at the back of list l. The lists l and other may be the same.
func (*List) PushFront

func (l *List) PushFront(v interface{}) *Element

PushFront inserts a new element e with value v at the front of list l and returns e.
func (*List) PushFrontList

func (l *List) PushFrontList(other *List)

PushFrontList inserts a copy of an other list at the front of list l. The lists l and other may be the same.
func (*List) Remove

func (l *List) Remove(e *Element) interface{}

Remove removes e from l if e is an element of list l. It returns the element value e.Value. 
