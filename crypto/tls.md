
 Package tls

    import "crypto/tls"

    Overview
    Index
    Examples

Overview ▾

Package tls partially implements TLS 1.2, as specified in RFC 5246.
Index ▾

    Constants
    func Listen(network, laddr string, config *Config) (net.Listener, error)
    func NewListener(inner net.Listener, config *Config) net.Listener
    type Certificate
        func LoadX509KeyPair(certFile, keyFile string) (Certificate, error)
        func X509KeyPair(certPEMBlock, keyPEMBlock []byte) (Certificate, error)
    type CertificateRequestInfo
    type ClientAuthType
    type ClientHelloInfo
    type ClientSessionCache
        func NewLRUClientSessionCache(capacity int) ClientSessionCache
    type ClientSessionState
    type Config
        func (c *Config) BuildNameToCertificate()
        func (c *Config) Clone() *Config
        func (c *Config) SetSessionTicketKeys(keys [][32]byte)
    type Conn
        func Client(conn net.Conn, config *Config) *Conn
        func Dial(network, addr string, config *Config) (*Conn, error)
        func DialWithDialer(dialer *net.Dialer, network, addr string, config *Config) (*Conn, error)
        func Server(conn net.Conn, config *Config) *Conn
        func (c *Conn) Close() error
        func (c *Conn) CloseWrite() error
        func (c *Conn) ConnectionState() ConnectionState
        func (c *Conn) Handshake() error
        func (c *Conn) LocalAddr() net.Addr
        func (c *Conn) OCSPResponse() []byte
        func (c *Conn) Read(b []byte) (n int, err error)
        func (c *Conn) RemoteAddr() net.Addr
        func (c *Conn) SetDeadline(t time.Time) error
        func (c *Conn) SetReadDeadline(t time.Time) error
        func (c *Conn) SetWriteDeadline(t time.Time) error
        func (c *Conn) VerifyHostname(host string) error
        func (c *Conn) Write(b []byte) (int, error)
    type ConnectionState
    type CurveID
    type RecordHeaderError
        func (e RecordHeaderError) Error() string
    type RenegotiationSupport
    type SignatureScheme
    Bugs

Examples

    Config (KeyLogWriter)
    Dial

Package files

alert.go cipher_suites.go common.go conn.go handshake_client.go handshake_messages.go handshake_server.go key_agreement.go prf.go ticket.go tls.go
Constants

A list of cipher suite IDs that are, or have been, implemented by this package.

Taken from http://www.iana.org/assignments/tls-parameters/tls-parameters.xml

const (
        TLS_RSA_WITH_RC4_128_SHA                uint16 = 0x0005
        TLS_RSA_WITH_3DES_EDE_CBC_SHA           uint16 = 0x000a
        TLS_RSA_WITH_AES_128_CBC_SHA            uint16 = 0x002f
        TLS_RSA_WITH_AES_256_CBC_SHA            uint16 = 0x0035
        TLS_RSA_WITH_AES_128_CBC_SHA256         uint16 = 0x003c
        TLS_RSA_WITH_AES_128_GCM_SHA256         uint16 = 0x009c
        TLS_RSA_WITH_AES_256_GCM_SHA384         uint16 = 0x009d
        TLS_ECDHE_ECDSA_WITH_RC4_128_SHA        uint16 = 0xc007
        TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA    uint16 = 0xc009
        TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA    uint16 = 0xc00a
        TLS_ECDHE_RSA_WITH_RC4_128_SHA          uint16 = 0xc011
        TLS_ECDHE_RSA_WITH_3DES_EDE_CBC_SHA     uint16 = 0xc012
        TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA      uint16 = 0xc013
        TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA      uint16 = 0xc014
        TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256 uint16 = 0xc023
        TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256   uint16 = 0xc027
        TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256   uint16 = 0xc02f
        TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 uint16 = 0xc02b
        TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384   uint16 = 0xc030
        TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 uint16 = 0xc02c
        TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305    uint16 = 0xcca8
        TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305  uint16 = 0xcca9

        // TLS_FALLBACK_SCSV isn't a standard cipher suite but an indicator
        // that the client is doing version fallback. See
        // https://tools.ietf.org/html/rfc7507.
        TLS_FALLBACK_SCSV uint16 = 0x5600
)

const (
        VersionSSL30 = 0x0300
        VersionTLS10 = 0x0301
        VersionTLS11 = 0x0302
        VersionTLS12 = 0x0303
)

func Listen

func Listen(network, laddr string, config *Config) (net.Listener, error)

Listen creates a TLS listener accepting connections on the given network address using net.Listen. The configuration config must be non-nil and must include at least one certificate or else set GetCertificate.
func NewListener

func NewListener(inner net.Listener, config *Config) net.Listener

NewListener creates a Listener which accepts connections from an inner Listener and wraps each connection with Server. The configuration config must be non-nil and must include at least one certificate or else set GetCertificate.
type Certificate

A Certificate is a chain of one or more certificates, leaf first.

type Certificate struct {
        Certificate [][]byte
        // PrivateKey contains the private key corresponding to the public key
        // in Leaf. For a server, this must implement crypto.Signer and/or
        // crypto.Decrypter, with an RSA or ECDSA PublicKey. For a client
        // (performing client authentication), this must be a crypto.Signer
        // with an RSA or ECDSA PublicKey.
        PrivateKey crypto.PrivateKey
        // OCSPStaple contains an optional OCSP response which will be served
        // to clients that request it.
        OCSPStaple []byte
        // SignedCertificateTimestamps contains an optional list of Signed
        // Certificate Timestamps which will be served to clients that request it.
        SignedCertificateTimestamps [][]byte
        // Leaf is the parsed form of the leaf certificate, which may be
        // initialized using x509.ParseCertificate to reduce per-handshake
        // processing for TLS clients doing client authentication. If nil, the
        // leaf certificate will be parsed as needed.
        Leaf *x509.Certificate
}

func LoadX509KeyPair

func LoadX509KeyPair(certFile, keyFile string) (Certificate, error)

LoadX509KeyPair reads and parses a public/private key pair from a pair of files. The files must contain PEM encoded data. The certificate file may contain intermediate certificates following the leaf certificate to form a certificate chain. On successful return, Certificate.Leaf will be nil because the parsed form of the certificate is not retained.
func X509KeyPair

func X509KeyPair(certPEMBlock, keyPEMBlock []byte) (Certificate, error)

X509KeyPair parses a public/private key pair from a pair of PEM encoded data. On successful return, Certificate.Leaf will be nil because the parsed form of the certificate is not retained.
type CertificateRequestInfo

CertificateRequestInfo contains information from a server's CertificateRequest message, which is used to demand a certificate and proof of control from a client.

type CertificateRequestInfo struct {
        // AcceptableCAs contains zero or more, DER-encoded, X.501
        // Distinguished Names. These are the names of root or intermediate CAs
        // that the server wishes the returned certificate to be signed by. An
        // empty slice indicates that the server has no preference.
        AcceptableCAs [][]byte

        // SignatureSchemes lists the signature schemes that the server is
        // willing to verify.
        SignatureSchemes []SignatureScheme
}

type ClientAuthType

ClientAuthType declares the policy the server will follow for TLS Client Authentication.

type ClientAuthType int

const (
        NoClientCert ClientAuthType = iota
        RequestClientCert
        RequireAnyClientCert
        VerifyClientCertIfGiven
        RequireAndVerifyClientCert
)

type ClientHelloInfo

ClientHelloInfo contains information from a ClientHello message in order to guide certificate selection in the GetCertificate callback.

type ClientHelloInfo struct {
        // CipherSuites lists the CipherSuites supported by the client (e.g.
        // TLS_RSA_WITH_RC4_128_SHA).
        CipherSuites []uint16

        // ServerName indicates the name of the server requested by the client
        // in order to support virtual hosting. ServerName is only set if the
        // client is using SNI (see
        // http://tools.ietf.org/html/rfc4366#section-3.1).
        ServerName string

        // SupportedCurves lists the elliptic curves supported by the client.
        // SupportedCurves is set only if the Supported Elliptic Curves
        // Extension is being used (see
        // http://tools.ietf.org/html/rfc4492#section-5.1.1).
        SupportedCurves []CurveID

        // SupportedPoints lists the point formats supported by the client.
        // SupportedPoints is set only if the Supported Point Formats Extension
        // is being used (see
        // http://tools.ietf.org/html/rfc4492#section-5.1.2).
        SupportedPoints []uint8

        // SignatureSchemes lists the signature and hash schemes that the client
        // is willing to verify. SignatureSchemes is set only if the Signature
        // Algorithms Extension is being used (see
        // https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1).
        SignatureSchemes []SignatureScheme

        // SupportedProtos lists the application protocols supported by the client.
        // SupportedProtos is set only if the Application-Layer Protocol
        // Negotiation Extension is being used (see
        // https://tools.ietf.org/html/rfc7301#section-3.1).
        //
        // Servers can select a protocol by setting Config.NextProtos in a
        // GetConfigForClient return value.
        SupportedProtos []string

        // SupportedVersions lists the TLS versions supported by the client.
        // For TLS versions less than 1.3, this is extrapolated from the max
        // version advertised by the client, so values other than the greatest
        // might be rejected if used.
        SupportedVersions []uint16

        // Conn is the underlying net.Conn for the connection. Do not read
        // from, or write to, this connection; that will cause the TLS
        // connection to fail.
        Conn net.Conn
}

type ClientSessionCache

ClientSessionCache is a cache of ClientSessionState objects that can be used by a client to resume a TLS session with a given server. ClientSessionCache implementations should expect to be called concurrently from different goroutines.

type ClientSessionCache interface {
        // Get searches for a ClientSessionState associated with the given key.
        // On return, ok is true if one was found.
        Get(sessionKey string) (session *ClientSessionState, ok bool)

        // Put adds the ClientSessionState to the cache with the given key.
        Put(sessionKey string, cs *ClientSessionState)
}

func NewLRUClientSessionCache

func NewLRUClientSessionCache(capacity int) ClientSessionCache

NewLRUClientSessionCache returns a ClientSessionCache with the given capacity that uses an LRU strategy. If capacity is < 1, a default capacity is used instead.
type ClientSessionState

ClientSessionState contains the state needed by clients to resume TLS sessions.

type ClientSessionState struct {
        // contains filtered or unexported fields
}

type Config

A Config structure is used to configure a TLS client or server. After one has been passed to a TLS function it must not be modified. A Config may be reused; the tls package will also not modify it.

type Config struct {
        // Rand provides the source of entropy for nonces and RSA blinding.
        // If Rand is nil, TLS uses the cryptographic random reader in package
        // crypto/rand.
        // The Reader must be safe for use by multiple goroutines.
        Rand io.Reader

        // Time returns the current time as the number of seconds since the epoch.
        // If Time is nil, TLS uses time.Now.
        Time func() time.Time

        // Certificates contains one or more certificate chains to present to
        // the other side of the connection. Server configurations must include
        // at least one certificate or else set GetCertificate. Clients doing
        // client-authentication may set either Certificates or
        // GetClientCertificate.
        Certificates []Certificate

        // NameToCertificate maps from a certificate name to an element of
        // Certificates. Note that a certificate name can be of the form
        // '*.example.com' and so doesn't have to be a domain name as such.
        // See Config.BuildNameToCertificate
        // The nil value causes the first element of Certificates to be used
        // for all connections.
        NameToCertificate map[string]*Certificate

        // GetCertificate returns a Certificate based on the given
        // ClientHelloInfo. It will only be called if the client supplies SNI
        // information or if Certificates is empty.
        //
        // If GetCertificate is nil or returns nil, then the certificate is
        // retrieved from NameToCertificate. If NameToCertificate is nil, the
        // first element of Certificates will be used.
        GetCertificate func(*ClientHelloInfo) (*Certificate, error)

        // GetClientCertificate, if not nil, is called when a server requests a
        // certificate from a client. If set, the contents of Certificates will
        // be ignored.
        //
        // If GetClientCertificate returns an error, the handshake will be
        // aborted and that error will be returned. Otherwise
        // GetClientCertificate must return a non-nil Certificate. If
        // Certificate.Certificate is empty then no certificate will be sent to
        // the server. If this is unacceptable to the server then it may abort
        // the handshake.
        //
        // GetClientCertificate may be called multiple times for the same
        // connection if renegotiation occurs or if TLS 1.3 is in use.
        GetClientCertificate func(*CertificateRequestInfo) (*Certificate, error)

        // GetConfigForClient, if not nil, is called after a ClientHello is
        // received from a client. It may return a non-nil Config in order to
        // change the Config that will be used to handle this connection. If
        // the returned Config is nil, the original Config will be used. The
        // Config returned by this callback may not be subsequently modified.
        //
        // If GetConfigForClient is nil, the Config passed to Server() will be
        // used for all connections.
        //
        // Uniquely for the fields in the returned Config, session ticket keys
        // will be duplicated from the original Config if not set.
        // Specifically, if SetSessionTicketKeys was called on the original
        // config but not on the returned config then the ticket keys from the
        // original config will be copied into the new config before use.
        // Otherwise, if SessionTicketKey was set in the original config but
        // not in the returned config then it will be copied into the returned
        // config before use. If neither of those cases applies then the key
        // material from the returned config will be used for session tickets.
        GetConfigForClient func(*ClientHelloInfo) (*Config, error)

        // VerifyPeerCertificate, if not nil, is called after normal
        // certificate verification by either a TLS client or server. It
        // receives the raw ASN.1 certificates provided by the peer and also
        // any verified chains that normal processing found. If it returns a
        // non-nil error, the handshake is aborted and that error results.
        //
        // If normal verification fails then the handshake will abort before
        // considering this callback. If normal verification is disabled by
        // setting InsecureSkipVerify then this callback will be considered but
        // the verifiedChains argument will always be nil.
        VerifyPeerCertificate func(rawCerts [][]byte, verifiedChains [][]*x509.Certificate) error

        // RootCAs defines the set of root certificate authorities
        // that clients use when verifying server certificates.
        // If RootCAs is nil, TLS uses the host's root CA set.
        RootCAs *x509.CertPool

        // NextProtos is a list of supported, application level protocols.
        NextProtos []string

        // ServerName is used to verify the hostname on the returned
        // certificates unless InsecureSkipVerify is given. It is also included
        // in the client's handshake to support virtual hosting unless it is
        // an IP address.
        ServerName string

        // ClientAuth determines the server's policy for
        // TLS Client Authentication. The default is NoClientCert.
        ClientAuth ClientAuthType

        // ClientCAs defines the set of root certificate authorities
        // that servers use if required to verify a client certificate
        // by the policy in ClientAuth.
        ClientCAs *x509.CertPool

        // InsecureSkipVerify controls whether a client verifies the
        // server's certificate chain and host name.
        // If InsecureSkipVerify is true, TLS accepts any certificate
        // presented by the server and any host name in that certificate.
        // In this mode, TLS is susceptible to man-in-the-middle attacks.
        // This should be used only for testing.
        InsecureSkipVerify bool

        // CipherSuites is a list of supported cipher suites. If CipherSuites
        // is nil, TLS uses a list of suites supported by the implementation.
        CipherSuites []uint16

        // PreferServerCipherSuites controls whether the server selects the
        // client's most preferred ciphersuite, or the server's most preferred
        // ciphersuite. If true then the server's preference, as expressed in
        // the order of elements in CipherSuites, is used.
        PreferServerCipherSuites bool

        // SessionTicketsDisabled may be set to true to disable session ticket
        // (resumption) support.
        SessionTicketsDisabled bool

        // SessionTicketKey is used by TLS servers to provide session
        // resumption. See RFC 5077. If zero, it will be filled with
        // random data before the first server handshake.
        //
        // If multiple servers are terminating connections for the same host
        // they should all have the same SessionTicketKey. If the
        // SessionTicketKey leaks, previously recorded and future TLS
        // connections using that key are compromised.
        SessionTicketKey [32]byte

        // SessionCache is a cache of ClientSessionState entries for TLS session
        // resumption.
        ClientSessionCache ClientSessionCache

        // MinVersion contains the minimum SSL/TLS version that is acceptable.
        // If zero, then TLS 1.0 is taken as the minimum.
        MinVersion uint16

        // MaxVersion contains the maximum SSL/TLS version that is acceptable.
        // If zero, then the maximum version supported by this package is used,
        // which is currently TLS 1.2.
        MaxVersion uint16

        // CurvePreferences contains the elliptic curves that will be used in
        // an ECDHE handshake, in preference order. If empty, the default will
        // be used.
        CurvePreferences []CurveID

        // DynamicRecordSizingDisabled disables adaptive sizing of TLS records.
        // When true, the largest possible TLS record size is always used. When
        // false, the size of TLS records may be adjusted in an attempt to
        // improve latency.
        DynamicRecordSizingDisabled bool

        // Renegotiation controls what types of renegotiation are supported.
        // The default, none, is correct for the vast majority of applications.
        Renegotiation RenegotiationSupport

        // KeyLogWriter optionally specifies a destination for TLS master secrets
        // in NSS key log format that can be used to allow external programs
        // such as Wireshark to decrypt TLS connections.
        // See https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/Key_Log_Format.
        // Use of KeyLogWriter compromises security and should only be
        // used for debugging.
        KeyLogWriter io.Writer
        // contains filtered or unexported fields
}

▹ Example (KeyLogWriter)
func (*Config) BuildNameToCertificate

func (c *Config) BuildNameToCertificate()

BuildNameToCertificate parses c.Certificates and builds c.NameToCertificate from the CommonName and SubjectAlternateName fields of each of the leaf certificates.
func (*Config) Clone

func (c *Config) Clone() *Config

Clone returns a shallow clone of c. It is safe to clone a Config that is being used concurrently by a TLS client or server.
func (*Config) SetSessionTicketKeys

func (c *Config) SetSessionTicketKeys(keys [][32]byte)

SetSessionTicketKeys updates the session ticket keys for a server. The first key will be used when creating new tickets, while all keys can be used for decrypting tickets. It is safe to call this function while the server is running in order to rotate the session ticket keys. The function will panic if keys is empty.
type Conn

A Conn represents a secured connection. It implements the net.Conn interface.

type Conn struct {
        // contains filtered or unexported fields
}

func Client

func Client(conn net.Conn, config *Config) *Conn

Client returns a new TLS client side connection using conn as the underlying transport. The config cannot be nil: users must set either ServerName or InsecureSkipVerify in the config.
func Dial

func Dial(network, addr string, config *Config) (*Conn, error)

Dial connects to the given network address using net.Dial and then initiates a TLS handshake, returning the resulting TLS connection. Dial interprets a nil configuration as equivalent to the zero configuration; see the documentation of Config for the defaults.

▹ Example
func DialWithDialer

func DialWithDialer(dialer *net.Dialer, network, addr string, config *Config) (*Conn, error)

DialWithDialer connects to the given network address using dialer.Dial and then initiates a TLS handshake, returning the resulting TLS connection. Any timeout or deadline given in the dialer apply to connection and TLS handshake as a whole.

DialWithDialer interprets a nil configuration as equivalent to the zero configuration; see the documentation of Config for the defaults.
func Server

func Server(conn net.Conn, config *Config) *Conn

Server returns a new TLS server side connection using conn as the underlying transport. The configuration config must be non-nil and must include at least one certificate or else set GetCertificate.
func (*Conn) Close

func (c *Conn) Close() error

Close closes the connection.
func (*Conn) CloseWrite

func (c *Conn) CloseWrite() error

CloseWrite shuts down the writing side of the connection. It should only be called once the handshake has completed and does not call CloseWrite on the underlying connection. Most callers should just use Close.
func (*Conn) ConnectionState

func (c *Conn) ConnectionState() ConnectionState

ConnectionState returns basic TLS details about the connection.
func (*Conn) Handshake

func (c *Conn) Handshake() error

Handshake runs the client or server handshake protocol if it has not yet been run. Most uses of this package need not call Handshake explicitly: the first Read or Write will call it automatically.
func (*Conn) LocalAddr

func (c *Conn) LocalAddr() net.Addr

LocalAddr returns the local network address.
func (*Conn) OCSPResponse

func (c *Conn) OCSPResponse() []byte

OCSPResponse returns the stapled OCSP response from the TLS server, if any. (Only valid for client connections.)
func (*Conn) Read

func (c *Conn) Read(b []byte) (n int, err error)

Read can be made to time out and return a net.Error with Timeout() == true after a fixed time limit; see SetDeadline and SetReadDeadline.
func (*Conn) RemoteAddr

func (c *Conn) RemoteAddr() net.Addr

RemoteAddr returns the remote network address.
func (*Conn) SetDeadline

func (c *Conn) SetDeadline(t time.Time) error

SetDeadline sets the read and write deadlines associated with the connection. A zero value for t means Read and Write will not time out. After a Write has timed out, the TLS state is corrupt and all future writes will return the same error.
func (*Conn) SetReadDeadline

func (c *Conn) SetReadDeadline(t time.Time) error

SetReadDeadline sets the read deadline on the underlying connection. A zero value for t means Read will not time out.
func (*Conn) SetWriteDeadline

func (c *Conn) SetWriteDeadline(t time.Time) error

SetWriteDeadline sets the write deadline on the underlying connection. A zero value for t means Write will not time out. After a Write has timed out, the TLS state is corrupt and all future writes will return the same error.
func (*Conn) VerifyHostname

func (c *Conn) VerifyHostname(host string) error

VerifyHostname checks that the peer certificate chain is valid for connecting to host. If so, it returns nil; if not, it returns an error describing the problem.
func (*Conn) Write

func (c *Conn) Write(b []byte) (int, error)

Write writes data to the connection.
type ConnectionState

ConnectionState records basic TLS details about the connection.

type ConnectionState struct {
        Version                     uint16                // TLS version used by the connection (e.g. VersionTLS12)
        HandshakeComplete           bool                  // TLS handshake is complete
        DidResume                   bool                  // connection resumes a previous TLS connection
        CipherSuite                 uint16                // cipher suite in use (TLS_RSA_WITH_RC4_128_SHA, ...)
        NegotiatedProtocol          string                // negotiated next protocol (from Config.NextProtos)
        NegotiatedProtocolIsMutual  bool                  // negotiated protocol was advertised by server
        ServerName                  string                // server name requested by client, if any (server side only)
        PeerCertificates            []*x509.Certificate   // certificate chain presented by remote peer
        VerifiedChains              [][]*x509.Certificate // verified chains built from PeerCertificates
        SignedCertificateTimestamps [][]byte              // SCTs from the server, if any
        OCSPResponse                []byte                // stapled OCSP response from server, if any

        // TLSUnique contains the "tls-unique" channel binding value (see RFC
        // 5929, section 3). For resumed sessions this value will be nil
        // because resumption does not include enough context (see
        // https://secure-resumption.com/#channelbindings). This will change in
        // future versions of Go once the TLS master-secret fix has been
        // standardized and implemented.
        TLSUnique []byte
}

type CurveID

CurveID is the type of a TLS identifier for an elliptic curve. See http://www.iana.org/assignments/tls-parameters/tls-parameters.xml#tls-parameters-8

type CurveID uint16

const (
        CurveP256 CurveID = 23
        CurveP384 CurveID = 24
        CurveP521 CurveID = 25
        X25519    CurveID = 29
)

type RecordHeaderError

RecordHeaderError results when a TLS record header is invalid.

type RecordHeaderError struct {
        // Msg contains a human readable string that describes the error.
        Msg string
        // RecordHeader contains the five bytes of TLS record header that
        // triggered the error.
        RecordHeader [5]byte
}

func (RecordHeaderError) Error

func (e RecordHeaderError) Error() string

type RenegotiationSupport

RenegotiationSupport enumerates the different levels of support for TLS renegotiation. TLS renegotiation is the act of performing subsequent handshakes on a connection after the first. This significantly complicates the state machine and has been the source of numerous, subtle security issues. Initiating a renegotiation is not supported, but support for accepting renegotiation requests may be enabled.

Even when enabled, the server may not change its identity between handshakes (i.e. the leaf certificate must be the same). Additionally, concurrent handshake and application data flow is not permitted so renegotiation can only be used with protocols that synchronise with the renegotiation, such as HTTPS.

type RenegotiationSupport int

const (
        // RenegotiateNever disables renegotiation.
        RenegotiateNever RenegotiationSupport = iota

        // RenegotiateOnceAsClient allows a remote server to request
        // renegotiation once per connection.
        RenegotiateOnceAsClient

        // RenegotiateFreelyAsClient allows a remote server to repeatedly
        // request renegotiation.
        RenegotiateFreelyAsClient
)

type SignatureScheme

SignatureScheme identifies a signature algorithm supported by TLS. See https://tools.ietf.org/html/draft-ietf-tls-tls13-18#section-4.2.3.

type SignatureScheme uint16

const (
        PKCS1WithSHA1   SignatureScheme = 0x0201
        PKCS1WithSHA256 SignatureScheme = 0x0401
        PKCS1WithSHA384 SignatureScheme = 0x0501
        PKCS1WithSHA512 SignatureScheme = 0x0601

        PSSWithSHA256 SignatureScheme = 0x0804
        PSSWithSHA384 SignatureScheme = 0x0805
        PSSWithSHA512 SignatureScheme = 0x0806

        ECDSAWithP256AndSHA256 SignatureScheme = 0x0403
        ECDSAWithP384AndSHA384 SignatureScheme = 0x0503
        ECDSAWithP521AndSHA512 SignatureScheme = 0x0603
)

Bugs

    ☞

    The crypto/tls package only implements some countermeasures against Lucky13 attacks on CBC-mode encryption, and only on SHA1 variants. See http://www.isg.rhul.ac.uk/tls/TLStiming.pdf and https://www.imperialviolet.org/2013/02/04/luckythirteen.html.
