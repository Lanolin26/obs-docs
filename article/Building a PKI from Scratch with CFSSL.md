---
source: https://blog.laisky.com/p/cfssl/#gsc.tab=0
---

Building a PKI from Scratch with CFSSL
======================================

Ⅰ、Overview
-----------

This document will guide you through the process of building a complete PKI from scratch using cfssl. This includes issuing root certificates (rootCA), intermediate certificates, leaf certificates, and implementing certificate revocation lists (CRL).

![x509-chain.drawio.png](https://s3.laisky.com/uploads/2022/09/x509-chain.drawio.png)

Ⅱ、Background
-------------

### 1、Certificate Encoding Formats

-   `PEM (Privacy Enhanced Mail)`

Typically used for Certificate Authorities (CA), with extensions like .pem, .crt, .cer, and .key. Content is Base64 encoded ASCII files, marked with headers like `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----`. Server authentication certificates, intermediate certificates, and private keys are commonly stored in PEM format. Servers like Apache and nginx use PEM format certificates.

-   `DER (Distinguished Encoding Rules)`

Differs from PEM in that it uses binary instead of Base64 encoded ASCII. It has the extension `.der` but is also often used with `.cer` as the extension.

Ⅲ、Installation
---------------

For Go &gt;= 1.18, you can directly install it (other installation methods can be found on the [official repo](https://github.com/cloudflare/cfssl)):

    go install github.com/cloudflare/cfssl/cmd/...@latest

Copy

Ⅳ、Initialization
-----------------

    cfssl print-defaults config > config.json

Copy

This will generate a default configuration file `config.json`. Modify it according to your requirements:

(More details can be found in the [official code repository](https://github.com/cloudflare/cfssl/blob/bba3a2015ca4ad91f5b2c61c5876f0ecbfcb39bc/config/config.go#L77))

    {
      "signing": {
        "default": {
          "expiry": "87600h",
          "crl_url": "https://s3.laisky.com/public/laisky.crl"
        },
        "profiles": {
          "leaf": {
            "expiry": "87600h",
            "usages": ["signing", "key encipherment", "server auth"]
          },
          "intermediate": {
            "expiry": "87600h",
            "usages": ["cert sign", "crl sign"],
            "ca_constraint": {
              "is_ca": true
            }
          }
        }
      }
    }

Copy

The `profiles` section can contain multiple configurations:

-   `intermediate`: Represents intermediate CA, used for issuing the next level CA or leaf certificates, generally not used for encryption.
-   `leaf`: Leaf certificate, used only for encryption.

Ⅴ、Issuing Root Certificate
---------------------------

    # Generate csr.json template
    cfssl print-defaults csr > csr.json

Copy

Modify the `csr.json` slightly:

(The most important parts are `CN`, `hosts`, and `key`)

    {
      "CN": "laisky.com",
      "hosts": ["laisky.com", "*.laisky.com"],
      "key": {
        "algo": "ecdsa",
        "size": 256
      },
      "names": [
        {
          "C": "CN",
          "ST": "SH",
          "L": "Shanghai",
          "O": "Laisky"
        }
      ],
      "ca": {
        "expiry": "87600h"
      }
    }

Copy

Field descriptions can be found at [Creating a new CSR](https://github.com/cloudflare/cfssl/wiki/Creating-a-new-CSR):

-   `CN`: commonName, generally used as a username (should generally be unique)
-   `O`: organizationName, generally used as a user group name
-   `OU`: organisationalUnit
-   `C`: country, country
-   `L`: localityName, city or town name
-   `ST`: stateOrProvinceName, state/province name
-   `STREET`: streetAddress
-   `DC`: domainComponent

Signing the root certificate:

    cfssl genkey -initca csr.json | cfssljson -bare ca

Copy

This will generate three files:

-   ca-key.pem: private key
-   ca.csr: CSR file used to generate the certificate
-   ca.pem: certificate

> ℹ️ The process for generating the certificate is as follows:
>
> 1.  Generate a random private key
> 2.  Write the CSR file
> 3.  Use the private key to sign the CSR and generate the certificate

Ⅵ、Sign a secondary CA with rootCA
----------------------------------

Create an `l2` folder to place files related to the secondary certificate.

    mkdir l2
    cd l2

Copy

> ℹ️ Certificates can be either regular certificates or CA certificates. CA certificates can be used to issue subordinate certificates, while regular certificates can only be used for encryption negotiation.
>
> Specific fields in `X509v3 extensions:` include:
>
> -   `X509v3 Key Usage:`
>     -   CA certificate: `Certificate Sign, CRL Sign`
>     -   Regular certificate: `Digital Signature, Key Encipherment`
> -   `X509v3 Basic Constraints:`
>     -   CA certificate: `CA:TRUE`
>     -   Regular certificate: `CA:FALSE`

First, prepare `csr.json`:

    {
      "CN": "l2.laisky.com",
      "hosts": ["l2.laisky.com", "*.l2.laisky.com"],
      "key": {
        "algo": "ecdsa",
        "size": 256
      },
      "names": [
        {
          "C": "CN",
          "ST": "SH",
          "L": "Shanghai",
          "O": "Laisky"
        }
      ]
    }

Copy

The process of issuing an intermediate certificate with cfssl is slightly different from traditional openssl operations:

1.  The traditional way is to generate a CSR for a CA certificate and hand it over to the higher-level CA for signing
2.  cfssl generates a regular CSR, and the higher-level CA generates a CA or leaf certificate based on the configuration in `config.json`. (If there are special requirements, a CA CSR can also be generated with the `-initca` parameter using `genkey`)

    # Generate private key and CSR
    cfssl genkey -config ../config.json -profile intermediate csr.json | cfssljson -bare

    # If you already have a private key, you can also generate only the CSR
    cfssl gencsr -key cert-key.pem csr.json | cfssljson -bare

Copy

This will generate two files:

-   `cert-key.pem`: private key
-   `cert.csr`: certificate CSR

Review the CSR content:

(If there is no `CA:TRUE`, it indicates that it is a regular CSR for applying for a leaf certificate. However, this is not a problem, as cfssl will determine this when actually issuing the certificate based on the configuration file)

    ✗ openssl req -text -noout -verify -in cert.csr
    # Alternatively, you can use cfssl certinfo -csr cert.csr

    ✗ openssl req -text -noout -verify -in cert.csr
    Certificate request self-signature verify OK
    Certificate Request:
        Data:
            Version: 1 (0x0)
            Subject: C = CN, ST = SH, L = Shanghai, O = Laisky, CN = l2.laisky.com
            Subject Public Key Info:
                Public Key Algorithm: id-ecPublicKey
                    Public-Key: (256 bit)
                    pub:
                        04:db:54:d1:ba:e6:10:86:f0:84:1d:27:aa:46:75:
                        99:c3:09:33:03:65:4b:1e:24:bc:85:34:3a:4c:d4:
                        b7:84:f3:0c:a3:dd:41:27:4a:34:e0:d4:a0:1a:a8:
                        cf:a3:cf:a2:12:31:12:0b:78:f1:d4:da:5c:fc:5e:
                        9b:03:bb:c4:b2
                    ASN1 OID: prime256v1
                    NIST CURVE: P-256
            Attributes:
                Requested Extensions:
                    X509v3 Subject Alternative Name:
                        DNS:l2.laisky.com, DNS:*.l2.laisky.com
        Signature Algorithm: ecdsa-with-SHA256
        Signature Value:
            30:46:02:21:00:bd:0c:a6:fd:7d:da:6e:87:8a:cf:7a:77:b3:
            41:9e:e0:4d:a0:cf:0e:39:0a:f1:48:95:5a:57:c0:bb:13:f3:
            37:02:21:00:81:aa:a7:a2:da:01:0e:96:48:ff:d6:17:05:f9:
            aa:e9:59:56:c6:ff:3b:0f:ce:4a:f1:b6:4f:77:9c:41:f6:0a

Copy

ℹ️ x509 certificate chain:

1.  Intermediate CA generates its private key
2.  Intermediate CA writes the desired certificate content into a CSR and submits it to the higher-level CA (in this example, rootCA)
3.  The higher-level CA signs the next level’s certificate according to the CSR content using its private key and CA

Certificate issued by rootCA:

    cfssl sign \
        -ca ../ca.pem \
        -ca-key ../ca-key.pem \
        -config ../config.json \
        -profile intermediate \
        -csr cert.csr | cfssljson -bare

Copy

This will generate the `cert.pem` certificate file, and its content can be viewed using openssl:

(Note the `CA:TRUE`)

    ✗ openssl x509 -in cert.pem -noout -text
    # Alternatively, you can use cfssl certinfo -cert cert.pem to view


    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number:
                2b:dd:d6:75:ad:40:3c:a1:3e:fe:97:6d:63:15:44:24:6b:51:15:c5
            Signature Algorithm: ecdsa-with-SHA256
            Issuer: C = CN, ST = SH, L = Shanghai, CN = laisky.com
            Validity
                Not Before: Sep 13 02:18:00 2022 GMT
                Not After : Sep 10 02:18:00 2032 GMT
            Subject: C = CN, ST = SH, L = Shanghai, O = Laisky, CN = l2.laisky.com
            Subject Public Key Info:
                Public Key Algorithm: id-ecPublicKey
                    Public-Key: (256 bit)
                    pub:
                        04:db:54:d1:ba:e6:10:86:f0:84:1d:27:aa:46:75:
                        99:c3:09:33:03:65:4b:1e:24:bc:85:34:3a:4c:d4:
                        b7:84:f3:0c:a3:dd:41:27:4a:34:e0:d4:a0:1a:a8:
                        cf:a3:cf:a2:12:31:12:0b:78:f1:d4:da:5c:fc:5e:
                        9b:03:bb:c4:b2
                    ASN1 OID: prime256v1
                    NIST CURVE: P-256
            X509v3 extensions:
                X509v3 Key Usage: critical
                    Certificate Sign, CRL Sign
                X509v3 Basic Constraints: critical
                    CA:TRUE
                X509v3 Subject Key Identifier:
                    14:5B:C1:43:9E:0F:B2:02:50:58:CB:BC:0F:EA:FD:C2:9F:73:51:80
                X509v3 Authority Key Identifier:
                    63:4F:CF:04:42:60:CC:4A:C1:CA:CB:D6:36:77:BF:7C:1F:59:F7:FA
        Signature Algorithm: ecdsa-with-SHA256
        Signature Value:
            30:46:02:21:00:83:6a:41:55:90:b6:67:ae:91:18:f7:e6:8b:
            f9:89:91:ca:83:3d:43:7d:54:fe:15:f0:5d:7f:19:23:05:ef:
            9d:02:21:00:85:83:ee:7c:19:67:72:13:99:c9:ed:d9:d1:69:
            b9:e7:df:e7:27:c4:43:29:ad:e5:1c:97:67:66:2b:b7:1a:15

Copy

Ⅶ、Issuing Leaf Certificates
----------------------------

Using a CA, you can issue subordinate CAs or leaf certificates. Leaf certificates are used only for encryption and cannot be used to issue subordinate CAs or certificates.

    # Create ./l2/l3
    mkdir l3
    cd l3

    # Create csr.json
    cat <<< '
    {
        "CN": "l3.laisky.com",
        "hosts": [
            "l3.laisky.com",
            "*.l3.laisky.com"
        ],
        "key": {
            "algo": "ecdsa",
            "size": 256
        },
        "names": [
            {
                "C": "CN",
                "ST": "SH",
                "L": "Shanghai",
                "O": "Laisky"
            }
        ]
    }' > csr.json

    # Generate private key
    cfssl genkey \
     -config ../../config.json \
     -profile leaf csr.json | cfssljson -bare

    # Use l2 CA to issue l3 leaf certificate
    cfssl sign \
        -ca ../cert.pem \
        -ca-key ../cert-key.pem \
        -config ../../config.json \
        -profile leaf \
        -csr cert.csr | cfssljson -bare

Copy

Ⅷ、Certificate Verification
---------------------------

Using openssl:

    # Verify l2 CA with rootCA
    ✗ openssl verify -CAfile ca.pem l2/cert.pem
    l2/cert.pem: OK

    # Verify l3 certificate with rootCA and l2CA
    ✗ openssl verify -CAfile ca.pem --untrusted l2/cert.pem l2/l3/cert.pem
    l2/l3/cert.pem: OK

Copy

### 1、Configuring Certificate Chain for Server

[As mentioned earlier](https://www.notion.so/CFSSL-1c84441a2bcb4dfaa07d6805e31603d4), in order to verify the l3 certificate, the verifier needs all the CA certificates in the certificate chain. There are two ways to provide the complete certificate chain:

1.  The verifier prepares all the certificates and then verifies the leaf certificate.
2.  The tester prepares the complete certificate chain, and the verifier only needs to prepare the root CA.

This section mainly introduces the second approach, which is also the most common method in practice. In this approach, clients only need to add the rootCA in the new trust root to verify all derived intermediate and leaf certificates.

![20220913T094337Z.png](https://s3.laisky.com/uploads/2022/09/cert-chain.png)

(As shown, the server provides the complete certificate chain at once)

Server (tester) Go code:

    package main

    import (
        "crypto/tls"
        "crypto/x509"
        "encoding/pem"
        "log"
        "net/http"
        "os"
    )

    func panicErr(err error) {
        if err != nil {
            panic(err)
        }
    }

    func parseCert(cnt []byte) *x509.Certificate {
        blk, _ := pem.Decode(cnt)
        cert, err := x509.ParseCertificate(blk.Bytes)
        panicErr(err)
        return cert
    }

    func pem2der(p []byte) []byte {
        b, _ := pem.Decode(p)
        return b.Bytes
    }

    func main() {
        caCertPem, err := os.ReadFile("../ca.pem")
        panicErr(err)

        l2CertPem, err := os.ReadFile("../l2/cert.pem")
        panicErr(err)

        l3CertPem, err := os.ReadFile("../l2/l3/cert.pem")
        panicErr(err)
        // l3Cert := parseCert(l3CertPem)

        l3keyPem, err := os.ReadFile("../l2/l3/cert-key.pem")
        panicErr(err)
        l3key, err := x509.ParseECPrivateKey(pem2der(l3keyPem))
        panicErr(err)

        // Prepare the complete certificate chain, l3 crt -> l2 crt -> root CA
        tlsCfg := &tls.Config{
            Certificates: []tls.Certificate{
                {
                    Certificate: [][]byte{pem2der(l3CertPem), pem2der(l2CertPem), pem2der(caCertPem)},
                    PrivateKey:  l3key,
                },
            },
        }

        mux := http.NewServeMux()

        mux.HandleFunc("/hello", func(w http.ResponseWriter, r *http.Request) {
            w.Header().Set("Content-Type", "text/plain")
            w.Write([]byte("world\n"))
        })

        server := &http.Server{
            Handler:   mux,
            Addr:      ":10443",
            TLSConfig: tlsCfg,
        }
        if err := server.ListenAndServeTLS("", ""); err != nil {
            log.Println("exit, ", err.Error())
        }
        panicErr(err)
    }

Copy

Client (verifier) Go code:

    package main

    import (
        "crypto/tls"
        "crypto/x509"
        "encoding/pem"
        "io"
        "log"
        "net/http"
        "os"
    )

    func panicErr(err error) {
        if err != nil {
            panic(err)
        }
    }

    func parseCert(cnt []byte) *x509.Certificate {
        blk, _ := pem.Decode(cnt)
        cert, err := x509.ParseCertificate(blk.Bytes)
        panicErr(err)
        return cert
    }

    func main() {
        // As the server provides a complete certificate chain,
        // the tester only needs to pre-configure the rootCA
        caCertPem, err := os.ReadFile("../ca.pem")
        panicErr(err)

        capool := x509.NewCertPool()
        capool.AddCert(parseCert(caCertPem))

        cli := &http.Client{
            Transport: &http.Transport{
                TLSClientConfig: &tls.Config{
                    RootCAs: capool,
                },
            },
        }

        resp, err := cli.Get("https://l3.laisky.com:10443/hello")
        panicErr(err)
        defer resp.Body.Close()

        respBytes, err := io.ReadAll(resp.Body)
        panicErr(err)
        log.Printf("resp: %s", string(respBytes))
    }

Copy

Ⅸ、Revoking Certificates with CRL
---------------------------------

> ℹ️ Revoking certificates generally has two methods:
>
> 1.  OCSP: Provides a server-side interface for real-time requests
> 2.  CRL provides a CRL file listing the revoked certificates. The CRL file is generally provided in the form of a public internet URL and can be embedded in the certificate file. When published on the public network, it is in binary DER format.

![Untitled](https://s3.laisky.com/uploads/2022/09/crl-in-cert.png)

The steps to generate a CRL file using the cfssl tool are as follows:

1.  Get the serial number (big int) of the certificate to be revoked, convert it to a string, and save it to a text file, with each line corresponding to a serial number.
2.  Generate the CRL using cfssl, and then convert it to pem format using openssl

    # In this example, we will revoke the l3 certificate.
    # Extract the serial number of the l3 certificate to a text file
    #
    # It should be noted that tools like openssl usually represent the serial number in hexadecimal,
    # while cfssl uses decimal to represent the serial number.
    cfssl certinfo -cert l2/l3/cert.pem | jq .serial_number | tr -d '"' > crl.txt

    # Generate a binary DER file with cfssl
    # When publishing externally with URI, publish this DER file
    #
    # The CRL file can be set with an expiration period in seconds to remind the user to update the CRL file regularly.
    cfssl gencrl crl.txt ca.pem ca-key.pem 604800 | base64 -d > crl.der

    # Convert to pem format using openssl
    openssl crl -inform DER -out crl.pem

Copy

You can use openssl to view the contents of the crl:

    ✗ openssl crl -text -noout -in crl.pem
    # Alternatively, use `openssl crl -text -noout -in crl.der`

    Certificate Revocation List (CRL):
            Version 2 (0x1)
            Signature Algorithm: ecdsa-with-SHA256
            Issuer: C = CN, ST = SH, L = Shanghai, CN = laisky.com
            Last Update: Sep 13 06:51:36 2022 GMT
            Next Update: Sep 20 06:51:36 2022 GMT
            CRL extensions:
                X509v3 Authority Key Identifier:
                    63:4F:CF:04:42:60:CC:4A:C1:CA:CB:D6:36:77:BF:7C:1F:59:F7:FA
    Revoked Certificates:
        Serial Number: 533C693318EF7E13EED4846CEA5C63ECAE9F7713
            Revocation Date: Sep 13 06:51:36 2022 GMT
        Signature Algorithm: ecdsa-with-SHA256
        Signature Value:
            30:44:02:20:50:1b:27:c4:0b:23:63:4c:fb:c9:32:93:eb:17:
            bd:26:c3:9d:f5:94:ab:cc:82:22:13:4a:2d:b1:17:9f:41:08:
            02:20:59:ef:ba:34:f0:8c:00:0c:42:17:18:d7:e8:8b:a8:4d:
            f9:11:06:65:d1:70:61:9f:ce:e3:52:30:c3:2c:88:ea

Copy

You can upload the `crl.der` file to the public network and then inject the URL into the certificate (you can configure `crl_url` in `config.json`).

### 1、Golang

Even if the server certificate has a CRL URI set, the native Go client still won’t automatically check. At present, it seems that it can only be manually checked.

The comprehensive check method is as follows:

1.  Traverse the server certificate chain to obtain all `cert.CRLDistributionPoints` (CRL URI).
2.  Traverse and download all CRL files, load all `crl.TBSCertList.RevokedCertificates`.
3.  Use a double for loop to compare the serial numbers of the certificates in the certificate chain with the revocation list in the CRL one by one.

    // check cert by crl.pem
    //
    // prepare crl.txt
    //
    //    cfssl certinfo -cert l2/l3/cert.pem | jq .serial_number | tr -d '"' > crl.txt
    //
    // generate crl.pem:
    //
    //    cfssl gencrl crl.txt ca.pem ca-key.pem 604800 | base64 -d | openssl crl -inform DER -out crl.pem
    //
    // check CRL:
    //
    //    openssl crl -text -noout -in crl.pem
    func checkCRL(certs ...*x509.Certificate) {
        rawCRL, err := os.ReadFile("crl.pem")
        panicErr(err)

        crl, err := x509.ParseCRL(rawCRL)
    panicErr(err)

    if crl.TBSCertList.NextUpdate.Before(time.Now()) {
        panic("crl is expired")
    }

    for _, c := range certs {
        for _, rc := range crl.TBSCertList.RevokedCertificates {
            if c.SerialNumber.Cmp(rc.SerialNumber) == 0 {
                log.Panicf("cert revoked: %s - %s", c.DNSNames[0], c.SerialNumber.String())
            }
        }
    }

    log.Println("crl verified")
    }

Copy

Supplementary explanation about CRL: To achieve better compatibility, a CRL file specifically for a leaf certificate is usually issued by the same issuer that issued the leaf certificate. In other words, the issuer of the CRL is typically the same as the issuer of the leaf certificate. However, this is not mandatory; the issuer of the CRL can be any CA, or even not a CA (CRL doesn’t necessarily require the CA’s private key for signing). When the CRL and its issuer are not directly related to the certificates being revoked, it is referred to as an indirect CRL. For more details, refer to page 55 of RFC-5280.
