
 Package httputil

    import "net/http/httputil"

    Overview
    Index
    Examples

Overview ▾

Package httputil provides HTTP utility functions, complementing the more common ones in the net/http package.
Index ▾

    Variables
    func DumpRequest(req *http.Request, body bool) ([]byte, error)
    func DumpRequestOut(req *http.Request, body bool) ([]byte, error)
    func DumpResponse(resp *http.Response, body bool) ([]byte, error)
    func NewChunkedReader(r io.Reader) io.Reader
    func NewChunkedWriter(w io.Writer) io.WriteCloser
    type BufferPool
    type ClientConn
        func NewClientConn(c net.Conn, r *bufio.Reader) *ClientConn
        func NewProxyClientConn(c net.Conn, r *bufio.Reader) *ClientConn
        func (cc *ClientConn) Close() error
        func (cc *ClientConn) Do(req *http.Request) (*http.Response, error)
        func (cc *ClientConn) Hijack() (c net.Conn, r *bufio.Reader)
        func (cc *ClientConn) Pending() int
        func (cc *ClientConn) Read(req *http.Request) (resp *http.Response, err error)
        func (cc *ClientConn) Write(req *http.Request) error
    type ReverseProxy
        func NewSingleHostReverseProxy(target *url.URL) *ReverseProxy
        func (p *ReverseProxy) ServeHTTP(rw http.ResponseWriter, req *http.Request)
    type ServerConn
        func NewServerConn(c net.Conn, r *bufio.Reader) *ServerConn
        func (sc *ServerConn) Close() error
        func (sc *ServerConn) Hijack() (net.Conn, *bufio.Reader)
        func (sc *ServerConn) Pending() int
        func (sc *ServerConn) Read() (*http.Request, error)
        func (sc *ServerConn) Write(req *http.Request, resp *http.Response) error

Examples

    DumpRequest
    DumpRequestOut
    DumpResponse
    ReverseProxy

Package files

dump.go httputil.go persist.go reverseproxy.go
Variables

var (
        // Deprecated: No longer used.
        ErrPersistEOF = &http.ProtocolError{ErrorString: "persistent connection closed"}

        // Deprecated: No longer used.
        ErrClosed = &http.ProtocolError{ErrorString: "connection closed by user"}

        // Deprecated: No longer used.
        ErrPipeline = &http.ProtocolError{ErrorString: "pipeline error"}
)

ErrLineTooLong is returned when reading malformed chunked data with lines that are too long.

var ErrLineTooLong = internal.ErrLineTooLong

func DumpRequest

func DumpRequest(req *http.Request, body bool) ([]byte, error)

DumpRequest returns the given request in its HTTP/1.x wire representation. It should only be used by servers to debug client requests. The returned representation is an approximation only; some details of the initial request are lost while parsing it into an http.Request. In particular, the order and case of header field names are lost. The order of values in multi-valued headers is kept intact. HTTP/2 requests are dumped in HTTP/1.x form, not in their original binary representations.

If body is true, DumpRequest also returns the body. To do so, it consumes req.Body and then replaces it with a new io.ReadCloser that yields the same bytes. If DumpRequest returns an error, the state of req is undefined.

The documentation for http.Request.Write details which fields of req are included in the dump.

▹ Example
func DumpRequestOut

func DumpRequestOut(req *http.Request, body bool) ([]byte, error)

DumpRequestOut is like DumpRequest but for outgoing client requests. It includes any headers that the standard http.Transport adds, such as User-Agent.

▹ Example
func DumpResponse

func DumpResponse(resp *http.Response, body bool) ([]byte, error)

DumpResponse is like DumpRequest but dumps a response.

▹ Example
func NewChunkedReader

func NewChunkedReader(r io.Reader) io.Reader

NewChunkedReader returns a new chunkedReader that translates the data read from r out of HTTP "chunked" format before returning it. The chunkedReader returns io.EOF when the final 0-length chunk is read.

NewChunkedReader is not needed by normal applications. The http package automatically decodes chunking when reading response bodies.
func NewChunkedWriter

func NewChunkedWriter(w io.Writer) io.WriteCloser

NewChunkedWriter returns a new chunkedWriter that translates writes into HTTP "chunked" format before writing them to w. Closing the returned chunkedWriter sends the final 0-length chunk that marks the end of the stream.

NewChunkedWriter is not needed by normal applications. The http package adds chunking automatically if handlers don't set a Content-Length header. Using NewChunkedWriter inside a handler would result in double chunking or chunking with a Content-Length length, both of which are wrong.
type BufferPool

A BufferPool is an interface for getting and returning temporary byte slices for use by io.CopyBuffer.

type BufferPool interface {
        Get() []byte
        Put([]byte)
}

type ClientConn

ClientConn is an artifact of Go's early HTTP implementation. It is low-level, old, and unused by Go's current HTTP stack. We should have deleted it before Go 1.

Deprecated: Use Client or Transport in package net/http instead.

type ClientConn struct {
        // contains filtered or unexported fields
}

func NewClientConn

func NewClientConn(c net.Conn, r *bufio.Reader) *ClientConn

NewClientConn is an artifact of Go's early HTTP implementation. It is low-level, old, and unused by Go's current HTTP stack. We should have deleted it before Go 1.

Deprecated: Use the Client or Transport in package net/http instead.
func NewProxyClientConn

func NewProxyClientConn(c net.Conn, r *bufio.Reader) *ClientConn

NewProxyClientConn is an artifact of Go's early HTTP implementation. It is low-level, old, and unused by Go's current HTTP stack. We should have deleted it before Go 1.

Deprecated: Use the Client or Transport in package net/http instead.
func (*ClientConn) Close

func (cc *ClientConn) Close() error

Close calls Hijack and then also closes the underlying connection.
func (*ClientConn) Do

func (cc *ClientConn) Do(req *http.Request) (*http.Response, error)

Do is convenience method that writes a request and reads a response.
func (*ClientConn) Hijack

func (cc *ClientConn) Hijack() (c net.Conn, r *bufio.Reader)

Hijack detaches the ClientConn and returns the underlying connection as well as the read-side bufio which may have some left over data. Hijack may be called before the user or Read have signaled the end of the keep-alive logic. The user should not call Hijack while Read or Write is in progress.
func (*ClientConn) Pending

func (cc *ClientConn) Pending() int

Pending returns the number of unanswered requests that have been sent on the connection.
func (*ClientConn) Read

func (cc *ClientConn) Read(req *http.Request) (resp *http.Response, err error)

Read reads the next response from the wire. A valid response might be returned together with an ErrPersistEOF, which means that the remote requested that this be the last request serviced. Read can be called concurrently with Write, but not with another Read.
func (*ClientConn) Write

func (cc *ClientConn) Write(req *http.Request) error

Write writes a request. An ErrPersistEOF error is returned if the connection has been closed in an HTTP keepalive sense. If req.Close equals true, the keepalive connection is logically closed after this request and the opposing server is informed. An ErrUnexpectedEOF indicates the remote closed the underlying TCP connection, which is usually considered as graceful close.
type ReverseProxy

ReverseProxy is an HTTP Handler that takes an incoming request and sends it to another server, proxying the response back to the client.

type ReverseProxy struct {
        // Director must be a function which modifies
        // the request into a new request to be sent
        // using Transport. Its response is then copied
        // back to the original client unmodified.
        // Director must not access the provided Request
        // after returning.
        Director func(*http.Request)

        // The transport used to perform proxy requests.
        // If nil, http.DefaultTransport is used.
        Transport http.RoundTripper

        // FlushInterval specifies the flush interval
        // to flush to the client while copying the
        // response body.
        // If zero, no periodic flushing is done.
        FlushInterval time.Duration

        // ErrorLog specifies an optional logger for errors
        // that occur when attempting to proxy the request.
        // If nil, logging goes to os.Stderr via the log package's
        // standard logger.
        ErrorLog *log.Logger

        // BufferPool optionally specifies a buffer pool to
        // get byte slices for use by io.CopyBuffer when
        // copying HTTP response bodies.
        BufferPool BufferPool

        // ModifyResponse is an optional function that
        // modifies the Response from the backend.
        // If it returns an error, the proxy returns a StatusBadGateway error.
        ModifyResponse func(*http.Response) error
}

▹ Example
func NewSingleHostReverseProxy

func NewSingleHostReverseProxy(target *url.URL) *ReverseProxy

NewSingleHostReverseProxy returns a new ReverseProxy that routes URLs to the scheme, host, and base path provided in target. If the target's path is "/base" and the incoming request was for "/dir", the target request will be for /base/dir. NewSingleHostReverseProxy does not rewrite the Host header. To rewrite Host headers, use ReverseProxy directly with a custom Director policy.
func (*ReverseProxy) ServeHTTP

func (p *ReverseProxy) ServeHTTP(rw http.ResponseWriter, req *http.Request)

type ServerConn

ServerConn is an artifact of Go's early HTTP implementation. It is low-level, old, and unused by Go's current HTTP stack. We should have deleted it before Go 1.

Deprecated: Use the Server in package net/http instead.

type ServerConn struct {
        // contains filtered or unexported fields
}

func NewServerConn

func NewServerConn(c net.Conn, r *bufio.Reader) *ServerConn

NewServerConn is an artifact of Go's early HTTP implementation. It is low-level, old, and unused by Go's current HTTP stack. We should have deleted it before Go 1.

Deprecated: Use the Server in package net/http instead.
func (*ServerConn) Close

func (sc *ServerConn) Close() error

Close calls Hijack and then also closes the underlying connection.
func (*ServerConn) Hijack

func (sc *ServerConn) Hijack() (net.Conn, *bufio.Reader)

Hijack detaches the ServerConn and returns the underlying connection as well as the read-side bufio which may have some left over data. Hijack may be called before Read has signaled the end of the keep-alive logic. The user should not call Hijack while Read or Write is in progress.
func (*ServerConn) Pending

func (sc *ServerConn) Pending() int

Pending returns the number of unanswered requests that have been received on the connection.
func (*ServerConn) Read

func (sc *ServerConn) Read() (*http.Request, error)

Read returns the next request on the wire. An ErrPersistEOF is returned if it is gracefully determined that there are no more requests (e.g. after the first request on an HTTP/1.0 connection, or after a Connection:close on a HTTP/1.1 connection).
func (*ServerConn) Write

func (sc *ServerConn) Write(req *http.Request, resp *http.Response) error

Write writes resp in response to req. To close the connection gracefully, set the Response.Close field to true. Write should be considered operational until it returns an error, regardless of any errors returned on the Read side. 
