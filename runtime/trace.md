
 Package trace

    import "runtime/trace"

    Overview
    Index

Overview ▾

Go execution tracer. The tracer captures a wide range of execution events like goroutine creation/blocking/unblocking, syscall enter/exit/block, GC-related events, changes of heap size, processor start/stop, etc and writes them to an io.Writer in a compact form. A precise nanosecond-precision timestamp and a stack trace is captured for most events. A trace can be analyzed later with 'go tool trace' command.
Index ▾

    func Start(w io.Writer) error
    func Stop()

Package files

trace.go
func Start

func Start(w io.Writer) error

Start enables tracing for the current program. While tracing, the trace will be buffered and written to w. Start returns an error if tracing is already enabled.
func Stop

func Stop()

Stop stops the current tracing, if any. Stop only returns after all the writes for the trace have completed. 
