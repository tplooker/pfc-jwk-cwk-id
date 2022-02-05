%%%
title = "Barreto-Lynn-Scott Elliptic Curve Key Representations for JOSE and COSE"
ipr= "trust200902"
area = "Internet"
workgroup = "COSE"
submissiontype = "IETF"
keyword = ["COSE", "JOSE"]

[seriesInfo]
name = "Internet-Draft"
value = "draft-looker-bls-jose-cose-latest"
status = "standard"

[pi]
toc = "yes"

[[author]]
initials = "T."
surname = "Looker"
fullname = "Tobias Looker"
organization = "Mattr"
  [author.address]
  email = "tobias.looker@mattr.global"

[[author]]
initials = "M."
surname = "Jones"
fullname = "Michael B. Jones"
organization = "Microsoft"
  [author.address]
  email = "mike.jones@microsoft.com"
  uri = "https://self-issued.info/"

%%%

.# Abstract

This specification defines how to represent cryptographic keys for the pairing friendly elliptic curves known as Barreto-Lynn-Scott (BLS), for use with in the key representation formats of JSON Web Key (JWK) and COSE (COSE_Key).

{mainmatter}

# Introduction

This specification defines how to represent cryptographic keys for the pairing friendly elliptic curve known as Barreto-Lynn-Scott, for use within the key representation formats of JSON Web Key (JWK) and COSE (COSE_Key). The elliptic curves are registered in appropriate IANA JOSE and COSE registries.

There are a variety of important applications for pairing based cryptography including schemes already published as RFC's by the IETF such as Identity-Based Cryptography [@RFC5091] Sakai-Kasahara Key Encryption (SAKKE) [@RFC6508], and Identity-Based Authenticated Key Exchange (IBAKE) [@RFC6539]. SAKKE is applied to Multimedia Internet KEYing (MIKEY) [@RFC6509].

This branch of cryptography has also been used to develop numerous privacy preserving cryptographic hardware attestations schemes, including the Elliptic Curve Direct Anonymous Attestation (ECDAA) in the specification of a Trusted Platform Module [@TPM] specified by the Trusted Computing Group. Further work on a similar scheme has also been progressed at the FIDO Alliance and W3C. Similarly Intel released (EPID) which provides a solution to remote hardware attestation for Intel Software Guard Extension (SGX) enabled environments.

More recently, applications of pairing based cryptography using the Barreto-Lynn-Scott curves include the standardization effort for BLS Signatures [@!id.draft.bls-signature-04] which are used extensively in multiple blockchain projects due to their unique signature aggregation properties. Additionally, efforts to standardize the general purpose short group signature scheme of BBS Signatures which features novel properties such as multi-message signing and selective disclosure alongside zero knowledge proving.

There are multiple different pairing friendly curves in active use, however this draft focuses on a definition for the Barreto-Lynn-Scott curves due to them being the most "widely used" and "efficient" whilst achieving 128bit and 256bit security (BLS12-381 and BLS48-581 respectively).

More extensive discussion on the broader application of pairing based cryptography can be found in [@!id.draft.pairing-friendly-curves-10].

# Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14 [@!RFC2119] [@RFC8174] when, and only when, they appear in all capitals, as shown here.

## Representation Definition

The following definitions apply to the pairing friendly elliptic curves known as the Barreto-Lynn-Scott (BLS) curves.

### JSON Web Key Representation

When expressing a cryptographic key for these curves in JSON Web Key (JWK) form, the following rules apply:

- The parameter "kty" MUST be present and set to "OKP"
- The parameter "crv" MUST be present and value MUST be one defined in (#curve-parameter-registration).
- The parameter "x" MUST be present whose value represents the curve point for the public key. This value MUST be encoded using the serialization defined in [@!id.draft.pairing-friendly-curves-10] Appendix C and MUST be base64url encoded without padding as defined in [@!RFC7515] Appendix C.
- The parameter "d" MUST be present for private key representations whose value MUST contain the little endian representation of the private key encoded using base64url encoded without padding as defined in [@!RFC7515] Appendix C. This parameter MUST NOT be present for public keys.

### COSE_Key Representation

When expressing a cryptographic key for these curves in COSE_Key form, the following rules apply:

- The parameter "kty" (1) MUST be present and set to "OKP" (1)
- The parameter "crv" (-1) MUST be present and value MUST be one defined in (#curve-parameter-registration).
- The parameter "x" (-2) MUST be present whose value represents the curve point for the public key. This value MUST be encoded using the serialization defined in [@!id.draft.pairing-friendly-curves-10] Appendix C.
- The parameter "d" (-4) MUST be present for private key representations whose value MUST contain the little endian representation of the private key. This parameter MUST NOT be present for public keys.

### Curve Parameter Registration

JWK "crv" value | COSE_Key "crv" value | Description         |
----------------|----------------------|---------------------|
Bls12381G1      | TBD (13 requested)                   | A cryptographic key on the Barreto-Lynn-Scott (BLS) curve featuring an embedding degree 12 with 381-bit p in the subgroup of G1 defined as `E(GF(p))` of order r
Bls12381G2      | TBD (14 requested)                   | A cryptographic key on the Barreto-Lynn-Scott (BLS) curve featuring an embedding degree 12 with 381-bit p in the subgroup of G1 defined as `E(GF(p^2))` of order r
Bls48581G1      | TBD (15 requested)                   | A cryptographic key on the Barreto-Lynn-Scott (BLS) curve featuring an embedding degree 48 with 581-bit p in the subgroup of G1 defined as `E(GF(p))` of order r
Bls48581G2      | TBD (16 requested)                   | A cryptographic key on the Barreto-Lynn-Scott (BLS) curve featuring an embedding degree 48 with 581-bit p in the subgroup of G1 defined as `E(GF(p^8))` of order r

# Security Considerations

See [@!id.draft.pairing-friendly-curves-10] for additional details about security considerations of the curves used.  Implementers should also consider section 9 of [@!RFC7517] when implementing this work.

# IANA Considerations

## JSON Web Key (JWK) Elliptic Curve Registrations

This section registers the following value in the IANA "JSON Web Key Elliptic Curve" registry [@!IANA.JOSE.Curves].

Bls12381G1

- Curve Name: Bls12381G1
- Curve Description: 381 bit with an embedding degree of 12 Barreto-
Lynn-Scott pairing friendly curve using the r-order subgroup of
E(GF(p))
- JOSE Implementation Requirements: Optional
- Change Controller: IESG
- Specification Document(s): (#json-web-key-representation)

Bls12381G2

- Curve Name: Bls12381G2
- Curve Description: 381 bit with an embedding degree of 12 Barreto-
Lynn-Scott pairing friendly curve using the r-order subgroup of
E'(GF(p^2))
- JOSE Implementation Requirements: Optional
- Change Controller: IESG
- Specification Document(s): (#json-web-key-representation)

Bls48581G1

- Curve Name: Bls48581G1
- Curve Description: 581 bit with an embedding degree of 48 Barreto-
Lynn-Scott pairing friendly curve using the r-order subgroup of
E(GF(p))
- JOSE Implementation Requirements: Optional
- Change Controller: IESG
- Specification Document(s): (#json-web-key-representation)

Bls48581G2

- Curve Name: Bls48581G2
- Curve Description: 581 bit with an embedding degree of 48 Barreto-
Lynn-Scott pairing friendly curve using the r-order subgroup of
E'(GF(p^8))
- JOSE Implementation Requirements: Optional
- Change Controller: IESG
- Specification Document(s): (#json-web-key-representation)

## COSE Elliptic Curve Registrations

This section registers the following value in the IANA "COSE Elliptic Curves" registry [@!IANA.COSE.Curves].

Bls12381G1

- Curve Name: Bls12381G1
- Value: TBD (13 requested)
- Key Type: OKP
- Curve Description: 381 bit with an embedding degree of 12 Barreto-
Lynn-Scott pairing friendly curve using the r-order subgroup of
E(GF(p))
- JOSE Implementation Requirements: Optional
- Change Controller: IESG
- Specification Document(s): (#cose-key-representation)
- Recommended: Yes

Bls12381G2

- Curve Name: Bls12381G2
- Value: TBD (14 requested)
- Key Type: OKP
- Curve Description: 381 bit with an embedding degree of 12 Barreto-
Lynn-Scott pairing friendly curve using the r-order subgroup of
E'(GF(p^2))
- JOSE Implementation Requirements: Optional
- Change Controller: IESG
- Specification Document(s): (#cose-key-representation)
- Recommended: Yes

Bls48581G1

- Curve Name: Bls48581G1
- Value: TBD (15 requested)
- Key Type: OKP
- Curve Description: 581 bit with an embedding degree of 48 Barreto-
Lynn-Scott pairing friendly curve using the r-order subgroup of
E(GF(p))
- JOSE Implementation Requirements: Optional
- Change Controller: IESG
- Specification Document(s): (#cose-key-representation)
- Recommended: Yes

Bls48581G2

- Curve Name: Bls48581G2
- Value: TBD (16 requested)
- Key Type: OKP
- Curve Description: 581 bit with an embedding degree of 48 Barreto-
Lynn-Scott pairing friendly curve using the r-order subgroup of
E'(GF(p^8))
- JOSE Implementation Requirements: Optional
- Change Controller: IESG
- Specification Document(s): (#cose-key-representation)
- Recommended: Yes

{backmatter}

# Acknowledgments

The authors of this draft would like to acknowledge the pre-work of Kyle Den Hartog which was used as the foundations for this draft.

# Document History

-00

* Initial history


<reference anchor="IANA.COSE.Curves" target="https://www.iana.org/assignments/cose/cose.xhtml#elliptic-curves">
 <front>
   <title>COSE Elliptic Curves</title>
   <author><organization>IANA</organization></author>
 </front>
</reference>

<reference anchor="IANA.JOSE.Curves" target="https://www.iana.org/assignments/jose/jose.xhtml#web-key-elliptic-curve">
 <front>
   <title>JOSE Elliptic Curves</title>
   <author><organization>IANA</organization></author>
 </front>
</reference>

<reference anchor="id.draft.pairing-friendly-curves-10" target="https://www.ietf.org/archive/id/draft-irtf-cfrg-pairing-friendly-curves-10.html">
 <front>
   <title>Pairing-Friendly Curves</title>
   <author><organization>IETF CFRG</organization></author>
 </front>
</reference>

<reference anchor="id.draft.bls-signature-04" target="https://datatracker.ietf.org/doc/html/draft-irtf-cfrg-bls-signature-04">
 <front>
   <title>BLS Signature</title>
   <author><organization>IETF CFRG</organization></author>
 </front>
</reference>

<reference anchor="TPM" target="https://trustedcomputinggroup.org/">
 <front>
   <title>Trusted Platform Module</title>
   <author><organization>Trusted Computing Group</organization></author>
 </front>
</reference>


