# WE BUILD Attestation Rulebook for attestations of type *Membership Credential*

*This WE BUILD v1 Rulebook specifies the Membership Credential used in the WE BUILD Supply Chain 2
(SC2) MVP scenario "seamless onboarding" piloted by the Data Sharing Initiatives (DSI) DjustConnect
(ILVO), DADS (ITC) and Tritom (Dataspace Europe), and the common agriculture dataspace "Agri-X".*

* Author(s):
    * Ingo Wolf, [Spherity GmbH](https://www.spherity.com)
    * Martin Domajnko, [Lutra Labs d.o.o.](https://lutralabs.io)

| Version | Date       | Description                                                                         |
|---------|------------|-------------------------------------------------------------------------------------|
| 01      | 25.06.2026 | Initial Rulebook derived from the MVP Membership credential attestation description. |
| 02      | 26.08.2026 | consolidated draft                                                                  |
| 03      | 26.08.2026 | BREAKING: `role`/`roles` values changed from bare strings to absolute URIs under `https://w3id.org/ebwv/roles#`; `memberOf` values changed from bare strings to absolute URIs minted per-DSI under the DSI's own domain. See Section 2.8. |
| 04      | 27.08.2026 | `onboardedBy` gains `memberIdentifier`, the platform-local identifier of the **holder** (Section 2.2.2/2.2.4), bridging the EBWOID/EUID to legacy identifiers; code list 2.8 corrected accordingly. Chapter 3 (SD-JWT VC and W3C VCDM) realigned with the Chapter 2 attribute names (`member`, `role`, `termsOfUse`) after drift; holder `legalName` folded into `member`. Disclosability rules made consistent (Section 3.2.2). Use of the credential in data-sharing transactions marked out of scope for the pilot. |
| 05      | 01.09.2026 | Editorial: "SC2" expanded to "Supply Chain 2 (SC2)" on first use; feedback channel now points to the Supply Chain 2 contact points. |

**Feedback:**
* Main feedback channel: [GitHub issues](https://github.com/webuild-consortium/eudi-wallet-rulebooks-and-schemas/issues)
* Alternative: contact Supply Chain 2 contact points in WE BUILD.

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
  during onboarding. Use during data-sharing transactions is a *potential* future application and
  is **out of scope for this pilot**: a data-transfer flow would likely require the Membership
  Credential to be transformed into a connector-native credential (for instance an EDC / dataspace
  protocol participant credential) rather than presented directly.
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
governance rulebook the member conforms to (`termsOfUse`), the roles the member has within the
DSI or dataspace (`role`), and the platform through which the member was onboarded
(`onboardedBy`).

**Logical model.** The Membership Credential is structured as follows:

* The credential subject is identified by `id` (a UUID, may change) and `member`, an object of type
  [EconomicOperator](https://w3id.org/ebwv#EconomicOperator) carrying the stable identifier of the
  holder; for the MVP this re-uses the EUID from the EUBWOID of that economic operator.
* `memberOf` names the DSI or dataspace the membership is for.
* `role` is an array of roles the holder has within that DSI or dataspace. The membership and its
  set of roles share the same lifecycle: a change of roles requires re-issuance of the credential.
* `termsOfUse` is an object of type [GovernanceRulebook](https://w3id.org/ebwv#GovernanceRulebook) referencing the accepted dataspace governance rulebook (URL,
  version, SHA-256 hash, and acceptance datetime).
* `onboardedBy` is an object of type [Platform](https://w3id.org/ebwv#Platform) carrying **all
  platform-specific information**: the platform's identifier (`platformId`) and commercial `name`,
  the `operator` (the economic operator hosting and running the platform), and `memberIdentifier`,
  the identifier by which that platform knows **the holder**.

Note that `onboardedBy.memberIdentifier` describes the holder, not the platform or its operator.
It exists to bridge the EU-level identifier in `member` to a legacy or platform-local identifier of
the same holder, which may not be recognisable or interoperable outside that platform. The name
`onboardedBy` refers to the onboarding event; the object describes both parties to it.

Standard credential metadata (issuer, issuance time, expiry, status) follows from the chosen VC
format and is documented in Chapter 3 rather than as attributes here. Each Membership Credential
has its own unique identifier.

**DS-specific terminology.** Attributes marked `(*)` below (`id`, `member`, `memberOf`) keep their
names for cross-dataspace interoperability (e.g. with Catena-X) and SHALL NOT be renamed. For
extensibility purposes the JSON Schema definition allows additional properties in the `onboardedBy`
data type definition.

*Subsections 2.2 - 2.7 define the attributes and metadata in an encoding-independent manner. Code
lists are in Section 2.8 and integrity rules in Section 2.9. The structured objects `termsOfUse`
and `onboardedBy` are defined as sub-tables in Section 2.2.*

### 2.2 Mandatory attributes of object [Membership](https://w3id.org/ebwv#Membership)

| **Data Identifier**          | **Semantic Reference**                                                     | **Definition**                                                                                                                                                                                                                                                                | **Data type**                                                  | **Example value**                     |
|------------------------------|----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|---------------------------------------|
| `attestation_legal_category` | [attestationLegalCategory](https://w3id.org/ebwv#attestationLegalCategory) | Indication that the attestation is a non-qualified EAA (per ARB_12).                                                                                                                                                                                                          | string                                                         | [EAA](https://w3id.org/ebwv#EAA)      |
| `id` (*)                     | @id                                                                        | UUID of the credential subject. May change, in contrast to `member.identifier` which is a persistent, stable identifier. Name kept for cross-dataspace interoperability.                                                                                                 | string (UUID)                                                  | `did:web:example.com:participant:123` |
| `member` (*)                 | [member](https://w3id.org/ebwv#member)                                     | The holder, as an economic operator carrying the stable identifier that uniquely identifies it. For the MVP this re-uses the EUID (part of the EUBWOID); scope is legal persons only. Name kept for cross-dataspace interoperability. Object, see table below.                 | [EconomicOperator](https://w3id.org/ebwv#EconomicOperator)     | *see 2.2.3*                           |
| `memberOf` (*)               | [memberOf](https://w3id.org/ebwv#memberOf)                                 | The DSI or dataspace the holder is a member of. Within a DSI/DS all issued membership credentials use the same value. Value is an absolute URI, minted and owned by that DSI/dataspace under its own domain. Name kept for cross-dataspace interoperability. See code list 2.8. | string (URI)                                                    | `https://agri-x.eu/id#Agri-X`         |
| `role`                       | [role](https://w3id.org/ebwv#role)                                         | Array of roles the holder has within the DSI or dataspace. A member may have multiple roles; the set of roles shares the membership lifecycle. Each value is an absolute URI under the `https://w3id.org/ebwv/roles#` namespace. See code list 2.8.                          | array of URIs (IRIs)                                            | `["https://w3id.org/ebwv/roles#DataProvider","https://w3id.org/ebwv/roles#DataConsumer"]` |
| `termsOfUse`                 | [termsOfUse](https://www.w3.org/2018/credentials/#termsOfUse)              | Dataspace governance rulebook information. Object, see table below.                                                                                                                                                                                                           | [GovernanceRulebook](https://w3id.org/ebwv#GovernanceRulebook) | *see 2.2.1*                           |
| `onboardedBy`                | [platform](https://w3id.org/ebwv#platform)                                 | The platform through which the holder was onboarded into the DSI or dataspace: the platform itself, the economic operator hosting it, and the identifier by which that platform knows the holder. Object, see table below.                                                    | [Platform](https://w3id.org/ebwv#Platform)                     | *see 2.2.2*                           |

#### 2.2.1 `termsOfUse` object of type [GovernanceRulebook](https://w3id.org/ebwv#GovernanceRulebook)

| **Data Identifier** | **Semantic Reference** | **Definition** | **Data type** | **Optionality** | **Example value** |
|---------------------|------------------------|----------------|---------------|-----------------|-------------------|
| `url`               | N/A                    | Reference to the online dataspace governance rulebook for the given DSI or DS. | string (URI) | M | `https://agri-x.eu/rulebook` |
| `version`           | N/A                    | Version of the online rulebook at the time of acceptance. | string | M | `1.2` |
| `hash`              | N/A                    | SHA-256 hash of the rulebook, for quick comparison. | string (SHA-256 hash) | M | `9f86d081...` |
| `acceptedAt`        | N/A                    | Datetime when the rulebook was accepted. May differ from issuance time in an outbound flow where a credential is issued to an existing member based on a previously completed onboarding flow. | datetime | O | `2026-06-01T10:00:00Z` |

#### 2.2.2 `onboardedBy` object of type [Platform](https://w3id.org/ebwv#Platform)

This object carries **all platform-specific information** about the onboarding event: which
platform performed the onboarding, which legal entity hosts that platform, and how that platform
identifies the holder locally. Example reading: *"DjustConnect, hosted by ILVO, onboarded farmer
XYZ, who was identified there by VAT-ID BE0123456789 during paper-flow onboarding."*

| **Data Identifier** | **Semantic Reference**                                     | **Definition**                                                                                                                                                                                                                                                                                                                                     | **Data type**                                              | **Optionality** | **Example value**         |
|---------------------|------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|-----------------|---------------------------|
| `platformId`        | N/A                                                        | DID identifying the platform through which the holder was onboarded.                                                                                                                                                                                                                                                                               | string (DID)                                               | M               | `did:web:djustconnect.be` |
| `name`              | N/A                                                        | Commercial name of the platform (DSI or onboarding partner).                                                                                                                                                                                                                                                                                       | string                                                     | M               | `DjustConnect`            |
| `operator`          | [EconomicOperator](https://w3id.org/ebwv#EconomicOperator) | The economic operator hosting and running the platform. Object, see 2.2.3.                                                                                                                                                                                                                                                                         | object                                                     | M               | *see 2.2.3*               |
| `memberIdentifier`  | [identifier](https://w3id.org/ebwv#identifier)             | The identifier by which **the holder** — the credential subject, not the platform or its operator — is known within this platform. Bridges the EU-level identifier in `member` to a legacy or platform-local identifier of the same holder, which may not be recognisable or interoperable outside that platform. Object, see 2.2.4. See IR-03, IR-04. | object                                                     | O               | *see 2.2.4*               |

#### 2.2.3 [EconomicOperator](https://w3id.org/ebwv#EconomicOperator) object

Used by `member` (the holder) and by `onboardedBy.operator` (the organisation hosting the
onboarding platform).

| **Data Identifier** | **Semantic Reference**                         | **Definition**                                                                                                                                        | **Data type** | **Optionality** | **Example value**                              |
|---------------------|------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-----------------|------------------------------------------------|
| `legalName`         | N/A                                            | Official legal name of the economic operator. For `member`, see integrity rule IR-02.                                                                 | string        | M               | `Farm Example BV`                              |
| `identifier`        | [identifier](https://w3id.org/ebwv#identifier) | The identifier of the economic operator. For `member` this is the stable, EU-level identifier: for the MVP the EUID taken from the holder's EUBWOID.   | object        | M               | `{"type":"EUID","value":"BEEUID0123456789"}`   |

#### 2.2.4 Identifier object

Used by `member.identifier`, `onboardedBy.operator.identifier` and
`onboardedBy.memberIdentifier`.

| **Data Identifier** | **Semantic Reference** | **Definition**                                                                                                                                          | **Data type** | **Optionality** | **Example value** |
|---------------------|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-----------------|-------------------|
| `type`              | N/A                    | Identifier scheme. See code list 2.8. Not strictly enumerated: platform-specific and member-state-specific schemes are expected and permitted.           | string        | M               | `VAT-ID`          |
| `value`             | N/A                    | The identifier value, in the scheme given by `type`.                                                                                                    | string        | M               | `BE0123456789`    |

### 2.3 Optional attributes

*No optional attributes are defined at the top level of the credential. The holder's official name
is carried by `member.legalName` (Section 2.2.3) and the holder's platform-local identifier by
`onboardedBy.memberIdentifier` (Section 2.2.2).*

| **Data Identifier** | **Semantic Reference** | **Definition** | **Data type** | **Example value** |
|------------------------|--------------------------|--------------|--------------|--------------|
| N/A | N/A | N/A | N/A | N/A |

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

**`memberOf`** — fixes the DSI or dataspace the holder is a member of. The value is an absolute URI,
decided and minted by the governance level of the DSI or dataspace itself, under that DSI's own
domain (unlike `role`, which is minted centrally by the WE BUILD Semantics work group). A Relying
Party matches `memberOf` by exact URI, since each value identifies a distinct DSI/dataspace rather
than a shared, hierarchical vocabulary.

| **Field name** | **Allowed values (IRI)** | **Meaning** | **Source / vocabulary** | **Notes / extensibility** |
|----------------|--------------------|-------------|--------------------------|---------------------------|
| `memberOf` | `https://agri-x.eu/id#Agri-X` | The common agriculture dataspace (federation level). | WE BUILD SC2 use case | Additional values are minted by DSI/DS governance under their own domain. |
| `memberOf` | `https://[DADS-DOMAIN]/id#DADS` | DSI operated by ITC. | WE BUILD SC2 use case | Domain not yet published in this catalog; to be confirmed by ITC. |
| `memberOf` | `https://djustconnect.be/id#DjustConnect` | DSI operated by ILVO. | WE BUILD SC2 use case | |
| `memberOf` | `https://[TRITOM-DOMAIN]/id#Tritom` | DSI operated by DataSpace Europe Oy. | WE BUILD SC2 use case | Domain not yet published in this catalog; to be confirmed by DataSpace Europe Oy. |

**`role`** — roles a member can have. A member may have multiple roles in one credential. Based on
the common roles agreed across the piloting DSIs, treating Agri-X as a federation of DSIs. Role
values are absolute URIs under the `https://w3id.org/ebwv/roles#` namespace, owned and published by
the WE BUILD Semantics work group. New roles MAY be minted under this namespace without a rulebook
or schema change; a Relying Party MAY trust roles by namespace prefix instead of enumerating each
value. This list is not exhaustive; other DSIs may map their own role names to these.

| **Field name** | **Allowed values (IRI)** | **Meaning** | **Source / vocabulary** | **Notes / extensibility** |
|-----------|--------------------|-------------|--------------------------|---------------------------|
| `role` | `https://w3id.org/ebwv/roles#DataRightsHolder` | Owns the rights to the data (e.g. farmers) and can consent to data being shared from a data provider to a data consumer. | WE BUILD SC2 use case | Non-exhaustive; mappable to DSI-specific names |
| `role` | `https://w3id.org/ebwv/roles#DataProvider` | Partner that enables sharing data, e.g. via an API. | WE BUILD SC2 use case | |
| `role` | `https://w3id.org/ebwv/roles#DataConsumer` | Partner that wants to use data, e.g. by calling an API. | WE BUILD SC2 use case | |
| `role` | `https://w3id.org/ebwv/roles#Operator` | Partner providing DSSC building-block services (identity management, consent management, logging, …). | WE BUILD SC2 use case (custom role part of DADS) | |
| `role` | `https://w3id.org/ebwv/roles#OnboardingServiceProvider` | Partner that onboards members into Agri-X; may be a DSI or another technology partner. | WE BUILD SC2 use case (role part of Catena-X) | |

**`identifier.type`** — defines how the accompanying `identifier.value` is to be interpreted. The
same code list applies wherever an Identifier object (Section 2.2.4) appears:
`member.identifier.type`, `onboardedBy.operator.identifier.type` and
`onboardedBy.memberIdentifier.type`. Most values refer to identifiers assigned by Business
Registries. Given the open-ended range of member-state and domain-specific identifiers, this list
is indicative and SHALL NOT be enforced as a closed enumeration.

| **Field name** | **Allowed values** | **Meaning** | **Source / vocabulary** | **Notes / extensibility** |
|-----------------------------------------|--------------------|-------------|--------------------------|---------------------------|
| `identifier.type` | `EUID` | The unique identifier attributed in accordance with Article 9 of EBW, taken from the holder's EUBWOID. The expected value of `member.identifier.type` in the MVP. | [European Business Wallet Vocabulary v0.1], `rb-ebwoid` | |
| `identifier.type` | `VAT-ID` | VAT identifier. Used by DjustConnect and Tritom to identify holders locally. | [European Business Wallet Vocabulary v0.1] | |
| `identifier.type` | `KMG-MID` | Unique farm id used in Slovenia, issued by the Ministry of Agriculture. Used by the DADS platform to identify holders locally. | National (SI) | |
| `identifier.type` | `PlatformSpecific` | Custom identifier whose meaning is known only to the issuing platform and is not interoperable at EU level. | DSI-specific | Non-enforced; extensible per DSI / member state |

Note that `EUID` is expected for `member.identifier` (the EU-level identity of the holder), whereas
`VAT-ID`, `KMG-MID` and `PlatformSpecific` are the values expected for
`onboardedBy.memberIdentifier` (the same holder as known locally by the onboarding platform). This
is the bridging mechanism between EBWOID identifiers and legacy platform identifiers; see IR-03 and
IR-04.

A common code list distinguishing Legal Person from Natural Person identifiers is being prepared by
the WE BUILD Semantics work group. It is not required for this version: the MVP scenario pilots
legal persons only. Values will be added once provided.

### 2.9 Integrity rules

| **Rule ID** | **Rule statement**                                                                                                                  | **Why it exists** | **Where enforced** | **Verifier / issuer behavior on failure** |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------|-------------------|--------------------|-------------------------------------------|
| `IR-01` | If `termsOfUse.acceptedAt` is present, its value SHALL be a datetime less than or equal to the issuance datetime of the credential. | Acceptance of the rulebook cannot logically occur after the credential was issued; supports outbound flows where acceptance happened during an earlier onboarding. | Issuer business rules, schema validation, and verifier business validation. | Issuer SHALL reject the value; verifier SHALL treat `termsOfUse` as invalid. |
| `IR-02` | If the member is a Legal Person, `member.legalName` SHOULD carry the official name as registered.                                   | Improves readability and identification of legal-person holders. | Issuer business rules. | Issuer SHOULD populate `member.legalName`; verifier MAY warn if absent. |
| `IR-03` | If `onboardedBy.memberIdentifier` is present, it SHALL identify the same legal person as `member`.                                  | It is a bridge to the same holder under a platform-local scheme, not a second subject. Confusing it with the platform operator's own identifier would misidentify the holder. | Issuer business rules and verifier business validation. | Issuer SHALL NOT issue the credential; verifier SHALL treat the credential as inconsistent. |
| `IR-04` | A Relying Party SHALL NOT treat `onboardedBy.memberIdentifier` as an authoritative EU-level identifier of the holder. `member.identifier` is authoritative. | The value may be a legacy or platform-local identifier with no recognition or interoperability outside the issuing platform. | Verifier business validation. | Verifier MAY use the value only to reconcile the holder against its own legacy records, never as the basis of identification. |

# 3 Attestation encoding

## 3.1 ISO/IEC 18013-5-compliant encoding

*Not applicable for this version.* The Membership Credential is used in online onboarding flows;
no proximity / offline presentation requirement applies (ARB_02). The primary
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

The following private claims map to the attributes defined in Chapter 2. Claim names are identical
to the Chapter 2 data identifiers; the two SHALL be kept in step.

| **Data Identifier**                       | **Attribute identifier** | **Encoding format** | **Reference / Notes** | **Optionality** | **Disclosable** |
|-------------------------------------------|--------------------------|---------------------|-----------------------|-----------------|-----------------|
| `attestationLegalCategory`                | `attestation_legal_category` | string | SHALL be `EAA`. | M | MUST NOT |
| `id`                                      | `id` | string (DID) | DID of the credential subject. | M | MUST |
| `member`                                  | `member` | JSON object | The holder as an economic operator. See Section 2.2.3. | M | MUST |
| `member.legalName`                        | `member.legalName` | string | Official name of the holder. See IR-02. | M | MUST |
| `member.identifier`                       | `member.identifier` | JSON object | Stable, EU-level identifier of the holder. See Section 2.2.4. | M | MUST |
| `member.identifier.type`                  | `member.identifier.type` | string | Identifier scheme; `EUID` in the MVP. See code list 2.8. | M | MUST |
| `member.identifier.value`                 | `member.identifier.value` | string | Identifier value. For the MVP this re-uses the EUID from the EUBWOID. | M | MUST |
| `memberOf`                                | `memberOf` | string (absolute URI, minted by the owning DSI/dataspace) | DSI or dataspace membership value. See code list 2.8. | M | MUST |
| `role`                                    | `role` | array of strings (absolute URI, `https://w3id.org/ebwv/roles#` namespace) | Non-empty array of role values. See code list 2.8. | M | MUST (per element) |
| `termsOfUse`                              | `termsOfUse` | JSON object | Dataspace governance rulebook information. See Section 2.2.1. | M | MUST |
| `termsOfUse.url`                          | `termsOfUse.url` | string (URI) | Reference to the online dataspace governance rulebook. | M | MUST |
| `termsOfUse.version`                      | `termsOfUse.version` | string | Version of the online rulebook at the time of acceptance. | M | MUST |
| `termsOfUse.hash`                         | `termsOfUse.hash` | string (SHA-256 hash) | SHA-256 hash of the rulebook, represented as 64 hexadecimal characters. | M | MUST |
| `termsOfUse.acceptedAt`                   | `termsOfUse.acceptedAt` | string (date-time) | Datetime when the rulebook was accepted, where present. See IR-01. | O | MUST |
| `onboardedBy`                             | `onboardedBy` | JSON object | The platform through which the holder was onboarded. See Section 2.2.2. | M | MUST |
| `onboardedBy.platformId`                  | `onboardedBy.platformId` | string (DID) | DID identifying the onboarding platform. | M | MUST |
| `onboardedBy.name`                        | `onboardedBy.name` | string | Commercial name of the onboarding platform. | M | MUST |
| `onboardedBy.operator`                    | `onboardedBy.operator` | JSON object | Economic operator hosting the platform. See Section 2.2.3. | M | MUST |
| `onboardedBy.operator.legalName`          | `onboardedBy.operator.legalName` | string | Legal name of the organisation hosting the platform. | M | MUST |
| `onboardedBy.operator.identifier`         | `onboardedBy.operator.identifier` | JSON object | Identifier of that organisation. See Section 2.2.4. | M | MUST |
| `onboardedBy.memberIdentifier`            | `onboardedBy.memberIdentifier` | JSON object | Identifier by which the platform knows **the holder**. See Section 2.2.4 and IR-03, IR-04. | O | MUST |
| `onboardedBy.memberIdentifier.type`       | `onboardedBy.memberIdentifier.type` | string | Identifier scheme, e.g. `VAT-ID`, `KMG-MID`, `PlatformSpecific`. See code list 2.8. | O | MUST |
| `onboardedBy.memberIdentifier.value`      | `onboardedBy.memberIdentifier.value` | string | Platform-local identifier value for the holder. | O | MUST |

**Reading the Disclosable column.** `MUST` means the issuer SHALL make the claim selectively
disclosable; `MAY` means the issuer MAY do so; `MUST NOT` means the claim SHALL remain in the
always-visible signed payload.

Disclosability is independent of whether an attribute is mandatory in the credential. Optionality
governs what the *issuer* SHALL include when issuing; disclosability governs what the *holder* may
withhold when presenting. All subject attributes are therefore selectively disclosable, including
the mandatory ones: a Relying Party enforces the set it needs through its presentation request and
rejects a presentation that omits any of them, so disclosability costs the Relying Party nothing
while leaving the holder in control of everything beyond that set. The claims marked `MUST NOT` are
exactly those required to process or validate the credential itself, which no presentation can
omit.

**Granularity for structured attributes.** Where an attribute is an object or an array, the
following applies:

* Members of `termsOfUse`, `member`, `onboardedBy` and their nested objects SHALL be individually
  disclosable (structured/recursive selective disclosure), so that a holder can present, for
  example, `termsOfUse.hash` and `termsOfUse.version` without disclosing
  `termsOfUse.acceptedAt`, which is onboarding-date metadata a Relying Party rarely needs.
* Elements of `role` SHALL be individually disclosable, so that a holder with several roles can
  present only the role or roles the Relying Party's policy requires.

Note that the granularity, not the requirement, is what depends on the data type: `MUST` applies
uniformly to `memberOf`, `role` and `termsOfUse` alike, since all three are part of the atomic set
a Relying Party uses to decide whether to accept the credential. The distinction between a URI, an
array of URIs and an object determines only *how* selective disclosure is applied to each.

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
  "member": {
    "legalName": "Farm Example BV",
    "identifier": {
      "type": "EUID",
      "value": "BEEUID0123456789"
    }
  },
  "memberOf": "https://agri-x.eu/id#Agri-X",
  "role": ["https://w3id.org/ebwv/roles#DataRightsHolder", "https://w3id.org/ebwv/roles#DataProvider"],
  "termsOfUse": {
    "url": "https://agri-x.eu/rulebook",
    "version": "1.2",
    "hash": "9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08",
    "acceptedAt": "2026-06-20T14:30:00Z"
  },
  "onboardedBy": {
    "platformId": "did:web:djustconnect.be",
    "name": "DjustConnect",
    "operator": {
      "legalName": "Instituut voor Landbouw-, Visserij- en Voedingsonderzoek (ILVO)",
      "identifier": {
        "type": "VAT-ID",
        "value": "BE0848278827"
      }
    },
    "memberIdentifier": {
      "type": "VAT-ID",
      "value": "BE0123456789"
    }
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

Note the two distinct VAT identifiers in the `onboardedBy` object:
`onboardedBy.operator.identifier` is the VAT-ID of ILVO, the organisation hosting the DjustConnect
platform, whereas `onboardedBy.memberIdentifier` is the VAT-ID by which DjustConnect knows the
holder, Farm Example BV — the same legal person that `member` identifies at EU level by its EUID.

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
| `attestationLegalCategory` | `attestationLegalCategory` (credential level) | string (`EAA`) | M |
| `id` | `credentialSubject.id` | string (URI/DID) | M |
| `member` | `credentialSubject.member` | object, `type` `EconomicOperator` | M |
| `member.legalName` | `credentialSubject.member.legalName` | string | M |
| `member.identifier` | `credentialSubject.member.identifier` | object (`type`, `value`) | M |
| `memberOf` | `credentialSubject.memberOf` | string (URI, `@id`-typed in `@context`) | M |
| `role` | `credentialSubject.role` | array of URIs (`@id`-typed in `@context`, `https://w3id.org/ebwv/roles#` namespace) | M |
| `termsOfUse` | `credentialSubject.termsOfUse` | object, `type` `GovernanceRulebook` | M |
| `onboardedBy` | `credentialSubject.onboardedBy` | object, `type` `Platform` | M |
| `onboardedBy.platformId` | `credentialSubject.onboardedBy.platformId` | string (DID) | M |
| `onboardedBy.name` | `credentialSubject.onboardedBy.name` | string | M |
| `onboardedBy.operator` | `credentialSubject.onboardedBy.operator` | object, `type` `EconomicOperator` | M |
| `onboardedBy.memberIdentifier` | `credentialSubject.onboardedBy.memberIdentifier` | object (`type`, `value`) | O |

The `onboardedBy.memberIdentifier` property carries the platform-local identifier of the **holder**
(Section 2.2.2). It is optional, but where a platform holds such an identifier the issuer SHOULD
include it: it is what allows a Relying Party operating a legacy system to reconcile the holder
against its existing records when the platform's identifier is not recognisable or interoperable at
EU level. It SHALL NOT be confused with `onboardedBy.operator.identifier`, which identifies the
organisation hosting the platform. See IR-03 and IR-04.

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
      "legalName": "Farm Example BV",
      "identifier": {
        "type": "EUID",
        "value": "BEEUID0123456789"
      }
    },
    "memberOf": "https://agri-x.eu/id#Agri-X",
    "role": ["https://w3id.org/ebwv/roles#DataRightsHolder", "https://w3id.org/ebwv/roles#DataProvider"],
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
      "operator": {
        "type": "EconomicOperator",
        "legalName": "Instituut voor Landbouw-, Visserij- en Voedingsonderzoek (ILVO)",
        "identifier": {
          "type": "VAT-ID",
          "value": "BE0848278827"
        }
      },
      "memberIdentifier": {
        "type": "VAT-ID",
        "value": "BE0123456789"
      }
    }
  }
}
```

This example reads: *"DjustConnect, a platform hosted by ILVO, onboarded Farm Example BV — known to
DjustConnect by VAT-ID BE0123456789 and identified at EU level by EUID BEEUID0123456789 — into
Agri-X as a Data Rights Holder and Data Provider."* The VAT-ID under `operator.identifier` belongs
to ILVO; the VAT-ID under `memberIdentifier` belongs to the holder.

*Proof type: an EU-approved Data Integrity proof / VC-JOSE-COSE securing mechanism SHALL be used.
[PROOF TYPE TO BE FIXED once the WE BUILD profile selects it; e.g. a Data Integrity ECDSA proof to
support ZKP-based selective disclosure.]*

## 4 Attestation usage

The Membership Credential is used in the WE BUILD SC2 "seamless onboarding" scenario. Its use case
in this pilot is:

* **Onboarding into a DSI or into Agri-X.** The holder presents the EUBWOID to identify their
  organisation, accepts the applicable dataspace governance rulebook, and receives a Membership
  Credential reflecting their membership and roles. Information already presented during a previous
  onboarding flow is trusted and not requested again. Where the onboarding platform holds a
  platform-local identifier for the holder, it is recorded in `onboardedBy.memberIdentifier`, so
  that a later Relying Party can reconcile the holder with its own legacy records.

**Potential future use, out of scope for this pilot.** Verifying membership and roles during
data-sharing transactions between participants of a DSI or dataspace is a plausible further
application, but it is not piloted in the MVP and is not specified by this Rulebook. Direct
presentation of the Membership Credential inside a data-transfer flow is not assumed: such a flow
would most likely require the credential to be transformed into a connector-native credential (for
instance an EDC / dataspace protocol participant credential) first. Any statement about
data-sharing use elsewhere in this document is to be read in that light.

**PID verification.** The holder is a legal person identified via the EUBWOID/EUID, not via a PID.
A Relying Party therefore does not need to request and verify a PID (ARB_27) for this attestation
in the MVP scenario.

**Relying Party obligations.** A Relying Party SHALL verify the issuer signature/proof, check
credential validity (`validFrom`/`validUntil`) and revocation status (Section 6), and confirm the
issuer is an authorised onboarding service provider for the relevant `memberOf` value (Section 5).
Where rulebook conformance matters, the Relying Party MAY compare `termsOfUse.hash` /
`termsOfUse.version` against the expected rulebook. A Relying Party MAY additionally use
`onboardedBy.memberIdentifier` to reconcile the holder with its own legacy records, subject to
IR-04.

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

**memberOf vocabulary governance.** `memberOf` values are absolute URIs minted by the owning
DSI/dataspace under its own domain (e.g. `https://agri-x.eu/id#Agri-X`,
`https://djustconnect.be/id#DjustConnect`), not by a shared central registry. A Relying Party SHALL
match `memberOf` by exact URI and confirm WP4 authorization for that specific URI; unlike `role`,
prefix-based trust does not apply since each `memberOf` value identifies a distinct DSI/dataspace
rather than a term in a shared vocabulary.

**Role vocabulary governance.** `role` values are minted and published under the
`https://w3id.org/ebwv/roles#` namespace by the WE BUILD Semantics work group, independent of this
Rulebook's own versioning (Section 2.8). A Relying Party MAY trust roles by namespace prefix rather
than an exact-match list, so that new roles published under this namespace do not require an RP
policy change. Role values outside this namespace (DSI-specific extensions) SHALL be governed by
that DSI's own trust framework.

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
* It carries attributes about the holder (`member.identifier`, `member.legalName`, `memberOf`,
  `role`) per ARB_15 / ARB_17 (Annex V points b and c).
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
