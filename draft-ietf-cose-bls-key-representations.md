%%%
title = "Barreto-Lynn-Scott Elliptic Curve Key Representations for JOSE and COSE"
ipr= "trust200902"
area = "Internet"
workgroup = "COSE"
submissiontype = "IETF"
keyword = ["COSE", "JOSE"]

[seriesInfo]
name = "Internet-Draft"
value = "draft-ietf-cose-bls-key-representations-latest"
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
  email = "mbj@microsoft.com"
  uri = "https://self-issued.info/"

%%%

.# Abstract

This specification defines how to represent cryptographic keys for the pairing-friendly elliptic curves known as Barreto-Lynn-Scott (BLS), for use with the key representation formats of JSON Web Key (JWK) and COSE (COSE_Key).

{mainmatter}

# Introduction

This specification defines how to represent cryptographic keys for the pairing-friendly elliptic curves known as Barreto-Lynn-Scott [@!BLS], for use with the key representation formats of JSON Web Key (JWK) and COSE_Key. This specification registers the elliptic curves in appropriate IANA JOSE and COSE registries.

There are a variety of applications for pairing based cryptography including schemes already published as RFCs, such as Identity-Based Cryptography [@RFC5091] Sakai-Kasahara Key Encryption (SAKKE) [@RFC6508], and Identity-Based Authenticated Key Exchange (IBAKE) [@RFC6539]. SAKKE is applied to Multimedia Internet KEYing (MIKEY) via [@RFC6509] and IBAKE is applied for a similar application via [@RFC6267].

This branch of cryptography has also been used to develop privacy-preserving cryptographic hardware attestations schemes, including the Elliptic Curve Direct Anonymous Attestation (ECDAA) in the Trusted Platform Modules [@TPM] specified by the Trusted Computing Group. Further work on similar schemes has also occurred at the FIDO Alliance [@ECDAA]. Similarly, Intel released [@EPID] which provides a solution to remote hardware attestation for Intel Software Guard Extension (SGX) enabled environments.

More recently, applications of pairing based cryptography using the Barreto-Lynn-Scott curves include the standardization effort for BLS Signatures [@!id.draft.bls-signature-04], which are used extensively in multiple blockchain projects due to their unique signature aggregation properties, including [Ethereum] [DFINITY] [Algorand]. Additionally, efforts are under way to standardize the general purpose short group signature scheme of BBS Signatures [@BBS], which features novel properties such as multi-message signing and selective disclosure alongside zero knowledge proving. It is intended that this draft will help with these efforts by standardizing the associated cryptographic key representation in the popular formats of JWK and COSE_Key.

Other relevant work to this draft includes [@JWP] which is extending the JOSE family of specifications to provide support for representing a variety of new proof based cryptographic schemes such as [@BBS] which as referred to above uses the Barreto-Lynn-Scott curves.

There are multiple different pairing-friendly curves in active use; however, this draft focuses on a definition for the Barreto-Lynn-Scott curves due to them being the most "widely used" and "efficient" whilst achieving 128-bit and 256-bit security (BLS12-381 and BLS48-581 respectively).

More extensive discussion on the broader application of pairing based cryptography and the assessment of various elliptic curves (including the BLS family) can be found in [@!id.draft.pairing-friendly-curves-10].

# Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14 [@!RFC2119] [@RFC8174] when, and only when, they appear in all capitals, as shown here.

## Representation Definition

The following definitions apply to the pairing-friendly elliptic curves known as the Barreto-Lynn-Scott (BLS) curves.

### JSON Web Key Representation

When expressing a cryptographic key for these curves in JSON Web Key (JWK) form, the following rules apply:

- The parameter "kty" MUST be present and set to "OKP".
- The parameter "crv" MUST be present and value MUST be one defined in (#curve-parameter-registration).
- The parameter "x" MUST be present whose value represents the curve point for the public key. This value MUST be encoded using the serialization defined in [@!id.draft.pairing-friendly-curves-10] Appendix C and MUST be base64url encoded without padding as defined in [@!RFC7515] Appendix C.
- The parameter "d" MUST be present for private key representations whose value MUST contain the little-endian representation of the private key base64url encoded without padding as defined in [@!RFC7515] Appendix C. This parameter MUST NOT be present for public keys.

### COSE_Key Representation

When expressing a cryptographic key for these curves in COSE_Key form, the following rules apply:

- The parameter "kty" (1) MUST be present and set to "OKP" (1).
- The parameter "crv" (-1) MUST be present and value MUST be one defined in (#curve-parameter-registration).
- The parameter "x" (-2) MUST be present whose value represents the curve point for the public key. This value MUST be encoded using the serialization defined in [@!id.draft.pairing-friendly-curves-10] Appendix C.
- The parameter "d" (-4) MUST be present for private key representations whose value MUST contain the little-endian representation of the private key. This parameter MUST NOT be present for public keys.

### Curve Parameter Registration

JWK "crv" value | COSE_Key "crv" value | Description         |
----------------|----------------------|---------------------|
Bls12381G1      | TBD (13 requested)                   | A cryptographic key on the Barreto-Lynn-Scott (BLS) curve featuring an embedding degree 12 with 381-bit p in the subgroup of G1 defined as `E(GF(p))` of order r
Bls12381G2      | TBD (14 requested)                   | A cryptographic key on the Barreto-Lynn-Scott (BLS) curve featuring an embedding degree 12 with 381-bit p in the subgroup of G2 defined as `E(GF(p^2))` of order r
Bls48581G1      | TBD (15 requested)                   | A cryptographic key on the Barreto-Lynn-Scott (BLS) curve featuring an embedding degree 48 with 581-bit p in the subgroup of G1 defined as `E(GF(p))` of order r
Bls48581G2      | TBD (16 requested)                   | A cryptographic key on the Barreto-Lynn-Scott (BLS) curve featuring an embedding degree 48 with 581-bit p in the subgroup of G2 defined as `E(GF(p^8))` of order r

# Security Considerations

See [@!id.draft.pairing-friendly-curves-10] for additional details on security considerations for the curves used.  Implementers should also consider the general guidance provided in Section 9 of [@!RFC7517] and Section 17 of [@!RFC8152] when using this specification.

Furthermore, because this specification only defines the cryptographic key representations and not the usage of these keys with specific algorithms, implementers should be aware to follow any guidance that may be provided around appropriate usage of the keys and or additional steps that may be required to validate the keys within the context of particular algorithms.

# IANA Considerations

## JSON Web Key (JWK) Elliptic Curve Registrations

This section registers the following values in the IANA "JSON Web Key Elliptic Curve" registry [@!IANA.JOSE.Curves].

Bls12381G1

- Curve Name: Bls12381G1
- Curve Description: 381 bit with an embedding degree of 12 Barreto-
Lynn-Scott pairing-friendly curve using the r-order subgroup of
E(GF(p))
- JOSE Implementation Requirements: Optional
- Change Controller: IESG
- Specification Document(s): (#json-web-key-representation)

Bls12381G2

- Curve Name: Bls12381G2
- Curve Description: 381 bit with an embedding degree of 12 Barreto-
Lynn-Scott pairing-friendly curve using the r-order subgroup of
E'(GF(p^2))
- JOSE Implementation Requirements: Optional
- Change Controller: IESG
- Specification Document(s): (#json-web-key-representation)

Bls48581G1

- Curve Name: Bls48581G1
- Curve Description: 581 bit with an embedding degree of 48 Barreto-
Lynn-Scott pairing-friendly curve using the r-order subgroup of
E(GF(p))
- JOSE Implementation Requirements: Optional
- Change Controller: IESG
- Specification Document(s): (#json-web-key-representation)

Bls48581G2

- Curve Name: Bls48581G2
- Curve Description: 581 bit with an embedding degree of 48 Barreto-
Lynn-Scott pairing-friendly curve using the r-order subgroup of
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
Lynn-Scott pairing-friendly curve using the r-order subgroup of
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
Lynn-Scott pairing-friendly curve using the r-order subgroup of
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
Lynn-Scott pairing-friendly curve using the r-order subgroup of
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
Lynn-Scott pairing-friendly curve using the r-order subgroup of
E'(GF(p^8))
- JOSE Implementation Requirements: Optional
- Change Controller: IESG
- Specification Document(s): (#cose-key-representation)
- Recommended: Yes

{backmatter}

# JSON Web Key Examples

## Bls12381 Key Pairs

The following examples showcase JWKs for both the G1 and G2 subgroups of the Bls12381 curve. Note, the examples also include the corresponding private key, expressed through the inclusion of the “d” parameter.

An example JWK for the Bls12381 curve where the public key is in the G1 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls12381G1",
  "x": "AenUwnEOYNZVj3r-Se74mYAHOKYjZpGRyY4AfHPTVErq9oLORZUO8TyusmI2urBB",
  "y": "B7E90HoyxdrEeOwbmMbZTKKnX-9y_C-ysf5UY9sA_GfotS-feEsG9YAIIQE_8T2X",
  "d": "afUKrrA56cJoCJ8aW4Xzxzawcup5wsfqM9oFYj40gOI"
}
```

Another example of a different JWK for the Bls12381 curve where the public key is in the G1 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls12381G1",
  "x": "CycImbdwxMGYpsqTliCvQ9gj3lruElTDSYbzh2g4VHhlJENpi7sUM-b-9IW_e5oY",
  "y": "GGbi2FLZzp1tAvnivmlxFHJ3GNplhVwzt6xigKMsEhjRMyQm3fjz5hdKSBhGctq-",
  "d": "UNr7ub9UVbSbLqWfIz0iA7SmTB9McY1MJ7nPElbZ9yA"
}
```

## Bls48581 Key Pairs

The following examples showcase JWKs for both the G1 and G2 subgroups of the Bls48581 curve. As before, note that the examples also include the corresponding private key, expressed through the inclusion of the “d” parameter.

An example JWK for the Bls48581 curve where the public key is in the G1 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls48581G1",
  "x": "AkmYVmiSAJkM1Mnwn4kFtwRQSHWXV71Sks4ARgJ8OjKxKFlKIdg738P5txciizAi7RyoZRbQdshCac4WAVcopGttFDZQ882qrQ",
  "y": "DDhnx0yoSIddnUNYyQqAoS_tZXguc4t8UjhFCBD3AR-wyMkIRmTVIs-_WTyQ3Lkul0nuLoMGbVYimIlJLKAlQBQqURD4bpW7eA",
  "d": "s76mQwCbnW-4rGoVU-39-jacXHD4zw1Wlfi9y-Lq0xHnYQC6eSt3lN9wYfrkZRw6ZA0wTRMxSilerAlqUG-Sk0Y9HyoLFjvSHg"
}
```

Another example of a different JWK for the Bls48581 curve where the public key is in the G1 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls48581G1",
  "x": "B5L-3_dDeqCpVj_dXjsCcD48VzWFfOpciuzvUgD4IfgwlntI1OPp1_xbXMkLWdecodEzI2SoTMNU8C_LkELDWJjlnUZBUBAttA",
  "y": "CJpH-wqGNPsjWw5yLPLlDQbkh7GToHtA0afsub9QTEvV_5xXV1vtBkzkVJ-rwUaGQwZTaX9UgO04Klxz8goFxPKKeJLmXPFPJA",
  "d": "VoDqj2s37PzHSnhPlJUypadLHCUsb64hLBdNWUXqhqBLmq7BknK5RlCD67nC8weAla5Ixh07Ar_u5amyVDQX-d1m2x3G7V-AYA"
}
```

# COSE_Key Examples

## Bls12381 Key Pairs

The following showcase COSE_key examples for both the G1 and G2 subgroups of the Bls12381 curve. Note, the examples also include the corresponding private key, expressed through the inclusion of the “d” (-4) parameter.

An example COSE_Key for the Bls12381 curve where the public key is in the G1 subgroup expressed as an octet string.

```
a40101200d215830a07befc5cba7962fe2985b51fcff3937037c500edbcdb54da9906b30
660f1ea88b97939ab03291e85a341fa1072657a62358203404e6ceb0f09a90130daa7b1a
9900859ddd643406ff77f33709400ccffcc90c
```

Another example of a different COSE_Key for the Bls12381 curve where the public key is in the G1 subgroup expressed as an octet string.

```
a40101200d215830b488a826b226038b44e4f056516c3d7cfa5aa1d36942c16945dba267
d217df71028732b4d326360695fe4bb86eeed6a62358207396ec560f4126a8afc98ef5d9
08171a5f9dec7091e8e65fe582cb0486e4f405
```

An example COSE_Key for the Bls12381 curve where the public key is in the G2 subgroup expressed as an octet string.

```
a40101200e215860b982b77cf2768da52b6c22b2f5063bc98ceadd75bd52a2055fa97417
c6af39e7205dd52b74ab1330126678a6b02a893d05019d2e163e7db21ef0e55745d844a9
a708c4b3a974190d95d6b549e63d94edc7b5b13bcc32589a9f60761d1cd20211235820b3
a0df789c792c7c59dd4a8cebd637fddee456c62cb4f0ad34d0d97cebc47e36
```

Another example of a different COSE_Key for the Bls12381 curve where the public key is in the G2 subgroup expressed as an octet string.

```
a40101200e215860ae56a48f7d757cc25b58d2300249475d6dab5e8433de25b361fa2182
586c33816affff978c11d3c3138cd0166da822c00d2ae4e424a5646547e89d8f9526fdcb
190efe78ef36325980911025d20beef88fb576540e50df0051e8eb363dc3656a235820bb
636cf2a270c3347646d736ad3a7fa6329e60083d15d0bd7473e2b262035b6e
```

## Bls48581 Key Pairs

The following showcase COSE_key examples for both the G1 and G2 subgroups of the Bls48581 curve. Note, the examples also include the corresponding private key, expressed through the inclusion of the “d” (-4) parameter.

An example COSE_Key for the Bls48581 curve where the public key is in the G1 subgroup expressed as an octet string.

```
a40101200f21584a0307dbfbad0afdd27c2d3c96bf385702d8a6261d95315d6c5b96e536
eee397794212d7cb1bb64f55019bbb63bcd3a609926f0449f55c276db5135b5887869b39
a1f6de461dbd908e98572358491ceac34d99d644e1d50974322a3a8c5c514879be5d53a8
80435e76d471f841e1afb143148f24dc095fe03be16d00cbb366998329f41ae131a100af
c80a05aa4373dddf52db645392bb
```

Another example of a different COSE_Key for the Bls48581 curve where the public key is in the G1 subgroup expressed as an octet string.

```
a40101200f21584a0305e6466cb859b690d744a72926f8128ebd3f5f638378ca757ad4c1
6265e5a280809ad58ba3e5cf45fbb439cfb1fa6cfdd25a477a886a5fc69705bb9c242a05
1476dcc1277fb70b1b862358499b09e852de116da86eb282b015973c1daa590a0e7ac238
c3327f28162234e3903fb7270abeb62994a01cd1e268e7e61e890eaad4fb16764e9469e4
62c8b0b7161cce4e52f979095795
```

An example COSE_Key for the Bls48581 curve where the public key is in the G2 subgroup expressed as an octet string.

```
a40101201021590249020f5ffe8621c4258f5dc04695f939e52aba0267e0e7ecc703999f
3cf9226236fee01cc8dbeb6a70523be06555e2725493f86b1bf55f2706657f4988e99892
23ec30030dfbabe567c6790e61821cae0ddee70a6fc24fc1001658dde28c6dd1cdfeefdb
0f1c3d9409ff84fcc45959398985ecdd3a42081ebacb56bd76612f96711ff7b16257340a
3be467ba2f4dc67a10453bf40047217d5606d2a145f16554a2c17b23a33c9662f24734f0
8cf71d2354d81e986b8739b69c32de0a61d90f04579e25c6b2b46e69fc25b8111a0eef74
1a589dd7af0b141a6af3ecddfb0c129d7bf3de0591d4348855b8132a6f03838ee7b65865
9f2e937ee7e084fcc3f676aaf67ec7f076e28ec8a56504490e73608953f3e45d2218b400
29ed030769bdda26b2b44cddec030d1a60ae9aac91dfe6ac9df3aa77659bd41943c62cf0
3be64fccb3372173cd3cec444246eff854b0a65b5cbf522d6942497d4abf39d63ddf8171
092d99ee01e8799e3ac4cc5fc8b5b40f50eca9788b196ad253beb068f855bdd86f716071
0752761fa3fedc56927f8c66c98a232cc005e72c270959cdebcf73434766b34809b98129
1357632a98dd36065fe7e5bdb7c61aeb0951fdff21eb18b7193c29d7c3d394f28fedb908
467b54357bdda3c99407925a090cd85aed384ddf65ca01acb13d52e9ba3cb959903ea573
d66495d64e1c7dd5561da98995fa1c3b5e05f54a93a76a26a973921cfcfc3216851c0825
1ee2eb8220b1b2f6c83b2c5c97d66875735392b1d2b89657093423fad3e80b0f5274abb9
2d3276d8f640329fbd8042cddee3767aadf3235849c745c8b77be92fb18bd324def75088
8dd7542742928eb810ffa5e627ca5e494f08d9bdc5982b8c648f99e3664ac9d014f0a902
5aed0e228dafbc29a602e7d488dfe8cd39618d162993
```

Another example of a different COSE_Key for the Bls48581 curve where the public key is in the G2 subgroup expressed as an octet string.


```
a40101201021590249020b01bf0f906b09087216e872667c16b55890a0a164511c661dda
14b9b7e0630659382ce4eec714d7ffc590c5da64d975169610aa4192705236c48baf583a
79ff9fc7d382067fef44ab095659802fec96e15fa18456513e3e748133bb353304079b76
3e4d97ed73e96773f2eaf473fd60cce617893a0258f7a096183b14173e706fc0fe31be5b
78c1cf61bd4c18e31e892c2010df8e5ad31777bca8115b33aecb83248a1363cc18748979
837a8dd3d6c507c5ddd7c8536309a1b0578b3f52e73d5d5af9de2bb540332fc569562d7b
347aa90cfc962ded4dce65ee6a00069e7a38e595ac19020a7d37145bb357f319b4acb2a4
1075eb3fc987b949345dcb3b96018b6f2c28ad2478322d110ae37f571e3144ee8b7a8cf3
09167eae3339b9d02f5e1f3000080be6d9ae84b151390a90672147d0fac420a22c2e2d87
bffe8fb634b5e722b6f701ec3dca9c188d301e435298ca7b044e146819b29a892cdfa763
992eceb2be131cba7331835ef0c7640fa270a2b7a0bf5b23263cab6eccbf57bdeeda0897
09625a2b2186a3e718b4948d929f474b00dced18581358177b25b276251af98e9abcc4c0
da8be77a26a6508b8d9ba6eb5792ea7d0eac5ed37893d6853a041ce458841298819022d6
0a57af4caafedaac3d6ea2e6d177b8b28592ce67392b12b6587c6ed3f1568951810bf3df
da2feef307df29deab2868dc8fdcf1bee40f9cbbace96420de99e73b118a8459826b1691
eea8b48acf98439483c3c51ce3fe2828af572d2fa154d383567f6aa45df7d97ba6834fe7
43a5ce1eebf094117df5d5e72ce3d40301932358499b17059b32802b7fccbeae25c24577
87e48fa0f5a7c4688fd02f5871f075c3305ba7c9965e8a37a75eb57c7a31ddca2e7e93aa
16e5abd9afe6b9c04dbc48045652d368b07a2e402244
```

# Acknowledgments

The authors would like to acknowledge the work of Kyle Den Hartog, which was used as the foundation for this draft.

# Document History

-00

* Created draft-ietf-cose-bls-key-representations-00 from draft-looker-cose-bls-key-representations-00 following working group adoption.

-01

* Added JWK examples.

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

<reference anchor="BLS">
  <front>
    <title>Constructing Elliptic Curves with Prescribed Embedding Degrees</title>
    <seriesInfo name="DOI" value="10.1007/3-540-36413-7_19"/>
    <seriesInfo name="Security in Communication Networks" value="pp. 257-267"/>
    <author initials="P." surname="Barreto" fullname="Paulo S. L. M. Barreto">
      <organization/>
    </author>
    <author initials="B." surname="Lynn" fullname="Ben Lynn">
      <organization/>
    </author>
    <author initials="M." surname="Scott" fullname="Michael Scott">
      <organization/>
    </author>
    <date year="2003"/>
  </front>
</reference>

<reference anchor="ECDAA" target="https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-ecdaa-algorithm-v2.0-id-20180227.html">
  <front>
    <title>ECDAA Algorithm</title>
   <author><organization>FIDO Alliance</organization></author>
    <date year="2018"/>
  </front>
</reference>

<reference anchor="EPID" target="https://software.intel.com/en-us/download/intel-sgx-intel-epid-provisioning-and-attestation-services">
  <front>
    <title>Intel (R) SGX: Intel (R) EPID Provisioning and Attestation Services</title>
   <author><organization>Intel Corporation</organization></author>
  </front>
</reference>

<reference anchor="BBS" target="https://identity.foundation/bbs-signature/draft-bbs-signatures.html">
  <front>
    <title>The BBS Signature Scheme</title>
   <author><organization>Decentralized Identity Foundation</organization></author>
  </front>
</reference>

<reference anchor="DFINITY" target="https://dfinity.org/pdf-viewer/library/dfinity-consensus.pdf">
  <front>
    <title>DFINITY Consensus Algorithm</title>
   <author><organization>DFINITY</organization></author>
  </front>
</reference>

<reference anchor="ETHEREUM" target="https://ethresear.ch/t/pragmatic-signature-aggregation-with-bls/2105">
  <front>
    <title>Ethereum Signature Aggregation</title>
   <author><organization>Ethereum Research</organization></author>
  </front>
</reference>


<reference anchor="ALGORAND" target="https://medium.com/algorand/digital-signatures-for-blockchains-5820e15fbe95">
  <front>
    <title>Efficient and Secure Digital Signatures for Proof-of-Stake Blockchains</title>
   <author><organization>Algorand</organization></author>
  </front>
</reference>


<reference anchor="JWP" target="https://json-web-proofs.github.io/json-web-proofs/draft-jmiller-json-proof-algorithms.html#name-bls-curve">
  <front>
    <title>JSON Web Proof</title>
    <author initials='J.' surname='Miller' fullname='Jeremie Miller'/>
    <author initials='M.' surname='Jones' fullname='Michael B. Jones'/>
  </front>
</reference>
