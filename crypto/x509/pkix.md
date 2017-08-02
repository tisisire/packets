
 Package pkix

    import "crypto/x509/pkix"

    Overview
    Index

Overview ▾

Package pkix contains shared, low level structures used for ASN.1 parsing and serialization of X.509 certificates, CRL and OCSP.
Index ▾

    type AlgorithmIdentifier
    type AttributeTypeAndValue
    type AttributeTypeAndValueSET
    type CertificateList
        func (certList *CertificateList) HasExpired(now time.Time) bool
    type Extension
    type Name
        func (n *Name) FillFromRDNSequence(rdns *RDNSequence)
        func (n Name) ToRDNSequence() (ret RDNSequence)
    type RDNSequence
    type RelativeDistinguishedNameSET
    type RevokedCertificate
    type TBSCertificateList

Package files

pkix.go
type AlgorithmIdentifier

AlgorithmIdentifier represents the ASN.1 structure of the same name. See RFC 5280, section 4.1.1.2.

type AlgorithmIdentifier struct {
        Algorithm  asn1.ObjectIdentifier
        Parameters asn1.RawValue `asn1:"optional"`
}

type AttributeTypeAndValue

AttributeTypeAndValue mirrors the ASN.1 structure of the same name in http://tools.ietf.org/html/rfc5280#section-4.1.2.4

type AttributeTypeAndValue struct {
        Type  asn1.ObjectIdentifier
        Value interface{}
}

type AttributeTypeAndValueSET

AttributeTypeAndValueSET represents a set of ASN.1 sequences of AttributeTypeAndValue sequences from RFC 2986 (PKCS #10).

type AttributeTypeAndValueSET struct {
        Type  asn1.ObjectIdentifier
        Value [][]AttributeTypeAndValue `asn1:"set"`
}

type CertificateList

CertificateList represents the ASN.1 structure of the same name. See RFC 5280, section 5.1. Use Certificate.CheckCRLSignature to verify the signature.

type CertificateList struct {
        TBSCertList        TBSCertificateList
        SignatureAlgorithm AlgorithmIdentifier
        SignatureValue     asn1.BitString
}

func (*CertificateList) HasExpired

func (certList *CertificateList) HasExpired(now time.Time) bool

HasExpired reports whether now is past the expiry time of certList.
type Extension

Extension represents the ASN.1 structure of the same name. See RFC 5280, section 4.2.

type Extension struct {
        Id       asn1.ObjectIdentifier
        Critical bool `asn1:"optional"`
        Value    []byte
}

type Name

Name represents an X.509 distinguished name. This only includes the common elements of a DN. When parsing, all elements are stored in Names and non-standard elements can be extracted from there. When marshaling, elements in ExtraNames are appended and override other values with the same OID.

type Name struct {
        Country, Organization, OrganizationalUnit []string
        Locality, Province                        []string
        StreetAddress, PostalCode                 []string
        SerialNumber, CommonName                  string

        Names      []AttributeTypeAndValue
        ExtraNames []AttributeTypeAndValue
}

func (*Name) FillFromRDNSequence

func (n *Name) FillFromRDNSequence(rdns *RDNSequence)

func (Name) ToRDNSequence

func (n Name) ToRDNSequence() (ret RDNSequence)

type RDNSequence

type RDNSequence []RelativeDistinguishedNameSET

type RelativeDistinguishedNameSET

type RelativeDistinguishedNameSET []AttributeTypeAndValue

type RevokedCertificate

RevokedCertificate represents the ASN.1 structure of the same name. See RFC 5280, section 5.1.

type RevokedCertificate struct {
        SerialNumber   *big.Int
        RevocationTime time.Time
        Extensions     []Extension `asn1:"optional"`
}

type TBSCertificateList

TBSCertificateList represents the ASN.1 structure of the same name. See RFC 5280, section 5.1.

type TBSCertificateList struct {
        Raw                 asn1.RawContent
        Version             int `asn1:"optional,default:0"`
        Signature           AlgorithmIdentifier
        Issuer              RDNSequence
        ThisUpdate          time.Time
        NextUpdate          time.Time            `asn1:"optional"`
        RevokedCertificates []RevokedCertificate `asn1:"optional"`
        Extensions          []Extension          `asn1:"tag:0,optional,explicit"`
}
