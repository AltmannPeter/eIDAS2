# Requirements for hardware protected revocation keys

## Background 

The work in TS14 has raised questions of whether or not the signing keys for validity status objects (e.g., a revocation list or a validity status token) has to be hardware protected. This text investigates the topic by looking into the following sources:

1. The [CIR 2015/1502](https://eur-lex.europa.eu/eli/reg_impl/2015/1502/oj/eng) with its associated [guidance](https://docs.google.com/document/d/153aFq40pzHETYkz12sJeDTczRDvECiDO/edit?usp=sharing&ouid=113683449039040581284&rtpof=true&sd=true)
2. [ETSI EN 319 411-1](https://www.etsi.org/deliver/etsi_en/319400_319499/31941101/01.04.01_60/en_31941101v010401p.pdf)
3. The [CIR 2025/2160](https://eur-lex.europa.eu/eli/reg_impl/2025/2160/oj)

## What an Assurance Level is

Let us look at definitions for Assurance Level, or how the concept is treated, in various sources.

> LoA describes the degree of confidence in the processes leading up to and including the authentication process itself, thus providing assurance that the entity that uses a particular identity is in fact the entity to which that identity was assigned. - ISO/IEC 29115

> Assurance levels should characterise the degree of confidence in electronic identification means in establishing the identity of a person, thus providing assurance that the person claiming a particular identity is in fact the person to which that identity was assigned. - Regulation (EU) No 910/2014

> The [eID] means is designed so that it: 1) can be assumed to be used only if under the control or possession of the person to whom it belongs, 2) can be reliably protected by the person to whom it belongs against use by others. - CIR 2015/1502 Annex Section 2.2.1

The Guidance document further clarifies what *reliably protected* means. 

> 'reliably protected' refers to the efforts taken to prevent the electronic identification means from being used without the subject's knowledge and active consent.

## Validity Status Checks

There are statements about misuse and alteration

> [...] characterised with reference to technical specifications, standards and procedures related thereto, including technical controls, the purpose of which is to prevent misuse or alteration of the identity. - Regulation (EU) No 910/2014 Article 8 2§

Notably, Regulation (EU) No 910/2014 Article 8 seemingly does not include validity status checks in the specifications, standards, and procedures related to assurance levels.

Authentication is not validity status checking:

> the authentication mechanism, through which the natural or legal person uses the electronic identification means to confirm its identity to a relying party; - Regulation (EU) No 910/2014 Article 8 3§

> The release of person identification data is preceded by reliable verification of the electronic identification means and its validity. - CIR 2015/1502 Annex Section 2.3.1

## CIR 2015/1502 and its Guidance

The Annex Section 2 covers technical specifications and procedures. Section 2.2 covers means management, and section 2.2.3 is the only one specifically addressing validity status controls. The following three points are mentioned:

1. It is possible to suspend and/or revoke an electronic identification means in a timely and effective manner.
2. The existence of measures taken to prevent unauthorised suspension, revocation and/or reactivation.
3. Reactivation shall take place only if the same assurance requirements as established before the suspension or revocation continue to be met.

Notably, the requirements are the same across all three assurance levels, meaning that both substantial and high has the same requirements as low.

Point two requires the *existance of measures* that prevent unauthorized validity status state changes. But it does not specify what these measures are. The Guidance document additionally clarifies that:

> This will generally imply authentication of the requester's authorisation to act in that way. It should be determined who can authorise suspension and/or revocation besides the user – for example relevant public authorities.

Note that this does not mention anything in particular related to how signing keys for revocation objects are protected.

Validity status checks are also mentioned in section 2.3.1. on the Authentication mechanism.

> The release of person identification data is preceded by reliable verification of the electronic identification means and its validity through a dynamic authentication.

The validity here is specifically mentioning dynamic authentication. This is defined as:

> an electronic process using cryptography or other techniques to provide a means of creating on demand an electronic proof that the subject is in control or in possession of the identification data and which changes with each authentication between the subject and the system verifying the subject's identity;

Note how this does not mention the validity status of the attestation. Arguably, it focuses on the validity of the means itself by requesting proof that the identity subject is in control. The Guidance document sheds further light on the definition:

> The primary purpose of dynamic authentication is to mitigate against attacks such as ‘man-in-the-middle’ or misusing verification data from a previously recorded authentication replay to the verifier.

As before, it is not the validity status of the attestation that is in question.

Security controls for the authentication mechanism has a particular focus.

> The authentication mechanism implements security controls for the verification of the electronic identification means, so that it is highly unlikely that activities such as guessing, eavesdropping, replay or manipulation of communication by an attacker with [enhanced-basic/substantial/high] attack potential can subvert the authentication mechanisms.

None of this applies to validity status checks.

With regards to hardware requirements, section 2.4.6. clarifies the needs for technical controls. At assurance level substantial and high, the following is stated:

> Sensitive cryptographic material, if used for issuing electronic identification means and authentication is protected from tampering

Note that only issuance and authentication are mentioned, and these are distinct from validity status checks. Instead, validity status checks could be interpreted as being covered by assurance level low:

> The existence of proportionate technical controls to manage the risks posed to the security of the services, protecting the confidentiality, integrity and availability of the information processed.

If validity status checking is a service, then all that is mentioned is that *proportionate* technical controls are in place. The Guidance document does not include any mention of validity status checking as a service under its guidance for section 2.4.6. However, it does mention the following:

> Sensitive cryptographic material refers to key materials used to issue electronic identification means, authenticating users and issuing of assertions [...] It is common practice that these security controls are implemented as part of a hardware security module (HSM).

An assertion could be interpreted as an attestation making a claim about the validity status of another attestation. However, the word 'assertion' is mentioned in other places of the Guidance document as relating to claims made about a person by an authority, and not as validity status information.

## ETSI EN 319 411-1

This document describes two roles that are relevant to this text: 1) Certificate status service, and 2) Revocation manager.
