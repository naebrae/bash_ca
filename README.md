# Bash Certificate Authority scripts.

These scripts are working examples of using openssl without a configuration file for typical certificate authority tasks. 

They create three RSA based Certificate Authorities (CA) certificates with two optional CA's: the root CA 'Home Lab Root' and the two intermediate CA's 'Home Lab Full CA' and 'Home Lab Partial CA'. The optional CA's are 'Home Lab Windows CA' and an Elliptic Curve Cryptography 'Home Lab ECC CA'.

The multiple intermediate CA only exist so that the same subject certificates can be created but with different issuers. The issuer can be tested for in a client certificate for MySQL as an example of use.

The full_ scripts operate against the Full CA and use the 'openssl ca' option which creates a database of certificates signed. The database can be used with OSCP.

The part_ scripts operate against the Partial CA and use the 'openssl x509' option. This does not create a database and so can not be easily used with OSCP.

The win_ scripts operate against the option Windows CA and use the 'openssl ca' option which creates a database of certificates signed. They give examples of scripting extensions that are specific to Microsoft.

The ecc_ scripts operate against the option ECC CA and use the 'openssl ca' option which creates a database of certificates signed.


## 1. RunOnce scripts

Need to be run first and only once. 

### 1.1. mkrootca

Creates a cacerts folder with the CA certificate 'Home Lab Root' (root_ca.crt).

The certificate created replicates DigiCert certificates in terms of extensions or attributes. This is so that testing using these certificate models certificates used in production.

### 1.2. mkfullca

Creates a cacerts folder with the CA certificate 'Home Lab Full CA' (full_ca.crt). The certificate is signed with the root_ca certificate so mkrootca has to be run first.

The certificate created replicate DigiCert certificates in terms of extensions or attributes. This is so that testing using these certificate models certificates used in production.

### 1.3. mkpartca

Creates a cacerts folder with the CA certificate 'Home Lab Partial CA' (partial_ca.crt). The certificate is signed with the root_ca certificate so mkrootca has to be run first.

The certificates created replicate DigiCert certificates in terms of extensions or attributes. This is so that testing using these certificate models certificates used in production.

### 1.4. mkwinca

Creates the options CA 'Home Lab Windows CA' (win_ca.crt). Only run once if needed.

The certificates created replicate Microsoft ADCS certificates in terms of extensions or attributes. This is so that testing using these certificate models certificates used in production.
```
certutil -f -addstore Root root_ca.crt
```
```
Install-WindowsFeature ADCS-Cert-Authority,ADCS-Web-Enrollment -Includemanagementtools
```
```
Install-AdcsCertificationAuthority -AllowAdministratorInteraction -CAType StandaloneSubordinateCA `
-CertFile bashwin.p12 -CertFilePassword (ConvertTo-SecureString "changeit" -AsPlainText -Force) -Force
```
Note: The p12 file above has additional Microsoft features.

### 1.5. mkeccca

Creates the options CA 'Home Lab ECC CA' (ecc_ca.crt) with a prime256v1 curve key. Only run if needed.


## 2. Full CA scripts

These operate against the Full CA and use 'openssl ca'.

### 2.1. full_mkclncrt

Creates a certificate with a subject of /OU=Clients/CN= signed by the Full CA. Example use is MySQL client access.

### 2.2. full_mkclncsr

Creates a Certificate Signing Request with a subject of /OU=Clients/CN=

### 2.3. full_mkevcrt

Creates a CSR with Extended Validation (EV) subject extensions and signs it with the Full CA adding extensions consistant with a comercial CA.

The certificate this creates has the EV extra subject attributes only. A true EV certificate has its issuer certificate details added to the source code of the web browser so this certificate won't be recognised by a browers as an EV certificate.

### 2.4. full_mksrvcrt

Creates a CSR and signs it with the Full CA adding extensions consistant with a comercial CA.

Also creates the db folder in the current folder if is doesn't already exist to store the CA database files. The certificate created is recorded in the CA database (db/full_ca.idx).

### 2.5. full_mksrvcsr

Creates a basic Certificate Signing Request (CSR) without any request extensions.

### 2.6. full_ocspchk

Sends an OCSP request to 'localhost:2560' to check the status of the supplied certificate file.

### 2.7. full_ocspstart

Starts the 'openssl ocsp' process listening on port 2560 to respond to OCSP client requests.

### 2.8. full_revoke

Marks a certificate as revoked in the CA database (db/full_ca.idx). Takes it's certificate information from the supplied certificate file so the certificates doesn't have to exist in the database to be listed as revoked.

Also creates the db folder in the current folder if is doesn't already exist to store the CA database files. The certificate revoked is recorded in the CA database (db/full_ca.idx).

Generates a Certificate Revocation List (CRL) in the CA database folder (db/full_ca.crl).

### 2.9. full_signaddev

Signs the CSR with the Full CA certificate, ignoring any request extensions but adding those consistant with a comercial CA. Replaces the CSR subject with EV attributes.

Also creates the db folder in the current folder if is doesn't already exist to store the CA database files. The certificate created is recorded in the CA database (db/full_ca.idx).

### 2.10. full_signclncsr

### 2.11. full_signsrvcsr

Signs the CSR with the Full CA certificate, ignoring any request extensions but adding those consistant with a comercial CA.

Also creates the db folder in the current folder if is doesn't already exist to store the CA database files. The certificate created is recorded in the CA database (db/full_ca.idx).


## 3. Partial CA scripts

These operate against the Partial CA and use 'openssl x509'.

### 3.1. part_mkclncrt

Creates a CSR with a subject /OU=Clients/CN= and signs it with the Partial CA.

### 3.2. part_mkcrt

Creates a CSR and signs it with the Partial CA adding extensions consistant with a comercial CA.

### 3.3. part_mkcsr

### 3.4. part_mkeccrt

Creates a ECC CSR and signs it with the Partial CA adding extensions consistant with a comercial CA.

### 3.5. part_mkrpki

Creates a certificate with Resource Public Key Infrastructure extensions.

```
./part_mkrpki part4.lab.home 127.0.0.4
```

```
Certificate:
    Data:
        Version: 3 (0x2)
        Issuer: C = AU, O = Home Lab, CN = Home Lab Partial CA
        Subject: C = AU, ST = Western Australia, L = Perth, O = Home, CN = part4.lab.home
        X509v3 extensions:
            X509v3 Authority Key Identifier:
                52:A0:71:F7:9D:0A:C6:5E:2D:D0:9E:37:DE:BF:EC:51:C3:E8:A8:8D
            X509v3 Subject Key Identifier:
                25:E9:F8:ED:AF:CA:BC:80:47:5F:AA:D4:E1:27:C9:7E:98:9C:F0:CD
            X509v3 Subject Alternative Name:
                DNS:part4.lab.home, IP Address:127.0.0.4
            sbgp-autonomousSysNum: critical
                Autonomous System Numbers:
                  64512-65534

            sbgp-ipAddrBlock: critical
                IPv4:
                  10.0.0.0/8
                  172.16.0.0/20
                  192.168.0.0/16
                IPv6:
                  fd00::/8

            X509v3 Key Usage: critical
                Digital Signature, Key Encipherment
            X509v3 Extended Key Usage:
                TLS Web Server Authentication, TLS Web Client Authentication
            X509v3 Basic Constraints: critical
                CA:FALSE
            Authority Information Access:
                CA Issuers - URI:http://ca.lab.home/partial_ca.crt
            X509v3 Certificate Policies:
                Policy: X509v3 Any Policy
                  CPS: https://www.lab.home/CPS
                Policy: 2.23.140.1.2.3
```

### 3.6. part_mksan

Creates a certificate with a number of different Subject Alternate Names.
> Used as a example only how to add SAN details.

```
./part_mksan part5.lab.home 127.0.0.5
```
> $SAN = DNS:part5.lab.home,IP:127.0.0.5

```
subjectAltName=email:copy,$SAN,dirName:dir_O,dirName:dir_DC,email:root@localhost.localdomain,IP:127.0.0.1,IP:::01
[ dir_O ]
O=remote
OU=lab
CN=www
[ dir_DC ]
DC=Servers/DC=ad/DC=lab
+DC=remote
```

```
Certificate:
    Data:
        Version: 3 (0x2)
        Issuer: C = AU, O = Home Lab, CN = Home Lab Partial CA
        Subject: C = AU, ST = Western Australia, L = Perth, O = Home, CN = part5.lab.home
        X509v3 extensions:
            X509v3 Subject Alternative Name:
                DNS:part5.lab.home, IP Address:127.0.0.5, DirName:/O=remote/OU=lab/CN=www, DirName:/DC=remote+DC=Servers\/DC=ad\/DC=lab, email:root@localhost.localdomain, IP Address:127.0.0.1, IP Address:0:0:0:0:0:0:0:1
```

### 3.7. part_signcsr

Signs the CSR with the Partial CA certificate, ignoring any request extensions but adding those consistant with a comercial CA.


## 4. Windows CA scripts

These operate against the optional Windows CA and create certificates with Microsoft extensions.

### 4.1. win_dccrt

Creates a certificate with the extensions S/MIME, GUID in subjectAltName.

```
./win_dccrt win1.ad.lab.home 00000000-0000-0000-0000-000000000000

```

```
Certificate:
    Data:
        Version: 3 (0x2)
        Issuer: CN = Home Lab Windows CA
        Subject: CN = win1.ad.lab.home, OU = Domain Controllers, DC = ad, DC = lab, DC = home
        X509v3 extensions:
            1.3.6.1.4.1.311.20.2:
                . .D.o.m.a.i.n.C.o.n.t.r.o.l.l.e.r
            X509v3 Extended Key Usage:
                TLS Web Client Authentication, TLS Web Server Authentication
            X509v3 Key Usage:
                Digital Signature, Key Encipherment
            S/MIME Capabilities:
......0...`.H.e...*0...`.H.e...-0...`.H.e....0...`.H.e....0...+....0
..*.H..
            X509v3 Subject Alternative Name:
                othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:win1.ad.lab.home
            X509v3 Subject Key Identifier:
                39:D9:C2:E8:E1:11:A5:60:4B:6F:95:46:01:5D:D1:08:D2:D5:22:27
            X509v3 Authority Key Identifier:
                91:25:84:70:00:74:65:91:6C:BF:87:26:CD:7F:91:B6:2C:28:87:15
            X509v3 Basic Constraints:
                CA:FALSE
```

### 4.2. win_dccsr

Creates a CSR with Microsoft request extensions.

### 4.3. win_mkcsr

### 4.4. win_mkpfx

### 4.5. win_oldsign

Signs the CSR with the Windows CA certificate and copies the entensions from the CSR but creates an *Expired Certificate* for update testing.

### 4.6. win_signcsr

Signs the CSR with the Windows CA certificate and copies the entensions from the CSR.


### 5. Elliptic Curve Cryptography (ECC) CA scripts

These operate against the optional ECC CA and create ECDSA keys.

### 5.1. ecc_mkcrt

### 5.2. ecc_mkcsr

### 5.3. ecc_signcsr

