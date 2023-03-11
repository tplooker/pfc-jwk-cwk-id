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

There are a variety of applications for pairing based cryptography including schemes already published as RFCs, such as Identity-Based Cryptography [@RFC5091] Sakai-Kasahara Key Encryption (SAKKE) [@RFC6508], and Identity-Based Authenticated Key Exchange (IBAKE) [@RFC6539]. SAKKE is applied to Multimedia Internet KEYing (MIKEY) [@RFC6509].

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
Bls12381G2      | TBD (14 requested)                   | A cryptographic key on the Barreto-Lynn-Scott (BLS) curve featuring an embedding degree 12 with 381-bit p in the subgroup of G1 defined as `E(GF(p^2))` of order r
Bls48581G1      | TBD (15 requested)                   | A cryptographic key on the Barreto-Lynn-Scott (BLS) curve featuring an embedding degree 48 with 581-bit p in the subgroup of G1 defined as `E(GF(p))` of order r
Bls48581G2      | TBD (16 requested)                   | A cryptographic key on the Barreto-Lynn-Scott (BLS) curve featuring an embedding degree 48 with 581-bit p in the subgroup of G1 defined as `E(GF(p^8))` of order r

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

# Appendix

## JSON Web Key Examples

### Bls12381 Key Pairs

The following examples showcase JWKs for both the G1 and G2 subgroups of the Bls12381 curve. Note, the examples also include the corresponding private key, expressed through the inclusion of the “d” parameter.

An example JWK for the Bls12381 curve where the public key is in the G1 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls12381G1",
  "d": "Mt_OyD9IAsYvobHJ9NCipm6-G7zAu28FCc-saRnXhjQ",
  "x": "iXmOmxttBniHSpyoq-vBr82BexrqG7WDTsxCY4ngUOERVxwpwUT7yKqSKqJeIr7J"
}
```

An example JWK for the Bls12381 curve where the public key is in the G2 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls12381G2",
  "d": "CUrC9Xp5pEonbFaykalWlbNYYwueJlcuoOexhEhpu0k",
  "x": "rvdKcdkxwlj0Y-XZsFpz1hDPJGjnLN27IJipbmaLlaKdYfICGG6dzakG6EkdcvW0AtVV6hXBSKtdFnKQKmmD759tMYYuvKYf5o2cZnROLN5iWQ2H6vp6FlLi71a_AE5I"
}
```

### Bls48581 Key Pairs

The following examples showcase JWKs for both the G1 and G2 subgroups of the Bls48581 curve. As before, note that the examples also include the corresponding private key, expressed through the inclusion of the “d” parameter.

An example JWK for the Bls48581 curve where the public key is in the G1 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls48581G1",
  "x": "AwwXxiUemAF5ZYORdFabm87Y-8mzSW8jB1HZ-0BvXi87VHlTJbUs7yFEZlYNxcaP2uUnfgBhR7GUgKK_uiW_yQ2v5EeNuitTKcE",
  "d": "IbTrWWx6KkM1ggh-53OMB8Q8Bv3jXRJ3mGf0-v4YcrrGCf6joOTKRUujsccKUqHckiITeqWmGmdGgKJ3luPmFTh0i95oV2Cktw"
}
```

An example JWK for the Bls48581 curve where the public key is in the G2 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls48581G2",
  "x": "AgtldXunqaVhEKx37rZJz81ZBTdR9l4Qi3B9rFPwCiRulIBbkAYNDilhGn5dSc_CP3vU6CfftbpMbt-Of1Wbclce92hxX3f-Ay4KMbzeAP4eC0hAgnd3vz1M_zsSNIYy8G8I2CH8eXeCswshKe6uR6DZtEmhSfv4Qnm7my0mOqAa9EJN5CkYc3w02H2Gul-C8kf1AX9H-626ukmxxJbkPaxszYDPFMFYLdP7O73lP3amhy8cZkVsVRQw4j-VDq484jNZoNIOlleBJlSuCQHl8-XpPTsFA6PP4H55nwTHpoGv-149SuhUI_AWcqdymTZOBt4Orp1urXKxp4k3X7lkknwQciMd6693h6Io_zP0duYZq94cKl9USGzo2vVQb_AL_qxJOJ8G-DisKL0cwrJY2AE69K2rXttOvxOxwbHUDdzoFsL8xGivaPba5ESnAnz1PGYn_cfTHq_slSWjfmf26IjcOx_fvapQjjECl9RVBZIEYAnqaly4hcP4uUydfo0qh_2wuTksYtvf_YxnaEFsk_OFakErKFGAAVyDXVTrrftBWnDz4h80tIc0ojXeuoorYqND723WGglvEbolDSBzd_n4EPArc77_Xmf_alfEhOyNIOZc1UPHSU3s0oQy726hwPvhZ4vde3bHmhCYoM6faUK8enhlIGHGQHgCwuEm0wgSConl_GXzoqEfQ2RY40qbhxKEsieofufzRq6Y-t-XEMnoL0p_F69PRshrj4I82Nki8MJki67D96nFyts-xrOYdojTceBFY53a",
  "d": "y77j03LMX1yONnhZb8FqMmiMH37nPoaeGSOkznov9OWFnT6HTfXvnmN38GgimttuZwMcVrHOmYFfNx5aHGTsNXB0n0IaYtxseg"
}
```

## COSE_Key Examples

### Bls12381 Key Pairs

The following showcase COSE_key examples for both the G1 and G2 subgroups of the Bls12381 curve. Note, the examples also include the corresponding private key, expressed through the inclusion of the “d” (-4) parameter.

An example COSE_Key for the Bls12381 curve where the public key is in the G1 subgroup expressed as an octet string.

```
a40101200d215830a256ee7ce44ef954d8b57903da4ee15dce3928938da42b9dace01e2
29387dd6aaedad27d30d50d713cdedc4aa4af9769235820fe79982dcb6de040378edccc
fc3a0eef6cce809b64ffba43a97a0509fa62cc51
```

The above octet string decoded into CBOR diagnostic view.

```
{
  1: 1,
  -1: 13,
  -2: h'A256EE7CE44EF954D8B57903DA4EE15DCE3928938DA42B9DACE01E229387DD6AAEDAD27D30D50D713CDEDC4AA4AF9769',
  -4: h'FE79982DCB6DE040378EDCCCFC3A0EEF6CCE809B64FFBA43A97A0509FA62CC51'
}
```

An example COSE_Key for the Bls12381 curve where the public key is in the G2 subgroup expressed as an octet string.

```
a40101200e2158608936714d127f9089fa45467e337cba9f88290ca50c9d390af3db57a
08c4e838640827ef0b5a363779b2088188ed8f9be077c5974d63586ba072ca6b9920c32
12b9e76cec18209d91be1088fbae9211f8ceb859154770ed12075db97f963f69fe23582
07077a2792b2ce6a16ccbb3e6840a8f04d96afb7f2b028a2858e09387fa1a5070
```

The above octet string decoded into CBOR diagnostic view.

```
{
  1: 1,
  -1: 14,
  -2: h'8936714D127F9089FA45467E337CBA9F88290CA50C9D390AF3DB57A08C4E838640827EF0B5A363779B2088188ED8F9BE077C5974D63586BA072CA6B9920C3212B9E76CEC18209D91BE1088FBAE9211F8CEB859154770ED12075DB97F963F69FE',
  -4: h'7077A2792B2CE6A16CCBB3E6840A8F04D96AFB7F2B028A2858E09387FA1A5070'
}
```

### Bls48581 Key Pairs

The following showcase COSE_key examples for both the G1 and G2 subgroups of the Bls48581 curve. Note, the examples also include the corresponding private key, expressed through the inclusion of the “d” (-4) parameter.

An example COSE_Key for the Bls48581 curve where the public key is in the G1 subgroup expressed as an octet string.

```
a40101200f21584a0307dbfbad0afdd27c2d3c96bf385702d8a6261d95315d6c5b96e536
eee397794212d7cb1bb64f55019bbb63bcd3a609926f0449f55c276db5135b5887869b39
a1f6de461dbd908e98572358491ceac34d99d644e1d50974322a3a8c5c514879be5d53a8
80435e76d471f841e1afb143148f24dc095fe03be16d00cbb366998329f41ae131a100af
c80a05aa4373dddf52db645392bb
```

The above octet string decoded into CBOR diagnostic view.

```
{
  1: 1, 
  -1: 15, 
  -2: h'0307DBFBAD0AFDD27C2D3C96BF385702D8A6261D95315D6C5B96E536EEE397794212D7CB1BB64F55019BBB63BCD3A609926F0449F55C276DB5135B5887869B39A1F6DE461DBD908E9857', 
  -4: h'1CEAC34D99D644E1D50974322A3A8C5C514879BE5D53A880435E76D471F841E1AFB143148F24DC095FE03BE16D00CBB366998329F41AE131A100AFC80A05AA4373DDDF52DB645392BB'
}
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

The above octet string decoded into CBOR diagnostic view.

```
{
  1: 1, 
  -1: 16, 
  -2: h'020F5FFE8621C4258F5DC04695F939E52ABA0267E0E7ECC703999F3CF9226236FEE01CC8DBEB6A70523BE06555E2725493F86B1BF55F2706657F4988E9989223EC30030DFBABE567C6790E61821CAE0DDEE70A6FC24FC1001658DDE28C6DD1CDFEEFDB0F1C3D9409FF84FCC45959398985ECDD3A42081EBACB56BD76612F96711FF7B16257340A3BE467BA2F4DC67A10453BF40047217D5606D2A145F16554A2C17B23A33C9662F24734F08CF71D2354D81E986B8739B69C32DE0A61D90F04579E25C6B2B46E69FC25B8111A0EEF741A589DD7AF0B141A6AF3ECDDFB0C129D7BF3DE0591D4348855B8132A6F03838EE7B658659F2E937EE7E084FCC3F676AAF67EC7F076E28EC8A56504490E73608953F3E45D2218B40029ED030769BDDA26B2B44CDDEC030D1A60AE9AAC91DFE6AC9DF3AA77659BD41943C62CF03BE64FCCB3372173CD3CEC444246EFF854B0A65B5CBF522D6942497D4ABF39D63DDF8171092D99EE01E8799E3AC4CC5FC8B5B40F50ECA9788B196AD253BEB068F855BDD86F7160710752761FA3FEDC56927F8C66C98A232CC005E72C270959CDEBCF73434766B34809B981291357632A98DD36065FE7E5BDB7C61AEB0951FDFF21EB18B7193C29D7C3D394F28FEDB908467B54357BDDA3C99407925A090CD85AED384DDF65CA01ACB13D52E9BA3CB959903EA573D66495D64E1C7DD5561DA98995FA1C3B5E05F54A93A76A26A973921CFCFC3216851C08251EE2EB8220B1B2F6C83B2C5C97D66875735392B1D2B89657093423FAD3E80B0F5274ABB92D3276D8F640329FBD8042CDDEE3767AADF3', 
  -4: h'C745C8B77BE92FB18BD324DEF750888DD7542742928EB810FFA5E627CA5E494F08D9BDC5982B8C648F99E3664AC9D014F0A9025AED0E228DAFBC29A602E7D488DFE8CD39618D162993'
}
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
