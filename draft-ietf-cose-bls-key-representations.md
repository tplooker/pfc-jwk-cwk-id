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

## Point Coordinates Encoding

A point representing a public key will either be in the G1 or G2 subgroup of a curve. Depending on which one of the subgroups the public key will belong to, different serialization procedures need to be used, to encode its coordinates. Most specifically, if the public key is a point in the G1 subgroup, each of its coordinates MUST be encoded using the serialization defined in Section 2.3.5 of [@SEC1]. If the public key is a point in the G2 subgroup, each of its coordinates MUST be serialize using the procedure described in section 2.5 in [@id.draft.pairing-friendly-curves-10].

## Representation Definition

The following definitions apply to the pairing-friendly elliptic curves known as the Barreto-Lynn-Scott (BLS) curves.

### JSON Web Key Representation

When expressing a cryptographic key for these curves in JSON Web Key (JWK) form, the following rules apply:

- The parameter "kty" MUST be present and set to "OKP".
- The parameter "crv" MUST be present and value MUST be one defined in (#curve-parameter-registration).
- The parameter "x" MUST be present whose value represents the x coordinate of the curve point for the public key. This value MUST be encoded using the procedure defined in [Section 2.1](#point-coordinates-encoding) and MUST be base64url encoded without padding as defined in [@!RFC7515] Appendix C.
- The parameter "y" MUST be present whose value represents the y coordinate of the curve point for the public key. This value MUST be encoded using the procedure defined in [Section 2.1](#point-coordinates-encoding) and MUST be base64url encoded without padding as defined in [@!RFC7515] Appendix C.
- The parameter "d" MUST be present for private key representations whose value MUST contain the little-endian representation of the private key base64url encoded without padding as defined in [@!RFC7515] Appendix C. This parameter MUST NOT be present for public keys.

### COSE_Key Representation

When expressing a cryptographic key for these curves in COSE_Key form, the following rules apply:

- The parameter "kty" (1) MUST be present and set to "OKP" (1).
- The parameter "crv" (-1) MUST be present and value MUST be one defined in (#curve-parameter-registration).
- The parameter "x" (-2) MUST be present whose value represents the x coordinate of the curve point for the public key. This value MUST be encoded using the procedure defined in [Section 2.1](#point-coordinates-encoding) and MUST be base64url encoded without padding as defined in [@!RFC7515] Appendix C.
- The parameter "y" (-3) Must be present whose value represents the y coordinate of the curve point for the public key. This value MUST be encoded using the procedure defined in [Section 2.1](#point-coordinates-encoding) and MUST be base64url encoded without padding as defined in [@!RFC7515] Appendix C.
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
  "x": "Ed4GBGLVasEp4ejPz44CvllbTldfLLcm2QcIJluBL6p_SQmRrZvJNa3YaJ-Wx8Im",
  "y": "AbdYAsAb20CHzlVW6VBO9i16BcGOmcYiMLlBEh9DfAiDu_1ZIAd1zewSi9f6517g",
  "d": "3nc6_s38FVVlawbwmPFOjB4TlAPy_K2Tx39I7XnEnDc"
}
```

Another example of a different JWK for the Bls12381 curve where the public key is in the G1 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls12381G1",
  "x": "EUGQExpzxebwmXEeqc39OI3J1NfUCMVQPqc_Lb-4dLu0xCaSrd0rDBMTFthd5r-2",
  "y": "FNpXfp4a-5N7Cb528pwRhLINOmwyuhjKjPz8jcaxgCSNZCbjaYQsGoOhftRqWKpg",
  "d": "OdNBrRDu89IrnlY5Hl-2CW-3cqW0Rw7IgQUuwLZ8gV4"
}
```

An example JWK for the Bls12381 curve where the public key is in the G2 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls12381G2",
  "x": "Ea2sH4PBZLaYCn8se-1clmCORDsKxbbw3Js_Alu4OmkV9gmbJsy1YF2rt7Vxzs6SAjs8lstTgoTgXMF6QXdyh3m8k2ixxURGYLMaYylVK_x0F8HhE8zk0YWiGV3CHwpQ",
  "y": "BV_vvCQ632ScAooEExXuz1IeQH9D2o-uY_dAjZ37YHuRMEyzh8Tq-90JHQvicOqxBVkkrVEib-P_FMPHNtqxJymP3pV-H8fCdvPkoWInpFfM9tViyqD8JAmwDf64zU2h",
  "d": "hR6HfxlTwcjMGST5wYnkGiJvuVnpUPbvXSGsvwjJhUM"
}
```

Another example of a different JWK for the Bls12381 curve where the public key is in the G2 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls12381G2",
  "x": "CyWIj8dwP9uvuu9c9WZrVWvQnY2n7xQHF-BM-Mx6747t_ZJCu809fskA3ca3TVr7Atx1GqJH4rXlpTk0wg9LA6SsHrWWdPPqL2BXLd7zzMpFLeLdr-QQMJ2NPebFkw-h",
  "y": "DRC65Jv08EkVBojJkraUVEkxVhixgYS5hdfRfYZWaisT4M_NYoOFZFP4_5Jg05_pBk0BZG3RijbTl9YqzuAZZHuxj4eNxKBF4TD_6KdxmCxgAri0Bx46xtcMqqKgqUmd",
  "d": "1jeo0uurZvaIC82YUMNLOHalfvhabbMHtLeVaI0tNhs"
}
```

## Bls48581 Key Pairs

The following examples showcase JWKs for both the G1 and G2 subgroups of the Bls48581 curve. As before, note that the examples also include the corresponding private key, expressed through the inclusion of the “d” parameter.

An example JWK for the Bls48581 curve where the public key is in the G1 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls48581G1",
  "x": "BZo5kuiunkAUaE9n-c8L12pjTfV35ZTzei0eYIzYEM05Y--PCABHPXt20ImhyL9KsdrUqYxe5KrsbsahKYdN9dXa6a7fsFnWxw",
  "y": "EJfIRJ62OJ1vnLuwtziSgC4syIDVgiYoGxqLhdryqJEGepP7-4iePB5nrAthL6glMeKIeF9q7jsZfeM3g4CTlXMOH2yLCh1VyA",
  "d": "FbigQBwP9gNCFI_LFxt-c8ZpgY_1JjSOx-oUweZJgmGYZijWUsL6R0qQY1gwtrHTLWEWNbq--FlOVKL_EBlbL9I"
}
```

Another example of a different JWK for the Bls48581 curve where the public key is in the G1 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls48581G1",
  "x": "BjRT7VST4vMlTj6yMF73QlEg5XSHYQflAJMPf-4BhjO9qL8A6Rqo1OsdiGTrC3jYS3mLOHp-zg-yDmK_7HD8RR4S9j2Tf84blA",
  "y": "BOS-o0BD7RI7t2w5q0ljpy9NRJ7h8vT-sjfj8HKpGXaVP17RFUswlc1dBSVPsiSZRU78ZinNO0kK57dJrDXeRUB7gSM6L6aZog",
  "d": "pAq4V50JFEVjNbx12WDpHCiolrB-wxQw6wOZh2l_5-p_TWfTui7LgcIBpZb17idKgSh-XhGqhxEkXMUggGOQjJc"
}
```

An example JWK for the Bls48581 curve where the public key is in the G2 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls48581G2",
  "x": "ANAdJhZscIOfuf06ScGr1EwKcy7IFnq4bKPRl_QpBhRtx7Hg4hvpCXXbt03L1UGvoZbf-pxVfOyQMaA1qsGAHU0R6OYgUIpc-wE3KKZFLtzfS3vKPVLSLNTm7USw7audlChzhn8gaG5aJc1J7j0SfN0pn5TPLVA6PE47cMhHWHEirUzpkFxLTKYNqNoQVOZxuqcJs6UAC9ZwJaIivfDvv4XJuS02y7irAjJxhXhWfF9TuTyp6aiyGU1mjiYAoJ6HSIaJ2TiR5UztoiMjuUuOqZG4Rof5FuPGD665DvCmNurtsHM_KiT9gcJerJ_LbvJt-fqEIgEOnXCFlpcbxP0zoComUVEqzk-ArZ-a58hUVV0J6-7Y1lXJYDaqQPOX_YEiLrtudwfuoPz0iWLezmcI3bPx6g71SOjSQ7SiHU_kHG7IpWkg4uIq9zpQ_U66giDqmoglccwoMMyeG_LAd7zsuILtzOwPJOjJkC4U3HcAVZvSLQZz9dk1tuXR8ZfGuX1YHeTO6Hx-SA9Lz0F9yKJMBCUgd9ltimLO5JkzV3jGgin3IIHjM-XqEVFrtGCq0L775LRhmVg4CAb4bHaqvN2XW7mWordv9EvHq7NRwCVzMPFYWUd2-sXgqJ3AU0l5BqpngjH3gxSnG7mNEnmctmjQSnSOYhBoWRzmoJ2FmtQEFxCVIZvL7ZCaqkD27ZjamowAKFzmdFWV-wzHSs-br6ely00z402C7iX5c85zsI9NqxMwkDpprpx5oGjIFqiO8kkDyPdJSoLfzPA",
  "y": "DhTWwVEjNmqbRMtXexiuMVQEvL_ZqLk0NcjlzrlqO-k8uiRmKxiupQ8-Qm1tKYUJsJVdkLiyL14ptu0BbNoFIxUqLIP3PtauAgZjdSgUPOqDz7xvEaHGIhcEdivoKwokBpLQfO0xCSoia3tBm37xFWTLYNmmQQR8AEpUPk4OP9IWn5XkJQGYUfraO2qwNUmDdJwIJb8pDZVp_A0xYugO1CuKp-hlfNV6rdEYeGsYuMZelR9XlcNI9CdfsnQsfcoE5bypqeN5kJiCDNI0apdFQgxXD5xipr-6OafMARtcJclLbOlqzYL4l0coAwVbBe1Yz54yPGu4ubBI5N4dZcZ43c46hMFh_hz3qYkru5ClWjGtvRHUrBWNVTKoyyHAelBylnBckATeep2wHJmwE92fILQx-TQH3xYAwgx0o4ZC4_2gZXr73zddtS7mxaxWm6Y6AbHAoIpUBrqcMd9QjZ4_lX6BR1P_TKL_HySHOd4RMM-3H1fwe52V2E9Gi8tZJJzkLsWNT0F9MLYDULv1-4zRkPuKuqZGUfswU-cufNmx1U4TfrWpeqzwhQ8ih74AH8Fygk7ZpPwLAqFr_FNPoXeoxlRP4426_8Rsw-9fMjlDW3qqdEMI1M25BEm9kglUW__rn0OyFrCPfPDpUmlQxDmu50Uwi1m-8jnmGQxfDqzgrQqN79DUtD8C8m9y26zFttasdNMFEjE9vlq6GVbpYEr3juZhvKBvLzJLGGRQrxxGDPDGA_itNRJnw_EtafcH4rkWDtWV4yb79Sw",
  "d": "x-6jzDQgIvWIEyd5PF6nq9DNTIFesIExG_jBMJd9HJMtQdfDooFlQvbY3Ts0rSb-sPIIpLBx7bRCtwhkpioKmMM"
}
```

Another example of a different JWK for the Bls48581 curve where the public key is in the G2 subgroup.

```
{
  "kty": "OKP",
  "crv": "Bls48581G2",
  "x": "BgaTERiH8qDLfouNR44GOkiE2EOCgCbLxWEI3ocyXPLxeEtCCVUtnj3sQv-I3nM3V6IptFAoEJkLpCmMLBwyMTZ3B69p61yYBAU-fO2XyUiR0aEmWgqI-tesctbExIleiWWi221nwzIVUKi47E6bkNnBjvynvHMaB4lsJ0llYlsuYO7QqKXY55xSatMbTkTFncEOeEjQbDjHu96ODBD0F9r2-yahJ2PW5Jaztt9B0-UtHlTz0nje_ZaRTarna3-2p9ZrWM6DpXJQJg1dvefE1ngk7wJK0Agl0XQ8B0kKd3kkomO1CygV7MKN_OyKOJB2k4Vouv4i5MCC6GzIjUnF8vkakzzW-FjahSgKl1_QsvBrVSNwrGxpamhjpNn1Jx_FLbH2FAlE3Qv2sCveeEEoDGZ5qk1FbGhQFSicUlLhXymY1xBxMyqhoNPVy6_utYgeM9MPU9UtylGShAoIuE7qjzwH5OKY9rKj7z2z39gBdH12X6MoQwnn1zKQyhYyfHSmQcLN9Kmpke-5AQsNw0Oo8R05HhjhdR2GIWPIRHqCXN_cu__qof7jdtUupCoNUms3YyGMNYXdDEZzQS_8Fex6zWrHLCsTXqBrlAtKUIrzA-zAS_Jj_iIEvZRQM6J949P5iSEbSOtIalYzUuoOIjbF2s7bWpwNwgvKL1LbHT_ldARL9y0RxeK5BINrKCLA9aDDpK4rQX1B_ZR4ni422eQgfbdT6MjZ_YooobKKQxYCXdcx-DQsjhVMIHcr2RG_1450wmybP9Ycwpc",
  "y": "DoHzwzZAL1ArvHD-z7Eey67D4IEjfVgdXgANRGmtQzPirXDhOmohjnvMJ9KisyWmkJKuSp5B0wr-EO4AmmPualDZNutOC97NzAk2tJeVi5kNWPKp1RKxHIDhAqME-LUbVCTRFn23rfO9QNDd0dPev35o6ee3FwGtJYlieEeYEym0wHM96FxwfjsPrxgZYYorCToGES5rzbr1zOh0vmLVei-n-VWOnjymKh6CzjGLZm7q8exB8QZoG_JeugV7l0GifG4eeLfh4hDtYVNH7JCeJJENjfgDdp1Dy1fHDv7U9bX8gDEfRoTJS9FDU85a-tc3c5Oy472IzIq-Aml39YolBPISU1gNRO5RPK4gMad184pxtrYXZaMWOow3I2aUSDZ0-IBH4QLP8MSJoVxZHo5fhkpkSlFT1C5FgNiNny_I48EtANkHjH4S8zOxegLeTxK9doj2k6c2ptNAR7xDdm2mhfNhKIRgev3vhW70W54QpUpckS9iznbOl6ofzs-CtSezA7yMhLh_Vb0bgz6Wz2viixI6jd9_yVkicPse-57Fi24Et0x3A1CnuWkwEz1TvXk-9QWFQWKhAsoIMCB0svCjgcnSKcaAeRFGvd0XKtBICmHhCK1CqvcAuYtsDELmhzsiJVxOpmB5JdgDt2Ii6CaiAzxkP-I02O6UbdzGbZPN0gJfgIJRMuvJRZF-G9An1CsTMD6Y0Q4Ro3aWrSY3mpnKRZddVC2W4OKuhS7qrLR7REl-gJvSCzaqiuLQlvqOrP4i55n52tDVWqQ",
  "d": "TeLY_9dGUWdbaMjjgWA8-osKXvyfPz3gq4B9Drr-dV0ZgB3hhYBWlkklKOuO78t8FUCypARd4bpHGKJmsPafn8A"
}
```

# COSE_Key Examples

## Bls12381 Key Pairs

The following showcase COSE_key examples for both the G1 and G2 subgroups of the Bls12381 curve. Note, the examples also include the corresponding private key, expressed through the inclusion of the “d” (-4) parameter.

An example COSE_Key for the Bls12381 curve where the public key is in the G1 subgroup expressed as an octet string.

```
a50101200d21583011de060462d56ac129e1e8cfcf8e02be595b4e575f2cb726d90708265b812faa7f490991ad9bc935add8689f96c7c22622583001b75802c01bdb4087ce5556e9504ef62d7a05c18e99c62230b941121f437c0883bbfd59200775cdec128bd7fae75ee0235820de773afecdfc1555656b06f098f14e8c1e139403f2fcad93c77f48ed79c49c37
```

Another example of a different COSE_Key for the Bls12381 curve where the public key is in the G1 subgroup expressed as an octet string.

```
a50101200d215830114190131a73c5e6f099711ea9cdfd388dc9d4d7d408c5503ea73f2dbfb874bbb4c42692addd2b0c131316d85de6bfb622583014da577e9e1afb937b09be76f29c1184b20d3a6c32ba18ca8cfcfc8dc6b180248d6426e369842c1a83a17ed46a58aa6023582039d341ad10eef3d22b9e56391e5fb6096fb772a5b4470ec881052ec0b67c815e
```

An example COSE_Key for the Bls12381 curve where the public key is in the G2 subgroup expressed as an octet string.

```
a50101200e21586011adac1f83c164b6980a7f2c7bed5c96608e443b0ac5b6f0dc9b3f025bb83a6915f6099b26ccb5605dabb7b571cece92023b3c96cb538284e05cc17a4177728779bc9368b1c5444660b31a6329552bfc7417c1e113cce4d185a2195dc21f0a50225860055fefbc243adf649c028a041315eecf521e407f43da8fae63f7408d9dfb607b91304cb387c4eafbdd091d0be270eab1055924ad51226fe3ff14c3c736dab127298fde957e1fc7c276f3e4a16227a457ccf6d562caa0fc2409b00dfeb8cd4da1235820851e877f1953c1c8cc1924f9c189e41a226fb959e950f6ef5d21acbf08c98543
```

Another example of a different COSE_Key for the Bls12381 curve where the public key is in the G2 subgroup expressed as an octet string.

```
a50101200e2158600b25888fc7703fdbafbaef5cf5666b556bd09d8da7ef140717e04cf8cc7aef8eedfd9242bbcd3d7ec900ddc6b74d5afb02dc751aa247e2b5e5a53934c20f4b03a4ac1eb59674f3ea2f60572ddef3ccca452de2ddafe410309d8d3de6c5930fa12258600d10bae49bf4f049150688c992b6945449315618b18184b985d7d17d86566a2b13e0cfcd6283856453f8ff9260d39fe9064d01646dd18a36d397d62acee019647bb18f878dc4a045e130ffe8a771982c6002b8b4071e3ac6d70caaa2a0a9499d235820d637a8d2ebab66f6880bcd9850c34b3876a57ef85a6db307b4b795688d2d361b
```

## Bls48581 Key Pairs

The following showcase COSE_key examples for both the G1 and G2 subgroups of the Bls48581 curve. Note, the examples also include the corresponding private key, expressed through the inclusion of the “d” (-4) parameter.

An example COSE_Key for the Bls48581 curve where the public key is in the G1 subgroup expressed as an octet string.

```
a50101200f2158490522e96ca7ca18bf31598a15d39aa95c6fe56da85a7bc8e8108f193c54ea68c84b973ed2f29725ed7a1413329699258050e2c69628d83d0cc4b83bb10fafae7fa535ad21a1fef91eaf225849003b9940c85d62aba1e9955d6b1836d01bad300b886d0ae97df1305b0bfbca337fad9662581647ca6cf11b861e3e71642d5d82c254774fb67937a23745c2d1f328898a53eac0cad87e2358416e9dcfdc9abd54a06233f8b0d49ef665362ebf4539a8d83fe273b4c54ff36e8b600823e2695560e4615d9a866e929b918e8183fa89660c1fe684e3bd671cf29ece
```

Another example of a different COSE_Key for the Bls48581 curve where the public key is in the G1 subgroup expressed as an octet string.

```
a50101200f215849063453ed5493e2f3254e3eb2305ef7425120e574876107e500930f7fee018633bda8bf00e91aa8d4eb1d8864eb0b78d84b798b387a7ece0fb20e62bfec70fc451e12f63d937fce1b9422584904e4bea34043ed123bb76c39ab4963a72f4d449ee1f2f4feb237e3f072a91976953f5ed1154b3095cd5d05254fb22499454efc6629cd3b490ae7b749ac35de45407b81233a2fa699a2235841a40ab8579d0914456335bc75d960e91c28a896b07ec31430eb039987697fe7ea7f4d67d3ba2ecb81c201a596f5ee274a81287e5e11aa8711245cc5208063908c97
```

An example COSE_Key for the Bls48581 curve where the public key is in the G2 subgroup expressed as an octet string.

```
a5010120102159024800d01d26166c70839fb9fd3a49c1abd44c0a732ec8167ab86ca3d197f42906146dc7b1e0e21be90975dbb74dcbd541afa196dffa9c557cec9031a035aac1801d4d11e8e620508a5cfb013728a6452edcdf4b7bca3d52d22cd4e6ed44b0edab9d942873867f20686e5a25cd49ee3d127cdd299f94cf2d503a3c4e3b70c847587122ad4ce9905c4b4ca60da8da1054e671baa709b3a5000bd67025a222bdf0efbf85c9b92d36cbb8ab0232718578567c5f53b93ca9e9a8b2194d668e2600a09e87488689d93891e54ceda22323b94b8ea991b84687f916e3c60faeb90ef0a636eaedb0733f2a24fd81c25eac9fcb6ef26df9fa8422010e9d708596971bc4fd33a02a2651512ace4f80ad9f9ae7c854555d09ebeed8d655c96036aa40f397fd81222ebb6e7707eea0fcf48962dece6708ddb3f1ea0ef548e8d243b4a21d4fe41c6ec8a56920e2e22af73a50fd4eba8220ea9a882571cc2830cc9e1bf2c077bcecb882edccec0f24e8c9902e14dc7700559bd22d0673f5d935b6e5d1f197c6b97d581de4cee87c7e480f4bcf417dc8a24c04252077d96d8a62cee499335778c68229f72081e333e5ea11516bb460aad0befbe4b4619958380806f86c76aabcdd975bb996a2b76ff44bc7abb351c0257330f158594776fac5e0a89dc053497906aa678231f78314a71bb98d12799cb668d04a748e621068591ce6a09d859ad404171095219bcbed909aaa40f6ed98da9a8c00285ce6745595fb0cc74acf9bafa7a5cb4d33e34d82ee25f973ce73b08f4dab1330903a69ae9c79a068c816a88ef24903c8f7494a82dfccf0225902480e14d6c15123366a9b44cb577b18ae315404bcbfd9a8b93435c8e5ceb96a3be93cba24662b18aea50f3e426d6d298509b0955d90b8b22f5e29b6ed016cda0523152a2c83f73ed6ae0206637528143cea83cfbc6f11a1c6221704762be82b0a240692d07ced31092a226b7b419b7ef11564cb60d9a641047c004a543e4e0e3fd2169f95e425019851fada3b6ab0354983749c0825bf290d9569fc0d3162e80ed42b8aa7e8657cd57aadd118786b18b8c65e951f5795c348f4275fb2742c7dca04e5bca9a9e3799098820cd2346a9745420c570f9c62a6bfba39a7cc011b5c25c94b6ce96acd82f897472803055b05ed58cf9e323c6bb8b9b048e4de1d65c678ddce3a84c161fe1cf7a9892bbb90a55a31adbd11d4ac158d5532a8cb21c07a507296705c9004de7a9db01c99b013dd9f20b431f93407df1600c20c74a38642e3fda0657afbdf375db52ee6c5ac569ba63a01b1c0a08a5406ba9c31df508d9e3f957e814753ff4ca2ff1f248739de1130cfb71f57f07b9d95d84f468bcb59249ce42ec58d4f417d30b60350bbf5fb8cd190fb8abaa64651fb3053e72e7cd9b1d54e137eb5a97aacf0850f2287be001fc172824ed9a4fc0b02a16bfc534fa177a8c6544fe38dbaffc46cc3ef5f3239435b7aaa744308d4cdb90449bd9209545bffeb9f43b216b08f7cf0e9526950c439aee745308b59bef239e6190c5f0eace0ad0a8defd0d4b43f02f26f72dbacc5b6d6ac74d30512313dbe5aba1956e9604af78ee661bca06f2f324b186450af1c460cf0c603f8ad351267c3f12d69f707e2b9160ed595e326fbf52c235841c7eea3cc342022f5881327793c5ea7abd0cd4c815eb081311bf8c130977d1c932d41d7c3a2816542f6d8dd3b34ad26feb0f208a4b071edb442b70864a62a0a98c3
```

Another example of a different COSE_Key for the Bls48581 curve where the public key is in the G2 subgroup expressed as an octet string.


```
a50101201021590248060693111887f2a0cb7e8b8d478e063a4884d843828026cbc56108de87325cf2f1784b4209552d9e3dec42ff88de733757a229b4502810990ba4298c2c1c3231367707af69eb5c9804053e7ced97c94891d1a1265a0a88fad7ac72d6c4c4895e8965a2db6d67c3321550a8b8ec4e9b90d9c18efca7bc731a07896c274965625b2e60eed0a8a5d8e79c526ad31b4e44c59dc10e7848d06c38c7bbde8e0c10f417daf6fb26a12763d6e496b3b6df41d3e52d1e54f3d278defd96914daae76b7fb6a7d66b58ce83a57250260d5dbde7c4d67824ef024ad00825d1743c07490a777924a263b50b2815ecc28dfcec8a389076938568bafe22e4c082e86cc88d49c5f2f91a933cd6f858da85280a975fd0b2f06b552370ac6c696a6863a4d9f5271fc52db1f6140944dd0bf6b02bde7841280c6679aa4d456c685015289c5252e15f2998d71071332aa1a0d3d5cbafeeb5881e33d30f53d52dca5192840a08b84eea8f3c07e4e298f6b2a3ef3db3dfd801747d765fa3284309e7d73290ca16327c74a641c2cdf4a9a991efb9010b0dc343a8f11d391e18e1751d862163c8447a825cdfdcbbffeaa1fee376d52ea42a0d526b3763218c3585dd0c4673412ffc15ec7acd6ac72c2b135ea06b940b4a508af303ecc04bf263fe2204bd945033a27de3d3f989211b48eb486a563352ea0e2236c5dacedb5a9c0dc20bca2f52db1d3fe574044bf72d11c5e2b904836b2822c0f5a0c3a4ae2b417d41fd94789e2e36d9e4207db753e8c8d9fd8a28a1b28a4316025dd731f8342c8e154c20772bd911bfd78e74c26c9b3fd61cc297225902480e81f3c336402f502bbc70fecfb11ecbaec3e081237d581d5e000d4469ad4333e2ad70e13a6a218e7bcc27d2a2b325a69092ae4a9e41d30afe10ee009a63ee6a50d936eb4e0bdecdcc0936b497958b990d58f2a9d512b11c80e102a304f8b51b5424d1167db7adf3bd40d0ddd1d3debf7e68e9e7b71701ad2589627847981329b4c0733de85c707e3b0faf1819618a2b093a06112e6bcdbaf5cce874be62d57a2fa7f9558e9e3ca62a1e82ce318b666eeaf1ec41f106681bf25eba057b9741a27c6e1e78b7e1e210ed615347ec909e24910d8df803769d43cb57c70efed4f5b5fc80311f4684c94bd14353ce5afad7377393b2e3bd88cc8abe026977f58a2504f21253580d44ee513cae2031a775f38a71b6b61765a3163a8c37236694483674f88047e102cff0c489a15c591e8e5f864a644a5153d42e4580d88d9f2fc8e3c12d00d9078c7e12f333b17a02de4f12bd7688f693a736a6d34047bc43766da685f3612884607afdef856ef45b9e10a54a5c912f62ce76ce97aa1fcecf82b527b303bc8c84b87f55bd1b833e96cf6be28b123a8ddf7fc9592270fb1efb9ec58b6e04b74c770350a7b96930133d53bd793ef505854162a102ca08302074b2f0a381c9d229c680791146bddd172ad0480a61e108ad42aaf700b98b6c0c42e6873b22255c4ea6607925d803b76222e826a2033c643fe234d8ee946ddcc66d93cdd2025f80825132ebc945917e1bd027d42b13303e98d10e11a37696ad26379a99ca45975d542d96e0e2ae852eeaacb47b44497e809bd20b36aa8ae2d096fa8eacfe22e799f9dad0d55aa42358414de2d8ffd74651675b68c8e381603cfa8b0a5efc9f3f3de0ab807d0ebafe755d19801de185805696492528eb8eefcb7c1540b2a4045de1ba4718a266b0f69f9fc0
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

<reference anchor="SEC1" target="http://www.secg.org/sec1-v2.pdf">
  <front>
    <title>SEC 1: Elliptic Curve Cryptography</title>
    <author><organization>Standards for Efficient Cryptography Group</organization></author>
  </front>
</reference>
