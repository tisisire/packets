
 Package heap

    import "container/heap"

    Overview
    Index
    Examples

Overview ▾

Package heap provides heap operations for any type that implements heap.Interface. A heap is a tree with the property that each node is the minimum-valued node in its subtree.

The minimum element in the tree is the root, at index 0.

A heap is a common way to implement a priority queue. To build a priority queue, implement the Heap interface with the (negative) priority as the ordering for the Less method, so Push adds items while Pop removes the highest-priority item from the queue. The Examples include such an implementation; the file example_pq_test.go has the complete source.

▹ Example (IntHeap)

▹ Example (PriorityQueue)
Index ▾

    func Fix(h Interface, i int)
    func Init(h Interface)
    func Pop(h Interface) interface{}
    func Push(h Interface, x interface{})
    func Remove(h Interface, i int) interface{}
    type Interface

Examples

    Package (IntHeap)
    Package (PriorityQueue)

Package files

heap.go
func Fix

func Fix(h Interface, i int)

Fix re-establishes the heap ordering after the element at index i has changed its value. Changing the value of the element at index i and then calling Fix is equivalent to, but less expensive than, calling Remove(h, i) followed by a Push of the new value. The complexity is O(log(n)) where n = h.Len().
func Init

func Init(h Interface)

A heap must be initialized before any of the heap operations can be used. Init is idempotent with respect to the heap invariants and may be called whenever the heap invariants may have been invalidated. Its complexity is O(n) where n = h.Len().
func Pop

func Pop(h Interface) interface{}

Pop removes the minimum element (according to Less) from the heap and returns it. The complexity is O(log(n)) where n = h.Len(). It is equivalent to Remove(h, 0).
func Push

func Push(h Interface, x interface{})

Push pushes the element x onto the heap. The complexity is O(log(n)) where n = h.Len().
func Remove

func Remove(h Interface, i int) interface{}

Remove removes the element at index i from the heap. The complexity is O(log(n)) where n = h.Len().
type Interface

Any type that implements heap.Interface may be used as a min-heap with the following invariants (established after Init has been called or if the data is empty or sorted):

!h.Less(j, i) for 0 <= i < h.Len() and 2*i+1 <= j <= 2*i+2 and j < h.Len()

Note that Push and Pop in this interface are for package heap's implementation to call. To add and remove things from the heap, use heap.Push and heap.Pop.

type Interface interface {
        sort.Interface
        Push(x interface{}) // add x as element Len()
        Pop() interface{}   // remove and return element Len() - 1.
}
