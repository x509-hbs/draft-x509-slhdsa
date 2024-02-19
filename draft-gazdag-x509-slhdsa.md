---
title: "Internet X.509 Public Key Infrastructure: Algorithm Identifiers for SLH-DSA"
abbrev: "SLH-DSA for X.509"
category: std
stand_alone: true # This lets us do fancy auto-generation of references
ipr: trust200902

docname: draft-gazdag-x509-slhdsa-latest
submissiontype: IETF
v: 3
# area: sec
# workgroup: LAMPS - Limited Additional Mechanisms for PKIX and SMIME
keyword:
 - SLH-DSA
 - SPHINCS+
 - PQ Signatures
 - post-quantum X.509
venue:
#  group: LAMPS
#  type: Working Group
#  mail: spasm@ietf.org
#  arch: "https://mailarchive.ietf.org/arch/browse/spasm/"
  github: "x509-hbs/draft-x509-slhdsa"

author:
-
    ins: K. Bashiri
    name: Kaveh Bashiri
    org: BSI
    email: kaveh.bashiri.ietf@gmail.com
-
    ins: S. Fluhrer
    name: Scott Fluhrer
    org: Cisco Systems
    email: sfluhrer@cisco.com
-
    ins: S. Gazdag
    name: Stefan Gazdag
    org: genua GmbH
    email: ietf@gazdag.de
-
    ins: D. Van Geest
    name: Daniel Van Geest
    org: CryptoNext Security
    street: 16, Boulevard Saint-Germain
    city: Paris
    country: France
    code: 75005
    email: daniel.vangeest.ietf@gmail.com
-
    ins: S. Kousidis
    name: Stavros Kousidis
    org: BSI
    email: kousidis.ietf@gmail.com

normative:
  FIPS205:
      title: TBD
  FIPS205-ipd:
      title: "Stateless Hash-Based Digital Signature Standard"
      date: 2023-08-24
      target: https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.205.ipd.pdf
      author:
        org: National Institute of Standards and Technology (NIST)

informative:
  NIST-PQC:
    target: https://csrc.nist.gov/projects/post-quantum-cryptography
    title: Post-Quantum Cryptography Project
    author:
      - org: National Institute of Standards and Technology
    date: 2016-12-20


--- abstract

Digital signatures are used within X.509 certificates, Certificate Revocation Lists (CRLs), and to sign messages.  This document describes the conventions for using the Stateless Hash-Based Digital Signature Standard (SLH-DSA) in Internet X.509 certificates and certificate revocation lists.  The conventions for the associated signatures, subject public keys, and private key are also described.

\[EDNOTE: This draft is not expected to be finalized before the NIST PQC Project has standardized FIPS 205 Stateless Hash-Based Digital Signature Standard.  The current FIPS draft was published August 24, 2023 for public review.  Final versions are expected by April 2024. This specification will use object identifiers for the new algorithms that are assigned by NIST, and will use placeholders until these are released.]

<!-- End of Abstract -->

--- middle

# Introduction

Stateless Hash-Based Digital Signatures (SLH-DSA) is a quantum-resistant digital signature scheme standardized in {{FIPS205}} \[EDNOTE: {{FIPS205-ipd}} until officially published] by the US National Institute of Standards and Technology (NIST) PQC project {{NIST-PQC}}. This document specifies the use of the SLH-DSA algorithm in Public Key Infrastructure X.509 (PKIX) certificates and Certificate Revocation Lists (CRLs).

SLH-DSA offers three security levels.  The parameters for each of the security levels were chosen to provide 128 bits of security, 192 bits of security, and 256 bits of security.  A separate algorithm identifier has been assigned for SLH-DSA at each of these security levels.

\[EDNOTE: TODO: sha2 vs shake, fast vs small]

This specification includes conventions for the signatureAlgorithm, signatureValue, signature, and subjectPublicKeyInfo fields within Internet X.509 certificates and CRLs {{!RFC5280}}, like {{?RFC3279}} did for classic cryptography and {{?RFC5480}} did for elliptic curve cryptography.  It describes the encoding of digital signatures and public keys generated with quantum-resistant signature algorithm SLH-DSA.

<!-- End of introduction section -->

# Conventions and Definitions

{::boilerplate bcp14-tagged}

<!-- EDNOTE: [DVG] I added this to my own (unpublished) draft based on draft-ietf-lamps-cms-sphincs-plus. draft-ietf-lamps-dilithium-certificates, on which we are basing this draft doesn't have such an extensive overview section. If draft-ietf-lamps-cms-sphincs-plus gets reduced in scope and refers more to this draft, we could add this text.-->
<!--
# SLH-DSA Hash-based Signature Algorithm Overview

SLH-DSA is a hash-based signature scheme which consists of a few time signature construction, namely Forest of Random Subsets (FORS) and a hypertree.  FORS signs a message with a private key.  The corresponding FORS public keys are the leaves in k binary trees.  The roots of these trees are hashed together to form a FORS root.  SLH-DSA uses a one-time signature scheme called WOTS+. The FORS tree roots are signed by a WOTS+ one-time signature private key.  The corresponding WOTS+ public keys form the leaves in d-layers of Merkle subtrees in the SLH-DSA hypertree.  The bottom layer of that hypertree signs the FORS roots with WOTS+. The root of the bottom Merkle subtrees are then signed with WOTS+ and the corresponding WOTS+ public keys form the leaves of the next level up subtree.  Subtree roots are consequently signed by their corresponding subtree layers until we reach the top subtree.  The top layer subtree forms the hypertree root which is trusted at the verifier.

A SLH-DSA signature consists of the FORS signature, the WOTS+ signature in each layer, and the path to the root of each subtree until the root of the hypertree is reached.

A SLH-DSA signature is verified by verifying the FORS signature, the WOTS+ signatures and the path to the root of each subtree.  When reaching the root of the hypertree, the signature verifies only if it hashes to the pre-trusted root of the SLH-DSA hypertree.

SLH-DSA is a stateless hash-based signature algorithm.  Stateful hash-based signature schemes require that the WOTS+ private key (generated by using a state index) is never reused or the scheme loses it security.  Although its security decreases, FORS which is used at the bottom of the SLH-DSA hypertree does not collapse if the same private key used to sign two or more different messages like in stateful hash-based signature schemes.  Without the need for state kept by the signer to ensure it is not reused, SLH-DSA is much less fragile.

SLH-DSA was designed to sign up to 2^64 messages and offers three security levels.  The parameters of the SLH-DSA hypertree include the security parameter, the hash function, the tree height, the number of layers of subtrees, the Winternitz parameter of WOTS+, the number of FORS trees and leaves in each.  The parameters for each of the security levels were chosen to provide 128 bits of security, 192 bits of security, and 256 bits of security.
-->

# Algorithm Identifiers {#sec-alg-ids}

This specification uses placeholders for object identifiers until the identifiers for the new algorithms are assigned by NIST.

The AlgorithmIdentifier type, which is included herein for convenience, is defined as follows:

~~~
    AlgorithmIdentifier  ::=  SEQUENCE  {
        algorithm   OBJECT IDENTIFIER,
        parameters  ANY DEFINED BY algorithm OPTIONAL
    }

    |  NOTE: The above syntax is from [RFC5280] and matches the
    |  version used therein, i.e., the 1988 ASN.1 syntax.  See
    |  [RFC5912] for ASN.1 copmatible with the 2015 ASN.1 syntax.
~~~

The fields in AlgorithmIdentifier have the following meanings:

* algorithm identifies the cryptographic algorithm with an object identifier.

* parameters, which are optional, are the associated parameters for the algorithm identifier in the algorithm field.

The OIDs are:

~~~
    id-alg-slh-dsa-128s-shake OBJECT IDENTIFIER ::= {
            joint-iso-itu-t(2) country(16) us(840) organization(1)
            gov(101) csor(3) nistAlgorithm(4) sigAlgs(3) TBD }

    id-alg-slh-dsa-128f-shake OBJECT IDENTIFIER ::= {
            joint-iso-itu-t(2) country(16) us(840) organization(1)
            gov(101) csor(3) nistAlgorithm(4) sigAlgs(3) TBD }

    id-alg-slh-dsa-128s-sha2 OBJECT IDENTIFIER ::= {
            joint-iso-itu-t(2) country(16) us(840) organization(1)
            gov(101) csor(3) nistAlgorithm(4) sigAlgs(3) TBD }

    id-alg-slh-dsa-128f-sha2 OBJECT IDENTIFIER ::= {
            joint-iso-itu-t(2) country(16) us(840) organization(1)
            gov(101) csor(3) nistAlgorithm(4) sigAlgs(3) TBD }

    id-alg-slh-dsa-192s-shake OBJECT IDENTIFIER ::= {
            joint-iso-itu-t(2) country(16) us(840) organization(1)
            gov(101) csor(3) nistAlgorithm(4) sigAlgs(3) TBD }

    id-alg-slh-dsa-192f-shake OBJECT IDENTIFIER ::= {
            joint-iso-itu-t(2) country(16) us(840) organization(1)
            gov(101) csor(3) nistAlgorithm(4) sigAlgs(3) TBD }

    id-alg-slh-dsa-256s-shake OBJECT IDENTIFIER ::= {
            joint-iso-itu-t(2) country(16) us(840) organization(1)
            gov(101) csor(3) nistAlgorithm(4) sigAlgs(3) TBD }

    id-alg-slh-dsa-256f-shake OBJECT IDENTIFIER ::= {
            joint-iso-itu-t(2) country(16) us(840) organization(1)
            gov(101) csor(3) nistAlgorithm(4) sigAlgs(3) TBD }
~~~

The contents of the parameters component for each algorithm are absent.

# SLH-DSA Signatures in PKIX

SLH-DSA is a digital signature scheme built upon hash functions. The security of SLH-DSA relies on the presumed diffculty of finding preimages for hash functions as well as several related properties of the same hash functions.

Signatures are used in a number of different ASN.1 structures.  As shown in the ASN.1 representation from {{!RFC5280}} below, in an X.509 certificate, a signature is encoded with an algorithm identifier in the signatureAlgorithm attribute and a signatureValue attribute that contains the actual signature.

~~~
    Certificate  ::=  SEQUENCE  {
        tbsCertificate       TBSCertificate,
        signatureAlgorithm   AlgorithmIdentifier,
        signatureValue       BIT STRING  }
~~~

Signatures are also used in the CRL list ASN.1 representation from {{!RFC5280}} below.  In a X.509 CRL, a signature is encoded with an algorithm identifier in the signatureAlgorithm attribute and a signatureValue attribute that contains the actual signature.

~~~
    CertificateList  ::=  SEQUENCE  {
        tbsCertificate       TBSCertList,
        signatureAlgorithm   AlgorithmIdentifier,
        signatureValue       BIT STRING  }
~~~

The identifiers defined in {{sec-alg-ids}} can be used as the AlgorithmIdentifier in the signatureAlgorithm field in the sequence Certificate/CertificateList and the signature field in the sequence TBSCertificate/TBSCertList in certificates CRLs, respectively, {{!RFC5280}}.  The parameters of these signature algorithms are absent, as explained in {{sec-alg-ids}}.

The signatureValue field contains the corresponding SLH-DSA signature computed upon the ASN.1 DER encoded tbsCertificate {{!RFC5280}}.

Conforming Certification Authority (CA) implementations MUST specify the algorithms explicitly by using the OIDs specified in {{sec-alg-ids}} when encoding SLH-DSA signatures in certificates and CRLs.  Conforming client implementations that process certificates and CRLs using SLH-DSA MUST recognize the corresponding OIDs. Encoding rules for SLH-DSA signature values are specified {{sec-alg-ids}}.

When any of the id-alg-slh-dsa-* identifiers appear in the algorithm field as an AlgorithmIdentifier, the encoding MUST omit the parameters field. That is, the AlgorithmIdentifier SHALL be a SEQUENCE of one component, the id-alg-slh-dsa-* OID.

# SLH-DSA Public Keys in PKIX {#sec-pub-keys}

 In the X.509 certificate, the subjectPublicKeyInfo field has the SubjectPublicKeyInfo type, which has the following ASN.1 syntax:

~~~
    SubjectPublicKeyInfo  ::=  SEQUENCE  {
        algorithm         AlgorithmIdentifier,
        subjectPublicKey  BIT STRING
    }
~~~

The fields in SubjectPublicKeyInfo have the following meanings:

* algorithm is the algorithm identifier and parameters for the public key (see above).

* subjectPublicKey contains the byte stream of the public key.

The SLH--DSA public key MUST be encoded using the ASN.1 type SLH-DSA-PublicKey:

~~~
    SLH-DSA-PublicKey ::= OCTET STRING
~~~

where SLH-DSA-PublicKey is a concatenation of the PK.seed and PK.root values as defined in Section 9.1 of {{FIPS205}}.  These parameters MUST be encoded as a
single OCTET STRING.  The size required to hold a SLH-DSA-PublicKey public key element is therefore 2*n bytes, where n is 16, 24, or 32, depending on the parameter set.

The id-alg-slh-dsa-* identifiers defined in {{sec-alg-ids}} MUST be used as the algorithm field in the SubjectPublicKeyInfo sequence {{!RFC5280}} to identify a SLH-DSA public key.

The SLH-DSA public key (a concatenation of seed and root that is an OCTET STRING) is mapped to a subjectPublicKey (a value of type BIT STRING) as follows: the most significant bit of the OCTET STRING value becomes the most significant bit of the BIT STRING value, and so on; the least significant bit of the OCTET STRING becomes the least significant bit of the BIT STRING.

The following is an example of a \[TODO: pick an OID] public key encoded using the textual encoding defined in {{?RFC7468}}.

~~~
-----BEGIN PUBLIC KEY-----
TODO
-----END PUBLIC KEY-----
~~~

Conforming CA implementations MUST specify the X.509 public key algorithm explicitly by using the OIDs specified in {{sec-alg-ids}} when using SLH-DSA public keys in certificates and CRLs.  Conforming client implementations that process SLH-DSA public keys when processing certificates and CRLs MUST recognize the corresponding OIDs.

# Key Usage Bits

The intended application for the key is indicated in the keyUsage certificate extension; see Section 4.2.1.3 of {{!RFC5280}}.  If the keyUsage extension is present in a certificate that indicates an id-alg-slh-dsa-* identifier in the SubjectPublicKeyInfo, then the at least one of following MUST be present:

~~~
    digitalSignature; or
    nonRepudiation; or
    keyCertSign; or
    cRLSign.
~~~

Requirements about the keyUsage extension bits defined in {{!RFC5280}} still apply.

# SLH-DSA Private Keys

<!-- Borrowed liberally from section 7 of RFC8410 -->
"Asymmetric Key Packages" {{!RFC5958}} describes how to encode a private key in a structure that both identifies what algorithm the private key is for and allows for the public key and additional attributes about the key to be included as well.  For illustration, the ASN.1 structure OneAsymmetricKey is replicated below.  The algorithm-specific details of how a private key is encoded are left for the document describing the algorithm itself.

~~~ asn1
   OneAsymmetricKey ::= SEQUENCE {
      version Version,
      privateKeyAlgorithm PrivateKeyAlgorithmIdentifier,
      privateKey PrivateKey,
      attributes [0] IMPLICIT Attributes OPTIONAL,
      ...,
      [[2: publicKey [1] IMPLICIT PublicKey OPTIONAL ]],
      ...
   }

   PrivateKey ::= OCTET STRING

   PublicKey ::= BIT STRING
~~~

An SLH-DSA private key consists of the concatenation of 4 n-byte elements, SK.seed, SK.prf, PK.seed and PK.root as defined in Section 9.1 of {{FIPS205}}.  The size required to hold an SLH-DSA-PrivateKey private key is therefore 4*n bytes, where n is 16, 24, or 32, depending on the parameter set.

For the keys defined in this document, the private key is always an opaque byte sequence.  The ASN.1 type SLH-DSA-PrivateKey is defined in this document to hold the byte sequence.  Thus, when encoding a OneAsymmetricKey object, the private key is wrapped in a SLH-DSA-PrivateKey object and wrapped by the OCTET STRING of the
"privateKey" field.

\[EDNOTE: the above paragraph is from RFC8410, and it reads to me like there's a double wrapping, i.e. OCTET STRING { OCTET STRING { private key bytes} }, however that's not the case.  Am I reading it wrong, or is the text unclear?]

~~~ asn1
   SLH-DSA-PrivateKey ::= OCTET STRING
~~~

To encode an SLH-DSA private key, the "privateKey" field will hold the encoded private key.  The "privateKeyAlgorithm" field uses the AlgorithmIdentifier structure.  The structure is encoded as defined above.  If present, the "publicKey" field will hold the encoded key as defined in {{sec-pub-keys}}.

The following is an example of a private key encoded using the textual encoding defined in {{!RFC7468}}.

~~~
-----BEGIN PRIVATE KEY-----
TODO
-----END PRIVATE KEY-----
~~~

NOTE: There exist some private key import functions that have not picked up the new ASN.1 structure OneAsymmetricKey that is defined in {{!RFC7748}}.  This means that they will not accept a private key structure that contains the public key field.  This means a balancing act needs to be done between being able to do a consistency check on the key pair and widest ability to import the key.

# ASN.1 Module

TODO: This is mostly copied from draft-ietf-lamps-cms-sphincs-plus; coordinate with those authors what goes in which draft.

TODO: Also do the proper module stuff. Again this is just here until we figure out how to do it right.

~~~~ asn.1
--
-- Object Identifiers
--

id-alg-slh-dsa-128s-shake OBJECT IDENTIFIER ::= { TBD }

id-alg-slh-dsa-128f-shake OBJECT IDENTIFIER ::= { TBD }

id-alg-slh-dsa-128s-sha2 OBJECT IDENTIFIER ::= { TBD }

id-alg-slh-dsa-128f-sha2 OBJECT IDENTIFIER ::= { TBD }

id-alg-slh-dsa-192s-shake OBJECT IDENTIFIER ::= { TBD }

id-alg-slh-dsa-192f-shake OBJECT IDENTIFIER ::= { TBD }

id-alg-slh-dsa-256s-shake OBJECT IDENTIFIER ::= { TBD }

id-alg-slh-dsa-256f-shake OBJECT IDENTIFIER ::= { TBD }

--
-- Signature Algorithm, Public Key, and Private Key
--

sa-slh-dsa-128s-shake SIGNATURE-ALGORITHM ::= {
    IDENTIFIER id-alg-slh-dsa-128s-shake
    PARAMS ARE absent
    PUBLIC-KEYS { pk-slh-dsa-128s-shake }
    SMIME-CAPS { IDENTIFIED BY id-alg-slh-dsa-128s-shake } }

sa-slh-dsa-128f-shake SIGNATURE-ALGORITHM ::= {
    IDENTIFIER id-alg-slh-dsa-128f-shake
    PARAMS ARE absent
    PUBLIC-KEYS { pk-slh-dsa-128f-shake }
    SMIME-CAPS { IDENTIFIED BY id-alg-slh-dsa-128f-shake } }

sa-slh-dsa-128s-sha2 SIGNATURE-ALGORITHM ::= {
    IDENTIFIER id-alg-slh-dsa-128s-sha2
    PARAMS ARE absent
    PUBLIC-KEYS { pk-slh-dsa-128s-sha2 }
    SMIME-CAPS { IDENTIFIED BY id-alg-slh-dsa-128s-sha2 } }

sa-slh-dsa-128f-sha2 SIGNATURE-ALGORITHM ::= {
    IDENTIFIER id-alg-slh-dsa-128f-sha2
    PARAMS ARE absent
    PUBLIC-KEYS { pk-slh-dsa-128f-sha2 }
    SMIME-CAPS { IDENTIFIED BY id-alg-slh-dsa-128f-sha2 } }

sa-slh-dsa-192s-shake SIGNATURE-ALGORITHM ::= {
    IDENTIFIER id-alg-slh-dsa-192s-shake
    PARAMS ARE absent
    PUBLIC-KEYS { pk-slh-dsa-192s-shake }
    SMIME-CAPS { IDENTIFIED BY id-alg-slh-dsa-192s-shake } }

sa-slh-dsa-192f-shake SIGNATURE-ALGORITHM ::= {
    IDENTIFIER id-alg-slh-dsa-192f-shake
    PARAMS ARE absent
    PUBLIC-KEYS { pk-slh-dsa-192f-shake }
    SMIME-CAPS { IDENTIFIED BY id-alg-slh-dsa-192f-shake } }

sa-slh-dsa-256s-shake SIGNATURE-ALGORITHM ::= {
    IDENTIFIER id-alg-slh-dsa-256s-shake
    PARAMS ARE absent
    PUBLIC-KEYS { pk-slh-dsa-256s-shake }
    SMIME-CAPS { IDENTIFIED BY id-alg-slh-dsa-256s-shake } }

sa-slh-dsa-256f-shake SIGNATURE-ALGORITHM ::= {
    IDENTIFIER id-alg-slh-dsa-256f-shake
    PARAMS ARE absent
    PUBLIC-KEYS { pk-slh-dsa-256f-shake }
    SMIME-CAPS { IDENTIFIED BY id-alg-slh-dsa-256f-shake } }

pk-slh-dsa-128s-shake PUBLIC-KEY ::= {
    IDENTIFIER id-alg-slh-dsa-128s-shake
    KEY SLH-DSA-PublicKey
    PARAMS ARE absent
    CERT-KEY-USAGE
        { digitalSignature, nonRepudiation, keyCertSign, cRLSign }
    PRIVATE-KEY SLH-DSA-PrivateKey }

pk-slh-dsa-128f-shake PUBLIC-KEY ::= {
    IDENTIFIER id-alg-slh-dsa-128f-shake
    KEY SLH-DSA-PublicKey
    PARAMS ARE absent
    CERT-KEY-USAGE
        { digitalSignature, nonRepudiation, keyCertSign, cRLSign }
    PRIVATE-KEY SLH-DSA-PrivateKey }

pk-slh-dsa-128s-sha2 PUBLIC-KEY ::= {
    IDENTIFIER id-alg-slh-dsa-128s-sha2
    KEY SLH-DSA-PublicKey
    PARAMS ARE absent
    CERT-KEY-USAGE
        { digitalSignature, nonRepudiation, keyCertSign, cRLSign }
    PRIVATE-KEY SLH-DSA-PrivateKey }

pk-slh-dsa-128f-sha2 PUBLIC-KEY ::= {
    IDENTIFIER id-alg-slh-dsa-128f-sha2
    KEY SLH-DSA-PublicKey
    PARAMS ARE absent
    CERT-KEY-USAGE
        { digitalSignature, nonRepudiation, keyCertSign, cRLSign }
    PRIVATE-KEY SLH-DSA-PrivateKey }

pk-slh-dsa-192s-shake PUBLIC-KEY ::= {
    IDENTIFIER id-alg-slh-dsa-192s-shake
    KEY SLH-DSA-PublicKey
    PARAMS ARE absent
    CERT-KEY-USAGE
        { digitalSignature, nonRepudiation, keyCertSign, cRLSign }
    PRIVATE-KEY SLH-DSA-PrivateKey }

pk-slh-dsa-192f-shake PUBLIC-KEY ::= {
    IDENTIFIER id-alg-slh-dsa-192f-shake
    KEY SLH-DSA-PublicKey
    PARAMS ARE absent
    CERT-KEY-USAGE
        { digitalSignature, nonRepudiation, keyCertSign, cRLSign }
    PRIVATE-KEY SLH-DSA-PrivateKey }

pk-slh-dsa-256s-shake PUBLIC-KEY ::= {
    IDENTIFIER id-alg-slh-dsa-256s-shake
    KEY SLH-DSA-PublicKey
    PARAMS ARE absent
    CERT-KEY-USAGE
        { digitalSignature, nonRepudiation, keyCertSign, cRLSign }
    PRIVATE-KEY SLH-DSA-PrivateKey }

pk-slh-dsa-256f-shake PUBLIC-KEY ::= {
    IDENTIFIER id-alg-slh-dsa-256f-shake
    KEY SLH-DSA-PublicKey
    PARAMS ARE absent
    CERT-KEY-USAGE
        { digitalSignature, nonRepudiation, keyCertSign, cRLSign }
    PRIVATE-KEY SLH-DSA-PrivateKey }

SLH-DSA-PublicKey ::= OCTET STRING

SLH-DSA-PrivateKey ::= OCTET STRING
~~~~

# Security Considerations

The security considerations of [RFC5280] applies accordingly.

TODO Security

# IANA Considerations

TODO

--- back

# Security Strengths

Instead of defining the strength of a quantum algorithm in a traditional manner using precise estimates of the number of bits of security, NIST has instead elected to define a collection of broad security strength categories.  Each category is defined by a comparatively easy-to-analyze reference primitive that cover a range of security strengths offered by existing NIST standards in symmetric cryptography, which NIST expects to offer significant resistance to quantum cryptanalysis.  These categories describe any attack that breaks the relevant security definition that must require computational resources comparable to or greater than those required for: Level 1 - key search on a block cipher with a 128-bit key (e.g., AES128), Level 2 - collision search on a 256-bit hash function (e.g., SHA256/ SHA3-256), Level 3 - key search on a block cipher with a 192-bit key (e.g., AES192), Level 4 - collision search on a 384-bit hash function (e.g.  SHA384/SHA3-384), Level 5 - key search on a block cipher with a 256-bit key (e.g., AES 256).

The parameter sets defined for NIST security levels 1, 3 and 5 are listed in {{tab-strengths}}, along with the resulting signature size, public key, and private key sizes in bytes.

| OID                       | NIST Level | Sig. (B) | Public Key (B) | Private Key (B) |
|---                        |---         |---       |---             |---              |
| id-alg-slh-dsa-128s-shake | 1          | TODO     | TODO           | TODO            |
| id-alg-slh-dsa-128f-shake | 1          | TODO     | TODO           | TODO            |
| id-alg-slh-dsa-128s-sha2  | 1          | TODO     | TODO           | TODO            |
| id-alg-slh-dsa-128f-sha2  | 1          | TODO     | TODO           | TODO            |
| id-alg-slh-dsa-192s-shake | 3          | TODO     | TODO           | TODO            |
| id-alg-slh-dsa-192f-shake | 3          | TODO     | TODO           | TODO            |
| id-alg-slh-dsa-256s-shake | 5          | TODO     | TODO           | TODO            |
| id-alg-slh-dsa-256f-shake | 5          | TODO     | TODO           | TODO            |
{: #tab-strengths title="SLH-DSA security strengths"}

# Acknowledgments
{:numbered="false"}

Much of the structure and text of this document is based on {{?I-D.ietf-lamps-dilithium-certificates}}. The remainder comes from {{?I-D.draft-ietf-lamps-cms-sphincs-plus}}. Thanks to those authors, and the ones they based their work on, for making our work earier. "Copying always makes things easier and less error prone" - {{?RFC8411}}.

TODO: Hopefully others will help out.  They will be acknowledged here.  And if you've read this far...
