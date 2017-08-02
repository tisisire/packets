
 Package fcgi

    import "net/http/fcgi"

    Overview
    Index

Overview ▾

Package fcgi implements the FastCGI protocol.

The protocol is not an official standard and the original documentation is no longer online. See the Internet Archive's mirror at: https://web.archive.org/web/20150420080736/http://www.fastcgi.com/drupal/node/6?q=node/22

Currently only the responder role is supported.
Index ▾

    Variables
    func Serve(l net.Listener, handler http.Handler) error

Package files

child.go fcgi.go
Variables

ErrConnClosed is returned by Read when a handler attempts to read the body of a request after the connection to the web server has been closed.

var ErrConnClosed = errors.New("fcgi: connection to web server closed")

ErrRequestAborted is returned by Read when a handler attempts to read the body of a request that has been aborted by the web server.

var ErrRequestAborted = errors.New("fcgi: request aborted by web server")

func Serve

func Serve(l net.Listener, handler http.Handler) error

Serve accepts incoming FastCGI connections on the listener l, creating a new goroutine for each. The goroutine reads requests and then calls handler to reply to them. If l is nil, Serve accepts connections from os.Stdin. If handler is nil, http.DefaultServeMux is used. 
