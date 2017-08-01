
 Package debug

    import "runtime/debug"

    Overview
    Index

Overview ▾

Package debug contains facilities for programs to debug themselves while they are running.
Index ▾

    func FreeOSMemory()
    func PrintStack()
    func ReadGCStats(stats *GCStats)
    func SetGCPercent(percent int) int
    func SetMaxStack(bytes int) int
    func SetMaxThreads(threads int) int
    func SetPanicOnFault(enabled bool) bool
    func SetTraceback(level string)
    func Stack() []byte
    func WriteHeapDump(fd uintptr)
    type GCStats

Package files

garbage.go stack.go stubs.go
func FreeOSMemory

func FreeOSMemory()

FreeOSMemory forces a garbage collection followed by an attempt to return as much memory to the operating system as possible. (Even if this is not called, the runtime gradually returns memory to the operating system in a background task.)
func PrintStack

func PrintStack()

PrintStack prints to standard error the stack trace returned by runtime.Stack.
func ReadGCStats

func ReadGCStats(stats *GCStats)

ReadGCStats reads statistics about garbage collection into stats. The number of entries in the pause history is system-dependent; stats.Pause slice will be reused if large enough, reallocated otherwise. ReadGCStats may use the full capacity of the stats.Pause slice. If stats.PauseQuantiles is non-empty, ReadGCStats fills it with quantiles summarizing the distribution of pause time. For example, if len(stats.PauseQuantiles) is 5, it will be filled with the minimum, 25%, 50%, 75%, and maximum pause times.
func SetGCPercent

func SetGCPercent(percent int) int

SetGCPercent sets the garbage collection target percentage: a collection is triggered when the ratio of freshly allocated data to live data remaining after the previous collection reaches this percentage. SetGCPercent returns the previous setting. The initial setting is the value of the GOGC environment variable at startup, or 100 if the variable is not set. A negative percentage disables garbage collection.
func SetMaxStack

func SetMaxStack(bytes int) int

SetMaxStack sets the maximum amount of memory that can be used by a single goroutine stack. If any goroutine exceeds this limit while growing its stack, the program crashes. SetMaxStack returns the previous setting. The initial setting is 1 GB on 64-bit systems, 250 MB on 32-bit systems.

SetMaxStack is useful mainly for limiting the damage done by goroutines that enter an infinite recursion. It only limits future stack growth.
func SetMaxThreads

func SetMaxThreads(threads int) int

SetMaxThreads sets the maximum number of operating system threads that the Go program can use. If it attempts to use more than this many, the program crashes. SetMaxThreads returns the previous setting. The initial setting is 10,000 threads.

The limit controls the number of operating system threads, not the number of goroutines. A Go program creates a new thread only when a goroutine is ready to run but all the existing threads are blocked in system calls, cgo calls, or are locked to other goroutines due to use of runtime.LockOSThread.

SetMaxThreads is useful mainly for limiting the damage done by programs that create an unbounded number of threads. The idea is to take down the program before it takes down the operating system.
func SetPanicOnFault

func SetPanicOnFault(enabled bool) bool

SetPanicOnFault controls the runtime's behavior when a program faults at an unexpected (non-nil) address. Such faults are typically caused by bugs such as runtime memory corruption, so the default response is to crash the program. Programs working with memory-mapped files or unsafe manipulation of memory may cause faults at non-nil addresses in less dramatic situations; SetPanicOnFault allows such programs to request that the runtime trigger only a panic, not a crash. SetPanicOnFault applies only to the current goroutine. It returns the previous setting.
func SetTraceback

func SetTraceback(level string)

SetTraceback sets the amount of detail printed by the runtime in the traceback it prints before exiting due to an unrecovered panic or an internal runtime error. The level argument takes the same values as the GOTRACEBACK environment variable. For example, SetTraceback("all") ensure that the program prints all goroutines when it crashes. See the package runtime documentation for details. If SetTraceback is called with a level lower than that of the environment variable, the call is ignored.
func Stack

func Stack() []byte

Stack returns a formatted stack trace of the goroutine that calls it. It calls runtime.Stack with a large enough buffer to capture the entire trace.
func WriteHeapDump

func WriteHeapDump(fd uintptr)

WriteHeapDump writes a description of the heap and the objects in it to the given file descriptor.

WriteHeapDump suspends the execution of all goroutines until the heap dump is completely written. Thus, the file descriptor must not be connected to a pipe or socket whose other end is in the same Go process; instead, use a temporary file or network socket.

The heap dump format is defined at https://golang.org/s/go15heapdump.
type GCStats

GCStats collect information about recent garbage collections.

type GCStats struct {
        LastGC         time.Time       // time of last collection
        NumGC          int64           // number of garbage collections
        PauseTotal     time.Duration   // total pause for all collections
        Pause          []time.Duration // pause history, most recent first
        PauseEnd       []time.Time     // pause end times history, most recent first
        PauseQuantiles []time.Duration
}
