
 Package httptrace

    import "net/http/httptrace"

    Overview
    Index
    Examples

Overview ▾

Package httptrace provides mechanisms to trace the events within HTTP client requests.

▹ Example
Index ▾

    func WithClientTrace(ctx context.Context, trace *ClientTrace) context.Context
    type ClientTrace
        func ContextClientTrace(ctx context.Context) *ClientTrace
    type DNSDoneInfo
    type DNSStartInfo
    type GotConnInfo
    type WroteRequestInfo

Examples

    Package

Package files

trace.go
func WithClientTrace

func WithClientTrace(ctx context.Context, trace *ClientTrace) context.Context

WithClientTrace returns a new context based on the provided parent ctx. HTTP client requests made with the returned context will use the provided trace hooks, in addition to any previous hooks registered with ctx. Any hooks defined in the provided trace will be called first.
type ClientTrace

ClientTrace is a set of hooks to run at various stages of an outgoing HTTP request. Any particular hook may be nil. Functions may be called concurrently from different goroutines and some may be called after the request has completed or failed.

ClientTrace currently traces a single HTTP request & response during a single round trip and has no hooks that span a series of redirected requests.

See https://blog.golang.org/http-tracing for more.

type ClientTrace struct {
        // GetConn is called before a connection is created or
        // retrieved from an idle pool. The hostPort is the
        // "host:port" of the target or proxy. GetConn is called even
        // if there's already an idle cached connection available.
        GetConn func(hostPort string)

        // GotConn is called after a successful connection is
        // obtained. There is no hook for failure to obtain a
        // connection; instead, use the error from
        // Transport.RoundTrip.
        GotConn func(GotConnInfo)

        // PutIdleConn is called when the connection is returned to
        // the idle pool. If err is nil, the connection was
        // successfully returned to the idle pool. If err is non-nil,
        // it describes why not. PutIdleConn is not called if
        // connection reuse is disabled via Transport.DisableKeepAlives.
        // PutIdleConn is called before the caller's Response.Body.Close
        // call returns.
        // For HTTP/2, this hook is not currently used.
        PutIdleConn func(err error)

        // GotFirstResponseByte is called when the first byte of the response
        // headers is available.
        GotFirstResponseByte func()

        // Got100Continue is called if the server replies with a "100
        // Continue" response.
        Got100Continue func()

        // DNSStart is called when a DNS lookup begins.
        DNSStart func(DNSStartInfo)

        // DNSDone is called when a DNS lookup ends.
        DNSDone func(DNSDoneInfo)

        // ConnectStart is called when a new connection's Dial begins.
        // If net.Dialer.DualStack (IPv6 "Happy Eyeballs") support is
        // enabled, this may be called multiple times.
        ConnectStart func(network, addr string)

        // ConnectDone is called when a new connection's Dial
        // completes. The provided err indicates whether the
        // connection completedly successfully.
        // If net.Dialer.DualStack ("Happy Eyeballs") support is
        // enabled, this may be called multiple times.
        ConnectDone func(network, addr string, err error)

        // TLSHandshakeStart is called when the TLS handshake is started. When
        // connecting to a HTTPS site via a HTTP proxy, the handshake happens after
        // the CONNECT request is processed by the proxy.
        TLSHandshakeStart func()

        // TLSHandshakeDone is called after the TLS handshake with either the
        // successful handshake's connection state, or a non-nil error on handshake
        // failure.
        TLSHandshakeDone func(tls.ConnectionState, error)

        // WroteHeaders is called after the Transport has written
        // the request headers.
        WroteHeaders func()

        // Wait100Continue is called if the Request specified
        // "Expected: 100-continue" and the Transport has written the
        // request headers but is waiting for "100 Continue" from the
        // server before writing the request body.
        Wait100Continue func()

        // WroteRequest is called with the result of writing the
        // request and any body. It may be called multiple times
        // in the case of retried requests.
        WroteRequest func(WroteRequestInfo)
}

func ContextClientTrace

func ContextClientTrace(ctx context.Context) *ClientTrace

ContextClientTrace returns the ClientTrace associated with the provided context. If none, it returns nil.
type DNSDoneInfo

DNSDoneInfo contains information about the results of a DNS lookup.

type DNSDoneInfo struct {
        // Addrs are the IPv4 and/or IPv6 addresses found in the DNS
        // lookup. The contents of the slice should not be mutated.
        Addrs []net.IPAddr

        // Err is any error that occurred during the DNS lookup.
        Err error

        // Coalesced is whether the Addrs were shared with another
        // caller who was doing the same DNS lookup concurrently.
        Coalesced bool
}

type DNSStartInfo

DNSStartInfo contains information about a DNS request.

type DNSStartInfo struct {
        Host string
}

type GotConnInfo

GotConnInfo is the argument to the ClientTrace.GotConn function and contains information about the obtained connection.

type GotConnInfo struct {
        // Conn is the connection that was obtained. It is owned by
        // the http.Transport and should not be read, written or
        // closed by users of ClientTrace.
        Conn net.Conn

        // Reused is whether this connection has been previously
        // used for another HTTP request.
        Reused bool

        // WasIdle is whether this connection was obtained from an
        // idle pool.
        WasIdle bool

        // IdleTime reports how long the connection was previously
        // idle, if WasIdle is true.
        IdleTime time.Duration
}

type WroteRequestInfo

WroteRequestInfo contains information provided to the WroteRequest hook.

type WroteRequestInfo struct {
        // Err is any error encountered while writing the Request.
        Err error
}
