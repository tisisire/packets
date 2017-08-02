
 Package jsonrpc

    import "net/rpc/jsonrpc"

    Overview
    Index

Overview ▾

Package jsonrpc implements a JSON-RPC ClientCodec and ServerCodec for the rpc package.
Index ▾

    func Dial(network, address string) (*rpc.Client, error)
    func NewClient(conn io.ReadWriteCloser) *rpc.Client
    func NewClientCodec(conn io.ReadWriteCloser) rpc.ClientCodec
    func NewServerCodec(conn io.ReadWriteCloser) rpc.ServerCodec
    func ServeConn(conn io.ReadWriteCloser)

Package files

client.go server.go
func Dial

func Dial(network, address string) (*rpc.Client, error)

Dial connects to a JSON-RPC server at the specified network address.
func NewClient

func NewClient(conn io.ReadWriteCloser) *rpc.Client

NewClient returns a new rpc.Client to handle requests to the set of services at the other end of the connection.
func NewClientCodec

func NewClientCodec(conn io.ReadWriteCloser) rpc.ClientCodec

NewClientCodec returns a new rpc.ClientCodec using JSON-RPC on conn.
func NewServerCodec

func NewServerCodec(conn io.ReadWriteCloser) rpc.ServerCodec

NewServerCodec returns a new rpc.ServerCodec using JSON-RPC on conn.
func ServeConn

func ServeConn(conn io.ReadWriteCloser)

ServeConn runs the JSON-RPC server on a single connection. ServeConn blocks, serving the connection until the client hangs up. The caller typically invokes ServeConn in a go statement. 
