
 Package mail

    import "net/mail"

    Overview
    Index
    Examples

Overview ▾

Package mail implements parsing of mail messages.

For the most part, this package follows the syntax as specified by RFC 5322 and extended by RFC 6532. Notable divergences:

* Obsolete address formats are not parsed, including addresses with
  embedded route information.
* Group addresses are not parsed.
* The full range of spacing (the CFWS syntax element) is not supported,
  such as breaking addresses across lines.
* No unicode normalization is performed.

Index ▾

    Variables
    func ParseAddressList(list string) ([]*Address, error)
    func ParseDate(date string) (time.Time, error)
    type Address
        func ParseAddress(address string) (*Address, error)
        func (a *Address) String() string
    type AddressParser
        func (p *AddressParser) Parse(address string) (*Address, error)
        func (p *AddressParser) ParseList(list string) ([]*Address, error)
    type Header
        func (h Header) AddressList(key string) ([]*Address, error)
        func (h Header) Date() (time.Time, error)
        func (h Header) Get(key string) string
    type Message
        func ReadMessage(r io.Reader) (msg *Message, err error)

Examples

    ParseAddress
    ParseAddressList
    ReadMessage

Package files

message.go
Variables

var ErrHeaderNotPresent = errors.New("mail: header not in message")

func ParseAddressList

func ParseAddressList(list string) ([]*Address, error)

ParseAddressList parses the given string as a list of addresses.

▹ Example
func ParseDate

func ParseDate(date string) (time.Time, error)

ParseDate parses an RFC 5322 date string.
type Address

Address represents a single mail address. An address such as "Barry Gibbs <bg@example.com>" is represented as Address{Name: "Barry Gibbs", Address: "bg@example.com"}.

type Address struct {
        Name    string // Proper name; may be empty.
        Address string // user@domain
}

func ParseAddress

func ParseAddress(address string) (*Address, error)

Parses a single RFC 5322 address, e.g. "Barry Gibbs <bg@example.com>"

▹ Example
func (*Address) String

func (a *Address) String() string

String formats the address as a valid RFC 5322 address. If the address's name contains non-ASCII characters the name will be rendered according to RFC 2047.
type AddressParser

An AddressParser is an RFC 5322 address parser.

type AddressParser struct {
        // WordDecoder optionally specifies a decoder for RFC 2047 encoded-words.
        WordDecoder *mime.WordDecoder
}

func (*AddressParser) Parse

func (p *AddressParser) Parse(address string) (*Address, error)

Parse parses a single RFC 5322 address of the form "Gogh Fir <gf@example.com>" or "foo@example.com".
func (*AddressParser) ParseList

func (p *AddressParser) ParseList(list string) ([]*Address, error)

ParseList parses the given string as a list of comma-separated addresses of the form "Gogh Fir <gf@example.com>" or "foo@example.com".
type Header

A Header represents the key-value pairs in a mail message header.

type Header map[string][]string

func (Header) AddressList

func (h Header) AddressList(key string) ([]*Address, error)

AddressList parses the named header field as a list of addresses.
func (Header) Date

func (h Header) Date() (time.Time, error)

Date parses the Date header field.
func (Header) Get

func (h Header) Get(key string) string

Get gets the first value associated with the given key. It is case insensitive; CanonicalMIMEHeaderKey is used to canonicalize the provided key. If there are no values associated with the key, Get returns "". To access multiple values of a key, or to use non-canonical keys, access the map directly.
type Message

A Message represents a parsed mail message.

type Message struct {
        Header Header
        Body   io.Reader
}

func ReadMessage

func ReadMessage(r io.Reader) (msg *Message, err error)

ReadMessage reads a message from r. The headers are parsed, and the body of the message will be available for reading from r.

▹ Example
