# WE BUILD Attestation Rulebook for attestations of type *Membership Credential*

*This WE BUILD v1 Rulebook specifies the Membership Credential used in the WE BUILD SC2 MVP
scenario "seamless onboarding" piloted by the Data Sharing Initiatives (DSI) DjustConnect (ILVO),
DADS (ITC) and Tritom (Dataspace Europe), and the common agriculture dataspace "Agri-X".*

* Author(s):
    * Ingo Wolf, [Spherity GmbH](https://www.spherity.com)
    * [NAME SURNAME, AFFILIATION]
* Previous Authors
    * [NAME SURNAME, AFFILIATION (versions)]

| Version          | Date               | Description |
|------------------|--------------------|------------|
| 01               | 25.06.2026         | Initial Rulebook derived from the MVP Membership credential attestation description. |
| [VERSION NUMBER] | [PUBLICATION DATE] | [DESCRIPTION OR LINK TO THE CHANGELOG] |

**Feedback:**
* Main feedback channel: [GitHub issues](https://github.com/webuild-consortium/eudi-wallet-rulebooks-and-schemas/issues)
* Alternative: Contact Business usecase 2 contact points in WE BUILD.

## 1 Introduction

### 1.1 Document scope and purpose

The Membership Credential expresses that its holder is a member of a specific Data Sharing
Initiative (DSI) or dataspace, which roles the holder has within that DSI or dataspace, and to
which dataspace governance rulebook the holder has conformed. It exists to enable *seamless
onboarding*: common information already presented in a previous onboarding flow is trusted and not
requested again when the holder onboards into a further DSI or the common dataspace.

In plain language, the credential combines all necessary information about the dataspace or DSI
membership into a single attestation held in an EU Business Wallet (EBW), independent of any
dataspace-specific connector technology.

* **Real-world fact expressed:** membership of a DSI or dataspace, the roles held within it
  (e.g. Data Rights Holder, Data Provider, Data Consumer, Operator, Onboarding Service Provider),
  and acceptance of the dataspace governance rulebook.
* **Issuers:** trusted issuers / onboarding service providers (the participating DSIs, or another
  technology partner acting as onboarding service provider). Agri-X is envisaged as a federation of
  DSIs; participants join Agri-X via a DSI or directly via a trusted issuer.
* **Holders:** organisations (legal persons) participating in a DSI or dataspace. The MVP scenario
  is limited to legal persons.
* **Relying parties:** DSI / dataspace platforms and participants that verify membership and roles
  during onboarding and data-sharing transactions.
* **Use case:** WE BUILD SC2 "seamless onboarding" piloted by DjustConnect (ILVO), DADS (ITC) and
  Tritom (Dataspace Europe), targeting the common agriculture dataspace "Agri-X".
* **Source document:** *MVP Membership credential — attestation description*.

The schema is designed to be re-usable and interoperable with other dataspaces. It is aligned with
the membership credential used within Catena-X; attributes that must remain interoperable with such
operational dataspaces are marked as dataspace-specific (DS-specific) terminology that SHALL NOT be
renamed.

### 1.2 Document structure

* Chapter 2, which describes the attestation attributes and metadata in an
  encoding-independent manner.
* Chapter 3, which specifies how the attestation
  attributes and metadata are encoded in case the attestation complies with [ISO/IEC
  18013-5] and/or [SD-JWT VC] and/or [W3C VCDM v2.0]. Each encoding SHALL be specified in a separate section, or even in a separate chapter.
* Chapter 4, which specifies attestation usage.
* Chapter 5, which defines how trust anchors for attestation verification can be obtained.
* Chapter 6, which defines attestation revocation mechanisms.
* Chapter 7, which provides compliance information.

### 1.3 Key words

This document uses the capitalised key words 'SHALL', 'SHOULD' and 'MAY' as
specified in [RFC 2119], i.e., to indicate requirements, recommendations and
options specified in this document.

In addition, 'must' (non-capitalised) is used to indicate an external
constraint, i.e., a requirement that is not mandated by this document, but, for
instance, by an external document. The word 'can' indicates a capability,
whereas other words, such as 'will', and 'is' or 'are' are intended as
statements of fact.

### 1.4 Terminology

This document uses the terminology specified in Annex 1 of the ARF.

## 2 Attestation attributes and metadata

### Chapter overview and requirements

*This chapter is used for defining all attributes that an
attestation of the defined type may contain. In this section
the attributes SHALL be defined in an encoding-independent manner (see ARB_06 in [Topic 12]).
Each attribute can be mandatory, optional, or conditional
and this SHALL be specified in the corresponding section (see ARB_09 in [Topic 12]).*

*When attributes are defined, referring to attributes that
already exist in a catalogue of attestation attributes
SHOULD be considered (see ARB_07 in [Topic 12]).*

*Where use-case documentation or an attestation description already defines attribute meanings,
logical models, code lists, or integrity constraints, authors SHOULD align terminology with those
sources and may copy and refine that material for this Rulebook.*

*[Topic 12] of Annex 2 of the ARF defines the following High-Level Requirements with
respect to the Attestation Rulebooks:*

**Requirements for QEAA**

* An attribute as meant in Annex V point a)  of the [European Digital Identity Regulation]
  SHALL be included (see ARB_11 in [Topic 12]). See also section 2.1.
* One or more attributes or metadata representing the set of data meant in Annex
  V point b) of the [European Digital Identity Regulation] SHALL be included (see ARB_13 in [Topic 12])
* One or more attributes representing the set of data meant in Annex V point c)  
  of the [European Digital Identity Regulation] SHALL be included (see ARB_16 in [Topic 12]).
* One or more attributes or metadata representing the set of data meant in Annex V point e)
  of the [European Digital Identity Regulation] SHALL be included (see ARB_18 in [Topic 12]).
* One or more attributes or metadata representing the location meant in Annex V point h)
  of the [European Digital Identity Regulation] SHALL be included. This location SHALL
  indicate at least the URL at which a machine-readable version of the trust anchor to be
  used for verifying the QEAA can be found or looked up (see ARB_20 in [Topic 12]).

**Requirements for PuB-EAA**

* An attribute as meant in  Annex VII point a) of the [European Digital Identity Regulation]
  SHALL be included (see ARB_11 in [Topic 12]). See also section 2.1.
* One or more attributes or metadata representing the set of data meant in Annex
  VII point b) of the [European Digital Identity Regulation] SHALL be included (see ARB_14 in [Topic 12]).
* One or more attributes representing the set of data meant in Annex VII point c)
  of the [European Digital Identity Regulation] SHALL be included (see ARB_16 in [Topic 12]).
* One or more attributes or metadata representing the set of data meant in Annex VII point e)
  of the [European Digital Identity Regulation] SHALL be included (see ARB_18 in [Topic 12]).
* one or more attributes or metadata representing the location meant in Annex VII point h)
  of the [European Digital Identity Regulation] SHALL be included. This location SHALL
  indicate at least the URL at which a machine-readable version of the qualified
  certificate that signed the PuB-EAA can be found or looked up. (see ARB_20 in [Topic 12])

**Requirements for non-qualified EAA**

* An attribute indicating that the attestation is an EAA should be included (see ARB_12 in [Topic 12]).
  See also section 2.1.
* One or more attributes or metadata representing the set of data meant in Annex
  V point b) of the [European Digital Identity Regulation] SHALL be included (see ARB_15 in [Topic 12]).
* One or more attributes representing the set of data meant in Annex V point c) of the
  [European Digital Identity Regulation] SHOULD be included (see ARB_17 in [Topic 12])
* One or more attributes representing the set of data meant in Annex V point e) of
  the [European Digital Identity Regulation] SHOULD be defined (see ARB_19 in [Topic 12]).
* One or more attributes or metadata representing the location at which a machine-readable
  version of the trust anchor to be used for verifying the EAA can be found or
  looked up SHOULD be defined. What this location indicates precisely is dependent
  on the nature of the mechanism used for distributing trust anchors, detailed in section
  5 (see ARB_21 in [Topic 12])

### 2.1 Introduction

The Membership Credential is a **non-qualified EAA**. This document defines the attribute
[attestationLegalCategory](https://w3id.org/ebwv#attestationLegalCategory) which SHALL have the value `EAA`.

The credential describes the member (the `CredentialSubject`) together with the dataspace
governance rulebook the member (holder of the credential) conforms to (`conformanceTo`), the roles the member has within the
DSI or dataspace (`role`), and information about the partner that onboarded the member
(`onboardedBy`).

**Logical model.** The Membership Credential is structured as follows:

* The credential subject is identified by `id` (a UUID, may change) and `member` (a stable
  identifier; for the MVP this re-uses the EUID from the EUBWOID of that EconomicOperator).
* `memberOf` names the DSI or dataspace the membership is for.
* `role` is an array of roles the holder has within that DSI or dataspace. The membership and its
  set of roles share the same lifecycle: a change of roles requires re-issuance of the credential.
* `termsOfUse` is an object of type [GovernanceRulebook](https://w3id.org/ebwv#GovernanceRulebook) referencing the accepted dataspace governance rulebook (URL,
  version, SHA-256 hash, and acceptance datetime).
* `onboardedBy` is an object of type [Platform](https://w3id.org/ebwv#Platform) describing the platform-specific identifier (platformId) and name as well as the operator (partner / service provider) that onboarded the member.

Standard credential metadata (issuer, issuance time, expiry, status) follows from the chosen VC
format and is documented in Chapter 3 rather than as attributes here. Each Membership Credential
has its own unique identifier.

**DS-specific terminology.** Attributes marked `(*)` below (`id`, `holderIdentifier`, `memberOf`)
keep their names for cross-dataspace interoperability (e.g. with Catena-X) and SHALL NOT be
renamed.

*Subsections 2.2 - 2.7 define the attributes and metadata in an encoding-independent manner. Code
lists are in Section 2.8 and integrity rules in Section 2.9. The structured objects `conformanceTo`
and `onboardedBy` are defined as sub-tables in Section 2.2.*

### 2.2 Mandatory attributes of object [Membership](https://w3id.org/ebwv#Membership)

| **Data Identifier**          | **Semantic Reference**                                                     | **Definition**                                                                                                                                                                                                                                                                | **Data type**                                                  | **Example value**                     |
|------------------------------|----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|---------------------------------------|
| `attestation_legal_category` | [attestationLegalCategory](https://w3id.org/ebwv#attestationLegalCategory) | Indication that the attestation is a non-qualified EAA (per ARB_12).                                                                                                                                                                                                          | string                                                         | [EAA](https://w3id.org/ebwv#EAA)      |
| `id` (*)                     | @id                                                                        | UUID of the credential subject. May change, in contrast to `holderIdentifier` which is a persistent, stable identifier. Name kept for cross-dataspace interoperability.                                                                                                       | string (UUID)                                                  | `did:web:example.com:participant:123` |
| `member` (**)                | [member](https://w3id.org/ebwv#member)                                     | Stable identifier that uniquely identifies the holder. For the MVP this re-uses the EUID (part of the EUBWOID); scope is legal persons only. Name kept for cross-dataspace interoperability. #Therefore we go with EconomicOperator which has a legalName and legalIdentifier | [EconomicOperator](https://w3id.org/ebwv#EconomicOperator)     | `BEEUID...`                           |
| `memberOf` (*)               | [memberOf](https://w3id.org/ebwv#memberOf)                                 | The DSI or dataspace the holder is a member of. Within a DSI/DS all issued membership credentials use the same value. Name kept for cross-dataspace interoperability. See code list 2.8.                                                                                      | string                                                         | `Agri-X`                              |
| `role`                       | [role](https://w3id.org/ebwv#role)                                         | Array of roles the holder has within the DSI or dataspace. A member may have multiple roles; the set of roles shares the membership lifecycle. See code list 2.8.                                                                                                             | array of strings                                               | `["DataProvider","DataConsumer"]`     |
| `termsOfUse`                 | [termsOfUse](https://www.w3.org/2018/credentials/#termsOfUse)              | Dataspace governance rulebook information. Object, see table below.                                                                                                                                                                                                           | [GovernanceRulebook](https://w3id.org/ebwv#GovernanceRulebook) | *see 2.2.1*                           |
| `onboardedBy`                | [platform](https://w3id.org/ebwv#platform)                                 | The partner or service provider that onboarded the holder into the DSI or dataspace, including identifiers used by that DSI/DS. Object, see table below.                                                                                                                      | [Platform](https://w3id.org/ebwv#Platform)                     | *see 2.2.2*                           |

#### 2.2.1 `termsOfUse` object of type [GovernanceRulebook](https://w3id.org/ebwv#GovernanceRulebook)

| **Data Identifier** | **Semantic Reference** | **Definition** | **Data type** | **Optionality** | **Example value** |
|---------------------|------------------------|----------------|---------------|-----------------|-------------------|
| `url`               | N/A                    | Reference to the online dataspace governance rulebook for the given DSI or DS. | string (URI) | M | `https://agri-x.eu/rulebook` |
| `version`           | N/A                    | Version of the online rulebook at the time of acceptance. | string | M | `1.2` |
| `hash`              | N/A                    | SHA-256 hash of the rulebook, for quick comparison. | string (SHA-256 hash) | M | `9f86d081...` |
| `acceptedAt`        | N/A                    | Datetime when the rulebook was accepted. May differ from issuance time in an outbound flow where a credential is issued to an existing member based on a previously completed onboarding flow. | datetime | O | `2026-06-01T10:00:00Z` |

#### 2.2.2 `onboardedBy` object of [Platform](https://w3id.org/ebwv#Platform)

| **Data Identifier** | **Semantic Reference**                                     | **Definition**                                                                                  | **Data type** | **Optionality** | **Example value**         |
|---------------------|------------------------------------------------------------|-------------------------------------------------------------------------------------------------|---------------|-----------------|---------------------------|
| `platformId`        | N/A                                                        | DID identifying the partner/platform that onboarded the member.                                 | string (DID)  | M               | `did:web:djustconnect.be` |
| `name`              | N/A                                                        | Commercial name of the DSI or partner that onboarded the holder.                                | string        | M               | `DjustConnect`            |
| `operator`          | [EconomicOperator](https://w3id.org/ebwv#EconomicOperator) | Legal name and identifier of the organisation that onboarded the holder and hosts the platform. | object        | M               | see 2.3.3                 | 


#### 2.2.3 `operator` object of [EconomicOperator](https://w3id.org/ebwv#EconomicOperator)

| **Data Identifier** | **Semantic Reference**                                     | **Definition**                                                                                  | **Data type** | **Optionality** | **Example value** |
|---------------------|------------------------------------------------------------|-------------------------------------------------------------------------------------------------|---------------|-----------------|-------------------|
| `legalName`         | N/A                                                        | Legal name and identifier of the organisation that onboarded the holder and hosts the platform.                            | string (DID)  | M               | `ILVO`            |
| `identifier`        | [identifier](https://w3id.org/ebwv#identifier)             | Commercial name of the DSI or partner that onboarded the holder.                                | string        | M               | `BE0123456789`    |

### 2.3 Optional attributes

| **Data Identifier** | **Semantic Reference** | **Definition** | **Data type** | **Example value** |
|------------------------|--------------------------|--------------|--------------|--------------|
| `legalName` | N/A | Official name of the holder. Optional, contributes to readability when the holder is a legal person. See integrity rule IR-02. | string | `Farm Example BV` |

### 2.5 Mandatory metadata

*Standard VC metadata (issuer, issuance time, expiry, status, credential `id`) is provided by the
chosen VC format and specified per encoding in Chapter 3. No additional mandatory metadata is
defined by this Rulebook beyond `attestation_legal_category` (Section 2.2).*

| **Data Identifier** | **Semantic Reference** | **Definition** | **Data type** | **Example value** |
|------------------------|--------------------------|--------------|--------------|--------------|
| N/A | N/A | N/A | N/A | N/A |

### 2.6 Optional metadata

| **Data Identifier** | **Semantic Reference** | **Definition** | **Data type** | **Example value** |
|------------------------|--------------------------|--------------|--------------|--------------|
| N/A | N/A | N/A | N/A | N/A |

### 2.7 Conditional metadata

| **Data Identifier** | **Semantic Reference** | **Definition** | **Data type** | **Example value** |
|------------------------|--------------------------|--------------|--------------|--------------|
| N/A | N/A | N/A | N/A | N/A |

### 2.8 Code lists

The following code lists apply to attributes defined in Section 2. Lists are kept fit-for-pilot:
they cover the minimal set needed by the three participating DSIs and are not exhaustive.

**`memberOf`** — fixes the DSI or dataspace the holder is a member of. The value is decided by the
governance level of the DSI or dataspace.

| **Field name** | **Allowed values** | **Meaning** | **Source / vocabulary** | **Notes / extensibility** |
|----------------|--------------------|-------------|--------------------------|---------------------------|
| `memberOf` | `Agri-X` | The common agriculture dataspace (federation level). | WE BUILD SC2 use case | Casing as listed. Additional values are set by DSI/DS governance. |
| `memberOf` | `DADS` | DSI operated by ITC. | WE BUILD SC2 use case | |
| `memberOf` | `DjustConnect` | DSI operated by ILVO. | WE BUILD SC2 use case | |
| `memberOf` | `Tritom` | DSI operated by DataSpace Europe Oy. | WE BUILD SC2 use case | |

**`role`** — roles a member can have. A member may have multiple roles in one credential. Based on
the common roles agreed across the piloting DSIs, treating Agri-X as a federation of DSIs. Not
exhaustive; other DSIs may map their own role names to these.

| **Field name** | **Allowed values** | **Meaning** | **Source / vocabulary** | **Notes / extensibility** |
|-----------|--------------------|-------------|--------------------------|---------------------------|
| `role` | `DataRightsHolder` | Owns the rights to the data (e.g. farmers) and can consent to data being shared from a data provider to a data consumer. | WE BUILD SC2 use case | Non-exhaustive; mappable to DSI-specific names |
| `role` | `DataProvider` | Partner that enables sharing data, e.g. via an API. | WE BUILD SC2 use case | |
| `role` | `DataConsumer` | Partner that wants to use data, e.g. by calling an API. | WE BUILD SC2 use case | |
| `role` | `Operator` | Partner providing DSSC building-block services (identity management, consent management, logging, …). | WE BUILD SC2 use case (custom role part of DADS) | |
| `role` | `OnboardingServiceProvider` | Partner that onboards members into Agri-X; may be a DSI or another technology partner. | WE BUILD SC2 use case (role part of Catena-X) | |

**`onboardedBy.holderIdentifierType`** — defines how `onboardedBy.holderIdentifier` is interpreted.
Most values refer to identifiers assigned by Business Registries. Given the open-ended range of
member-state and domain-specific identifiers, this list need not be strictly enforced as a code
list.

| **Field name**                          | **Allowed values** | **Meaning** | **Source / vocabulary** | **Notes / extensibility** |
|-----------------------------------------|--------------------|-------------|--------------------------|---------------------------|
| `onboardedBy.operator.identifier.value` | `PlatformSpecific` | Custom identifier type whose meaning is platform-specific and not known to other partners. | DSI-specific | Non-enforced; extensible per DSI / member state |
| `onboardedBy.operator.identifier.value`      | `VAT-ID` | VAT identifier. Used by DjustConnect and Tritom. | [European Business Wallet Vocabulary v0.1] | |
| `onboardedBy.operator.identifier.value`      | `KMG-MID` | Unique farm id used in Slovenia, issued by the Ministry of Agriculture. Used by the DADS platform. | National (SI) | |

**`onboardedBy.operator.identifier.value`** (credential subject) — intended to distinguish between the different identifier types for legal persons. The MVP scenario pilots only legal persons (re-using
the EUID from the EUBWOID), so this attribute is a future enhancement.

[identifier.type]: A common code list for `identifier.type` distinguishing Legal Person from
Natural Person is being prepared by the WE BUILD Semantics work group. Values to be added once
provided.

### 2.9 Integrity rules

| **Rule ID** | **Rule statement**                                                                                                                  | **Why it exists** | **Where enforced** | **Verifier / issuer behavior on failure** |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------|-------------------|--------------------|-------------------------------------------|
| `IR-01` | If `termsOfUse.acceptedAt` is present, its value SHALL be a datetime less than or equal to the issuance datetime of the credential. | Acceptance of the rulebook cannot logically occur after the credential was issued; supports outbound flows where acceptance happened during an earlier onboarding. | Issuer business rules, schema validation, and verifier business validation. | Issuer SHALL reject the value; verifier SHALL treat `termsOfUse` as invalid. |
| `IR-02` | If the member is a Legal Person, `legalName` SHOULD be present.                                                                     | Improves readability and identification of legal-person holders. | Issuer business rules. | Issuer SHOULD populate `legalName`; verifier MAY warn if absent. |

# 3 Attestation encoding

## 3.1 ISO/IEC 18013-5-compliant encoding

*Not applicable for this version.* The Membership Credential is used in online onboarding and
data-sharing flows; no proximity / offline presentation requirement applies (ARB_02). The primary
format is W3C VCDM (JSON-LD), see Section 3.3, with SD-JWT VC considered as an additional flow
(Section 3.2). No ISO/IEC 18013-5 mdoc document type is defined.

## 3.2 SD-JWT VC-based encoding

*Considered as an additional flow (not the primary format for the MVP).* The Membership Credential
MAY additionally be piloted as an SD-JWT VC. If issued in this format, attestations
SHALL comply with the 'SD-JWT VCs' profile specified in [HAIP] (ARB_01b). This Rulebook follows
the catalog baseline of [HAIP] draft-03 and [SD-JWT VC] draft-ietf-oauth-sd-jwt-vc-09.

**Verifiable Credential Type (`vct`):** `eu.we-build.ds-membership.1`

The issued SD-JWT VC SHALL use the JOSE `typ` header value required by [SD-JWT VC]. For draft -09,
this value is `dc+sd-jwt`.

### 3.2.1 IANA-registered and standard JWT / SD-JWT VC claims

The following claims are standard JWT or SD-JWT VC claims.

| **Data Identifier** | **Attribute identifier** | **Encoding format** | **Reference / Notes** | **Disclosable** |
|---------------------|--------------------------|---------------------|-----------------------|-----------------|
| `iss` | `iss` | string (HTTPS URL) | Issuer identifier. | MUST NOT |
| `iat` | `iat` | NumericDate | Issued-at timestamp. | MUST NOT |
| `nbf` | `nbf` | NumericDate | Not-before timestamp, where used. | MUST NOT |
| `exp` | `exp` | NumericDate | Expiration timestamp. | MUST NOT |
| `jti` | `jti` | string | Unique credential instance identifier. | MUST NOT |
| `vct` | `vct` | string | SHALL be `eu.we-build.ds-membership.1`. | MUST NOT |
| `status` | `status` | JSON object | Status-list revocation information. See Section 3.2.3. | MUST NOT |
| `cnf` | `cnf` | JSON object | Holder binding confirmation claim, where used. | MUST NOT |

### 3.2.2 Private names specific to the Membership Credential

The following private claims map to the attributes defined in Chapter 2.

| **Data Identifier**                | **Attribute identifier** | **Encoding format** | **Reference / Notes** | **Disclosable** |
|------------------------------------|--------------------------|---------------------|-----------------------|-----------------|
| `attestationLegalCategory`         | `attestation_legal_category` | string | SHALL be `EAA`. | MUST NOT |
| `id`                               | `id` | string (DID) | DID of the credential subject. | MUST |
| `holderIdentifier`                 | `holderIdentifier` | string | Stable holder identifier. For the MVP, this re-uses the EUID from the EUBWOID. | MUST |
| `holderIdentifierType`             | `holderIdentifierType` | string | Conditional future enhancement for holder identifier type. | MAY |
| `legalName`                        | `legalName` | string | Optional official name of the holder. | MAY |
| `memberOf`                         | `memberOf` | string | DSI or dataspace membership value. See code list 2.8. | MUST |
| `roles`                            | `roles` | array of strings | Non-empty array of role values. See code list 2.8. | MUST |
| `conformanceTo`                    | `conformanceTo` | JSON object | Dataspace governance rulebook information. See Section 2.2.1. | MAY |
| `conformanceTo.governanceRulebook` | `conformanceTo.governanceRulebook` | string (URI) | Reference to the online dataspace governance rulebook. | MAY |
| `conformanceTo.rulebookVersion`    | `conformanceTo.rulebookVersion` | string | Version of the online rulebook at the time of acceptance. | MAY |
| `conformanceTo.rulebookHash`       | `conformanceTo.rulebookHash` | string (SHA-256 hash) | SHA-256 hash of the rulebook, represented as 64 hexadecimal characters. | MAY |
| `conformanceTo.rulebookAcceptedAt` | `conformanceTo.rulebookAcceptedAt` | string (date-time) | Datetime when the rulebook was accepted, where present. | MAY |
| `onboardedBy`                      | `onboardedBy` | JSON object | Partner or service provider that onboarded the holder. See Section 2.2.2. | MAY |
| `onboardedBy.id`                   | `onboardedBy.id` | string (DID) | DID identifying the partner that onboarded the holder. | MAY |
| `onboardedBy.holderIdentifier`     | `onboardedBy.holderIdentifier` | string | Stable holder identifier used within the onboarding DSI or dataspace, where different from the top-level `holderIdentifier`. | MAY |
| `onboardedBy.holderIdentifierType` | `onboardedBy.holderIdentifierType` | string | Identifier type for `onboardedBy.holderIdentifier`. | MAY |
| `onboardedBy.platform`             | `onboardedBy.platform` | string | Commercial name of the DSI or partner that onboarded the holder. | MAY |
| `onboardedBy.organisation`         | `onboardedBy.organisation` | string | Legal name of the organisation that onboarded the holder and hosts the platform. | MAY |

In the Disclosable column, `MUST` means the issuer SHALL make the claim selectively disclosable;
`MAY` means the issuer MAY make it selectively disclosable; and `MUST NOT` means the claim SHALL
remain in the signed payload.

### 3.2.3 Status Claim

The Membership Credential is revocable (Chapter 6). Therefore, an SD-JWT VC-compliant
Membership Credential SHALL include a `status` claim unless a future profile explicitly
defines a short-lived, non-revocable variant.

The `status` claim SHALL use the status-list mechanism used by the other SD-JWT VC rulebooks in
this catalog. It SHALL be a JSON object with the following members:

* `type` (string): SHALL be `"status-list"`.
* `status_list_credential` (string, URI): URI of the Status List Credential that contains the
  status bitstring.
* `status_list_index` (integer, >= 0): zero-based index into the status list bitstring for this
  credential.
* `status_purpose` (string): SHALL be `"revocation"` for this attestation.

Example:

```json
{
  "status": {
    "type": "status-list",
    "status_list_credential": "https://djustconnect.be/status/membership/1",
    "status_list_index": 42,
    "status_purpose": "revocation"
  }
}
```

### 3.2.4 Example Payload

The following non-normative example shows the JWT claim set before SD-JWT processing.

```json
{
  "iss": "https://djustconnect.be",
  "iat": 1782205200,
  "nbf": 1782205200,
  "exp": 1813741200,
  "jti": "urn:uuid:8d6f0e3c-1c2a-4e2b-9f1a-1234567890ab",
  "vct": "eu.we-build.ds-membership.1",
  "attestation_legal_category": "EAA",
  "id": "did:web:example.com:participant:123",
  "holderIdentifier": "BEEUID0123456789",
  "legalName": "Farm Example BV",
  "memberOf": "Agri-X",
  "roles": ["DataRightsHolder", "DataProvider"],
  "conformanceTo": {
    "governanceRulebook": "https://agri-x.eu/rulebook",
    "rulebookVersion": "1.2",
    "rulebookHash": "9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08",
    "rulebookAcceptedAt": "2026-06-20T14:30:00Z"
  },
  "onboardedBy": {
    "id": "did:web:djustconnect.be",
    "holderIdentifierType": "VAT-ID",
    "holderIdentifier": "BE0123456789",
    "platform": "DjustConnect",
    "organisation": "ILVO"
  },
  "status": {
    "type": "status-list",
    "status_list_credential": "https://djustconnect.be/status/membership/1",
    "status_list_index": 42,
    "status_purpose": "revocation"
  },
  "cnf": {
    "jwk": {
      "kty": "EC",
      "crv": "P-256",
      "x": "...",
      "y": "..."
    }
  }
}
```

The SD-JWT VC JSON Schema and sample payload are published at:

* `data-schemas/sd-jwt/ds-membership-sd-jwt.json`
* `data-schemas/sd-jwt/sample-data/ds-membership-sd-jwt-sample.json`

## 3.3 W3C Verifiable Credentials Data Model-based encoding

**This is the primary format for the MVP.** The Membership Credential is a non-qualified EAA, which
is the only legal category permitted to use the W3C VCDM format (ARB_01a). W3C VCDM (JSON-LD) is
chosen for semantic interoperability with the dataspaces domain and because it supports selective
disclosure via ZKP.

The credential subject carries the attributes defined in Chapter 2. Standard VCDM metadata is used
for the credential envelope: `issuer`, `validFrom`, `validUntil`, `credentialStatus`, and the
credential `id` (unique per credential).

| **Data Identifier** | **VCDM location** | **Encoding** | **Optionality** |
|---------------------|-------------------|--------------|-----------------|
| `attestation_legal_category` | `credentialSubject.attestation_legal_category` | string (`EAA`) | M |
| `id` | `credentialSubject.id` | string (DID) | M |
| `holderIdentifier` | `credentialSubject.holderIdentifier` | string (EUID) | M |
| `memberOf` | `credentialSubject.memberOf` | string | M |
| `roles` | `credentialSubject.roles` | array of strings | M |
| `conformanceTo` | `credentialSubject.conformanceTo` | object | M |
| `onboardedBy` | `credentialSubject.onboardedBy` | object | M |
| `legalName` | `credentialSubject.legalName` | string | O |

*Attribute requests and selective disclosure mechanisms SHALL follow EU-approved specifications for
the W3C VCDM presentation/disclosure (ARB_04). [REFERENCE TO BE ADDED once the agreed WE BUILD /
EUDI specification document is available.]*

**Illustrative example (informative):**

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://w3id.org/ebwv/v0.1"
  ],
  "type": ["VerifiableCredential", "Membership"],
  "id": "urn:uuid:8d6f0e3c-1c2a-4e2b-9f1a-1234567890ab",
  "issuer": "did:web:djustconnect.be",
  "validFrom": "2026-06-23T09:00:00Z",
  "validUntil": "2027-06-23T09:00:00Z",
  "attestationLegalCategory": "EAA",
  "credentialStatus": {
    "id": "https://djustconnect.be/status/membership#42",
    "type": "BitstringStatusListEntry"
  },
  "credentialSubject": {
    "id": "urn:uuid:650805cd-8abf-4f2d-bc23-9552511c7e01",
    "member": {
      "type": "EconomicOperator",
      "legalIdentifier":"BEEUID0123456789",
      "legalName": "Farm Example BV"
    },
    "memberOf": "Agri-X",
    "role": ["DataRightsHolder", "DataProvider"],
    "termsOfUse": {
      "type":"GovernanceRulebook",
      "url": "https://agri-x.eu/rulebook",
      "version": "1.2",
      "hash": "9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08",
      "acceptedAt": "2026-06-20T14:30:00Z" 
    },
    "onboardedBy": {
      "type": "Platform",
      "platformId": "did:web:djustconnect.be",
      "name": "DjustConnect",
      "operator":{
        "type": "EconomicOperator",
        "legalName": "ILVO",
        "identifier": {
          "type": "VatId",
          "value": "BE0123456789"
        }
      }
    }
  }
}
```

*Proof type: an EU-approved Data Integrity proof / VC-JOSE-COSE securing mechanism SHALL be used.
[PROOF TYPE TO BE FIXED once the WE BUILD profile selects it; e.g. a Data Integrity ECDSA proof to
support ZKP-based selective disclosure.]*

## 4 Attestation usage

The Membership Credential is used in the WE BUILD SC2 "seamless onboarding" scenario. Its primary
use cases are:

* **Onboarding into a DSI or into Agri-X.** The holder presents the EUBWOID to identify their
  organisation, accepts the applicable dataspace governance rulebook, and receives a Membership
  Credential reflecting their membership and roles. Information already presented during a previous
  onboarding flow is trusted and not requested again.
* **Verifying membership and roles** during data-sharing transactions between participants of a DSI
  or dataspace.

**PID verification.** The holder is a legal person identified via the EUBWOID/EUID, not via a PID.
A Relying Party therefore does not need to request and verify a PID (ARB_27) for this attestation
in the MVP scenario.

**Relying Party obligations.** A Relying Party SHALL verify the issuer signature/proof, check
credential validity (`validFrom`/`validUntil`) and revocation status (Section 6), and confirm the
issuer is an authorised onboarding service provider for the relevant `memberOf` value (Section 5).
Where rulebook conformance matters, the Relying Party MAY compare `conformanceTo.rulebookHash` /
`rulebookVersion` against the expected rulebook.

**Presentation requirements.** Presentation is online. The primary format is W3C VCDM (JSON-LD)
presented via OpenID4VP; an SD-JWT VC flow may be piloted additionally. No proximity/offline
presentation is required.

**Device binding.**  The attestation is bound to an
organisation (legal person) held in an EU Business Wallet rather than to a natural person's device;
the MVP does not require cryptographic binding to a PID. The `cryptographically_bound_to` attribute
(ARB_28) is therefore not included. If a future scenario requires binding to the EUBWOID, add
`cryptographically_bound_to` as optional metadata in Section 2.6 with the corresponding attestation
type / `vct` value.

**Transactional data.** No transactional data (per [Topic 20]) is defined for this attestation; it
is not used for strong user authentication of electronic payments.

## 5 Trust anchors

**This Rulebook (non-qualified EAA):** the trust anchor is the public key of the issuing onboarding
service provider, resolvable from the issuer DID (`issuer` in the VCDM credential, e.g. a `did:web`
document). A Relying Party obtains the trust anchor by resolving that DID and verifies the
credential proof against it. Authorisation of an issuer to issue Membership Credentials for a given
`memberOf` value is governed by the WE BUILD / dataspace trust framework (WP4).

## 6 Revocation

The Membership Credential is **revocable**. Membership and the associated set of
roles share a single lifecycle: adding, changing, or removing a role requires re-issuance of the
credential and revocation of the superseded one. The credential carries a `credentialStatus` entry
(see the Section 3.3 example) used to publish status.


## 7 Compliance

The Membership Credential is defined as a **non-qualified EAA** under the [European Digital Identity
Regulation]:

* It includes an attribute indicating it is an EAA (`attestation_legal_category` = `EAA`,
  Section 2.1 / 2.2), per ARB_12.
* It carries attributes about the holder (`holderIdentifier`, `legalName`, `memberOf`, `roles`) per
  ARB_15 / ARB_17 (Annex V points b and c).
* The W3C VCDM (JSON-LD) format is used, which is permitted only for non-qualified EAA (ARB_01a).
* The trust-anchor location is provided via the issuer DID (Section 5), per ARB_21 / ARB_26.
* Revocation is addressed in Section 6, per [Topic 7].


## 8 References

| **Item Reference** | **Standard name/details**|
|--------------------|---------------------------|
| [European Business Wallet Vocabulary v0.1] | WE BUILD Semantics work group, European Business Wallet Vocabulary, version 0.1 |
| [European Digital Identity Regulation] | [Regulation (EU) 2024/1183](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=OJ:L_202401183) of the European Parliament and of the Council of 11 April 2024 amending Regulation (EU) No 910/2014 as regards establishing the European Digital Identity Framework |
| [HAIP] | Yasuda, K. *et al,* OpenID4VC High Assurance Interoperability Profile, OpenId Foundation, Version draft-03 |
| [IANA-JWT-Claims] | IANA JSON Web Token Claims Registry. Available: <https://www.iana.org/assignments/jwt/jwt.xhtml> |
| [ISO/IEC 18013-5] |  ISO/IEC 18013-5, Personal identification --- ISO-compliant driving licence - Part 5: Mobile driving licence (mDL) application, First edition, 2021-09 |
| [OIDC] | Sakimura, N. et al., "OpenID Connect Core 1.0", OpenID Foundation. Available: <https://openid.net/specs/openid-connect-core-1_0.html> |
| [RFC 3339] | RFC 3339  - Date and Time on the Internet: Timestamps, G. Klyne et al., July 2002 |
| [RFC 8610] | RFC 8610  - Concise Data Definition Language (CDDL): A Notational Convention to Express Concise Binary Object Representation (CBOR) and JSON Data Structures, H. Birkholz et al., June 2019 |
| [RFC 8943] | RFC 8943  - Concise Binary Object Representation (CBOR) Tags for Date, M. Jones et al., November 2020 |
| [RFC 8949] | RFC 8949 - Concise Binary Object Representation (CBOR), C. Bormann et al., December 2020 |
| [SD-JWT VC] |  SD-JWT-based Verifiable Credentials (SD-JWT VC). Available: <https://datatracker.ietf.org/doc/draft-ietf-oauth-sd-jwt-vc/>, version draft-ietf-oauth-sd-jwt-vc-09  |
| [Topic 7] | ARF Annex 2 - Topic 7 - Attestation revocation and revocation checking Available: <https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/latest/annexes/annex-2/annex-2-high-level-requirements/#a237-topic-7-attestation-revocation-and-revocation-checking>|
| [Topic 10] | ARF Annex 2 - Topic 10 - Issuing a PID or attestation to a Wallet Unit: <https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/latest/annexes/annex-2/annex-2-high-level-requirements/#a2310-topic-10-issuing-a-pid-or-attestation-to-a-wallet-unit>|
| [Topic 12] | ARF Annex 2 - Topic 12 - Attestation Rulebooks, Available: <https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/latest/annexes/annex-2/annex-2-high-level-requirements/#a2312-topic-12-attestation-rulebooks>|
| [Topic 20] | ARF Annex 2 - Strong User authentication for electronic payments, Available: <https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/latest/annexes/annex-2/annex-2-high-level-requirements/#a2320-topic-20-strong-user-authentication-for-electronic-payments>|
| [W3C VCDM v2.0] | Sporny, M. *et al,* Verifiable Credentials Data Model v2.0, W3C Recommendation.  |
