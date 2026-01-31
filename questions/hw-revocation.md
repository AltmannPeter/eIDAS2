# Requirements for hardware protected revocation keys

Summary: CIR 2015/1502 does not mandate hardware-protected signing keys for revocation. However, ETSI EN 319 411-1 includes such requirements for CA keys and for TSPs operating under NCP+. While OCSP responders are not explicitly required to use hardware-protected keys, the wording is ambiguous and may imply such a need in certain cases. The applicability of these requirements to BBS-signed attestations is unclear and not addressed in the text.

## Background 

The work in TS14 has raised the question of whether signing keys for validity status objects (e.g., revocation lists or validity status tokens) must be hardware-protected. This text examines the topic by reviewing the following sources:

1. [Regulation No 910/2014](https://eur-lex.europa.eu/eli/reg/2014/910/oj/eng)
2. The [CIR 2015/1502](https://eur-lex.europa.eu/eli/reg_impl/2015/1502/oj/eng) with its associated [guidance](https://docs.google.com/document/d/153aFq40pzHETYkz12sJeDTczRDvECiDO/edit?usp=sharing&ouid=113683449039040581284&rtpof=true&sd=true)
3. [ETSI EN 319 411-1](https://www.etsi.org/deliver/etsi_en/319400_319499/31941101/01.04.01_60/en_31941101v010401p.pdf)
4. The [CIR 2025/2160](https://eur-lex.europa.eu/eli/reg_impl/2025/2160/oj)

Next, key terms used in this text are defined. 

## Definitions

### Level of Assurance

The concept of Assurance Level is defined and treated across various sources in the following ways:

> LoA describes the degree of confidence in the processes leading up to and including the authentication process itself, thus providing assurance that the entity that uses a particular identity is in fact the entity to which that identity was assigned. - ISO/IEC 29115

> Assurance levels should characterise the degree of confidence in electronic identification means in establishing the identity of a person, thus providing assurance that the person claiming a particular identity is in fact the person to which that identity was assigned. - Regulation (EU) No 910/2014

> The [eID] means is designed so that it: 1) can be assumed to be used only if under the control or possession of the person to whom it belongs, 2) can be reliably protected by the person to whom it belongs against use by others. - CIR 2015/1502 Annex Section 2.2.1

The Guidance document further clarifies what *reliably protected* means. 

> 'reliably protected' refers to the efforts taken to prevent the electronic identification means from being used without the subject's knowledge and active consent.

In summary, the level of assurance reflects the degree of confidence that an electronic identification means is used exclusively by its rightful holder and accurately represents their identity.

### Validity Status Checks

There are statements concerning misuse and alteration that, at first glance, may appear relevant to validity status checking.

> [...] characterised with reference to technical specifications, standards and procedures related thereto, including technical controls, the purpose of which is to prevent misuse or alteration of the identity. - Regulation (EU) No 910/2014 Article 8 2§

However, Regulation (EU) No 910/2014 Article 8 does not appear to include validity status checks among the specifications, standards, and procedures related to assurance levels. In fact, it clarifies that authentication is separate from validity status checking (cf. the Guirande section):

> the authentication mechanism, through which the natural or legal person uses the electronic identification means to confirm its identity to a relying party; - Regulation (EU) No 910/2014 Article 8 3§

> The release of person identification data is preceded by reliable verification of the electronic identification means and its validity. - CIR 2015/1502 Annex Section 2.3.1

In summary, although concerns about misuse and alteration are addressed in the context of assurance levels, the regulatory texts treat validity status checking as a distinct process, separate from authentication and not explicitly included in the assurance level framework.

## CIR 2015/1502 and its Guidance

Annex Section 2 outlines technical specifications and procedures. Section 2.2 focuses on means management, with Section 2.2.3 being the only part specifically addressing validity status controls. It highlights three key points:

1. It is possible to suspend and/or revoke an electronic identification means in a timely and effective manner.
2. The existence of measures taken to prevent unauthorised suspension, revocation and/or reactivation.
3. Reactivation shall take place only if the same assurance requirements as established before the suspension or revocation continue to be met.

Notably, the requirements are identical across all three assurance levels, meaning that substantial and high levels are held to the same standards as low.

Point two requires the *existence of measures* to prevent unauthorized changes to validity status, but does not specify what those measures are. The Guidance document further clarifies that:

> This will generally imply authentication of the requester's authorisation to act in that way. It should be determined who can authorise suspension and/or revocation besides the user – for example relevant public authorities.

There is no specific mention of how signing keys for revocation objects are protected.

Validity status checks are also mentioned in section 2.3.1. on the Authentication mechanism.

> The release of person identification data is preceded by reliable verification of the electronic identification means and its validity through a dynamic authentication.

In this context, validity specifically refers to dynamic authentication, which is defined as:

> an electronic process using cryptography or other techniques to provide a means of creating on demand an electronic proof that the subject is in control or in possession of the identification data and which changes with each authentication between the subject and the system verifying the subject's identity;

Note that this does not address the validity status of the attestation itself. Rather, it focuses on the validity of the electronic identification means by requiring proof that the identity subject retains control. The Guidance document provides further clarification on this definition:

> The primary purpose of dynamic authentication is to mitigate against attacks such as ‘man-in-the-middle’ or misusing verification data from a previously recorded authentication replay to the verifier.

As before, it is not the validity status of the attestation that is considered.

Security controls for the authentication mechanism also has a particular focus.

> The authentication mechanism implements security controls for the verification of the electronic identification means, so that it is highly unlikely that activities such as guessing, eavesdropping, replay or manipulation of communication by an attacker with [enhanced-basic/substantial/high] attack potential can subvert the authentication mechanisms.

None of this applies to validity status checks.

With respect to hardware requirements, Section 2.4.6 outlines the necessary technical controls. For assurance levels substantial and high, the following is stated:

> Sensitive cryptographic material, if used for issuing electronic identification means and authentication is protected from tampering

Note that only issuance and authentication are mentioned, both of which are distinct from validity status checks. Instead, validity status checks could be interpreted as covered by measures already under assurance level low.

> The existence of proportionate technical controls to manage the risks posed to the security of the services, protecting the confidentiality, integrity and availability of the information processed.

If validity status checking is a service, then all that is mentioned is that *proportionate* technical controls are in place. The Guidance document does not include any mention of validity status checking as a service under its guidance for section 2.4.6. However, it does mention the following:

> Sensitive cryptographic material refers to key materials used to issue electronic identification means, authenticating users and issuing of assertions [...] It is common practice that these security controls are implemented as part of a hardware security module (HSM).

An assertion could be interpreted as an attestation making a claim about the validity status of another attestation. However, the word 'assertion' is mentioned in other places of the Guidance document as relating to claims made about a person by an authority, and not as validity status information.

In summary, CIR 2015/1502 and its Guidance do not explicitly require hardware protection for signing keys used in revocation. References to sensitive cryptographic material are limited to issuance and authentication functions. Since validity status checks are not clearly defined as high-assurance services, revocation keys appear to fall under low-assurance requirements, where only proportionate technical controls are expected.

Next, ETSI EN 319 411-1 is analyzed.

## ETSI EN 319 411-1

The text includes several sections, with those relevant to validity status checks discussed below.

### Section 6.2.4 

Section 6.2.4 defines requirements for revocation requests, including who may submit them, submission methods, confirmation steps, revocation reasons, distribution mechanisms, and delay limits. REV-6.2.4-03A sets a 24-hour maximum for certificate revocation. All requests must be authenticated and authorized.

### Section 6.3.3

Key protection is addressed in GEN-6.3.3-08 (NCP) and GEN-6.3.3-09A (NCP+) as follows:

> If the CA generated the subject's key pair, the private key shall be securely passed to the registered subject; or to the TSP managing the subject'sprivate key;

> If the TSP generated the subject's key pair, the secure cryptographic device containing the subject's private key shall be securely delivered to the registered subject or, in the case of a third party TSP managing the key on behalf of the subject, to that third party TSP.

Secure hardware is mentioned explicity in the context of certificate signing keys when these are issued by the CA and for NCP+.

### Section 6.3.9

Section 6.3.9 covers revocation and suspension, listing conditions under which a TSP must revoke a non-expired certificate and the obligation to notify users. REV-6.3.9-15 states that revocation is not required for short-term certificates. REV-6.3.9-17 adds a conditional requirement to notify users of issues with such certificates.

Notably, Section 6.3.9 does not require protection of revocation signing keys. The only related obligation appears to be the notification requirement in REV-6.3.9-17.

### Section 6.3.10

Section 6.3.10 outlines requirements for certificate status services provided by a TSP. It covers aspects such as availability, supported mechanisms (OCSP and CRL), and handling of response mismatches. Among these, one requirement specifically addresses the status information itself:

> The integrity and authenticity of the status information shall be protected - CSS-6.3.10-03

This could be interpreted as requiring hardware protection for signing keys. However, other sections specify hardware protection only when *secrecy* of key material is needed. For integrity and authenticity, it may suffice to sign status information with a key linked to the certificate status service.

### Sections 6.5 - 6.6

Section 6.5 addresses technical security controls, beginning with key pair generation and installation. Notably, GEN-6.5.1-02 specifically deals with a TSP generating keys used for revocation services.

> The TSP shall generate CA keys, including keys used by revocation and registration services, securely and the private key shall be secret.

Note that the text only requires *securely* without prescribing exactly how this should happen. The next couple of points, further detail what secure generation entails:

- GEN-6.5.1-03 is about physical location security
- GEN-6.5.1-04 is about dual control during key pair generation
- GEN-6.5.1-05 is about personnel practices for key generation
- GEN-6.5.1-06 is about algorithm usage
- GEN-6.5.1-07 is about key length

Neither of these mention the need for hardware securing the revocation signing key.

Later, in SDP-6.5.1-23 (NCP+), there is a hardware requirement for the case when the CA generates the subject's keys:

> If the CA generates the subject's keys, the TSP shall secure the issuance of a secure cryptographic device to the subject.

One interpretation of the above is that revocation keys that are used on NCP+, should be protected in secure hardware. This is supported by text in section 6.5.2.

> TSP's key pair generation, including keys used by revocation and registration services, shall be carried out within a secure cryptographic device which is a trustworthy system which: a) is issued to EAL 4 or higher [...]; or b) meets the requirements identified in [ISO/IEC 19790, FIPS PUB 140-2 level 3 or FIPS PUB 140-3 level 3 - OVR-6.5.2-01

This seems to be the strongest support in favor of the requirement to have secure hardware for protecting revocation signing keys. But, this is specifically limited to CA keys and may or may not apply to eID / (Q)EAA revocation keys. Section 6.6.3, however, suggests that these requirements extend beyond CA keys. 

> an OCSP responder key compromise is, in this case, as severe as a compromise of the corresponding issuing CA key. To this reason, the TSP carefully considers OCSP responder key management - CSS-6.6.3-01A

As before, more vague words like *carefully considers* and no hard requirements like those related to CA keys. Again, seemingly, the requirement to have a secure cryptographic device for revocation signing keys depends on whether or not the TSP offering the revocation service falls under NCP+.

