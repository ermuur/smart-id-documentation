<div id="TOC">

*   [Smart-ID Technical Overview](#smart-id-technical-overview)
    *   [Introduction](#introduction)
    *   [Terms and abbreviations](#terms-and-abbreviations)
    *   [Smart-ID services](#smart-id-services)
        *   [Smart-ID levels](#smart-id-levels)
        *   [Main use cases](#main-use-cases)
    *   [Software architecture](#software-architecture)
        *   [Smart-ID solution components](#smart-id-solution-components)
        *   [Smart-ID public APIs](#smart-id-public-apis)
        *   [Smart-ID internal APIs](#smart-id-internal-apis)
    *   [Cryptography](#cryptography)
        *   [RSA digital signature scheme](#rsa-digital-signature-scheme)
        *   [Smart-ID signature scheme](#smart-id-signature-scheme)
    *   [Security](#security)
        *   [Smart-ID Transaction Context](#smart-id-transaction-context)
        *   [Protections against brute-force attack trying all PINs](#protections-against-brute-force-attack-trying-all-pins)
        *   [Clone Detection](#clone-detection)
        *   [Certification of Smart-ID SSCD](#certification-of-smart-id-sscd)

</div>

# Smart-ID Technical Overview

## Introduction

Smart-ID solution is the new generation electronic ID product to take advantage of the smart capabilities of the mobile devices and offering the high level of security to the users.

The solution allows end-users to utilise their existing mobile devices to register for a securely verified Smart-ID account and later use the mobile device to authenticate to RP systems (such as websites, apps and call centres) and to give electronic signatures according to the [eIDAS regulation]([https://ec.europa.eu/digital-single-market/en/trust-services-and-eid), which is recognised in EU member states. The mobile devices doesn’t need a special purpose hardware (such as smart-card readers or Trusted Platform Module chips) and special purpose SIM-cards. The Smart-ID App works with regular devices and doesn’t require extra-ordinary permissions.

Smart-ID is built from the start as the international eID scheme and therefore the Smart-ID account issued in one country can be used for authentication and digital signatures in another country as well. Smart-ID could be the user’s _key_ to all e-services across European Union in the future.

Smart-ID combines well-known technologies and several new technical innovations to make this possible. Together, existing and new technologies provide the following features of the Smart-ID solution:

1.  high level of security,
2.  high level of identity assurance,
3.  non-repudiation of User operations,
4.  strong authentication service, compliant with the [PSD II regulations](http://ec.europa.eu/finance/payments/framework/index_en.htm) and [ECB recommendations](https://www.ecb.europa.eu/press/pr/date/2013/html/pr130131_1.en.html),
5.  electronic signatures, compliant with the [eIDAS regulations](https://ec.europa.eu/digital-single-market/en/trust-services-and-eid).

This document gives closer look to the technical underpinnings of the Smart-ID solution.

## Terms and abbreviations

*   JSON/REST - API interface architecture pattern, which uses the JSON standard for the text encoding and follows the RESTful principles.
*   HSM - Hardware security module. The Smart-ID HSM is running in the FIPS 186-4 mode and provides high level assurances about the security of the private keys and other cryptographic materials protected by the HSM.
*   RA - Registration Authority. Component in the Smart-ID solution, which implements the User registration processes and performs the identity proofing of the Users.
*   RP - Relying Party. This is the external organisation, which is using the Smart-ID solution to authenticate the customers and/or to get digital signatures from the customers.
*   SSCD - Secure Signature Creation Device. Component which securely holds the private key and computes electronic signature in a secure way and mitigates the related security threats. Smart-ID solution includes the SSCD component.
*   QSCD - Qualified Signature Creation Device. An SSCD, which complies with the requirements defined by Annex II of the [Regulation No 910/2014](http://eur-lex.europa.eu/legal-content/EN/TXT/?uri=uriserv:OJ.L_.2014.257.01.0073.01.ENG).
*   User - Physical person, who has subscribed to the Smart-ID services, performed the identity verification and has installed the Smart-ID App on his mobile device.

## Smart-ID services

### Smart-ID levels

Smart-ID service is being offered on two levels.

1.  Smart-ID Basic – User’s identity has been verified by a third party authentication and the identity details has been verified by national population registry.
2.  Smart-ID – User’s identity has been verified by strong authentication, which is based on the government issued eID (ID-card, Mobile-ID) during the on-line registration or the government issued physical ID document has been verified by two RA employees during the on-site registration.

The difference is mainly on the level of the identity proofing, which is performed during the registration process. The Smart-ID App and other system components are exactly the same.

In the following document we will use the “Smart-ID” for referring to both levels.

### Main use cases

Smart-ID solution provides the following services:

1.  Authentication of the User
2.  Digital signature of the User
3.  Registration of the User
4.  Management of the Smart-ID account

In the following sections, we will give overview of the services.

#### Authentication service

Authentication service is provided to the RP websites and mobile apps via the RP-API component. The RP-API works with the JSON/REST style methods. The support for the OpenID Connect protocol is in the development.

High-level simplified steps of the authentication service are the following.

1.  User navigates to the RP website, chooses the Smart-ID authentication method and enters the user identifier, such as national personal code or other username.
2.  RP sends authentication request to the Smart-ID Core.
3.  Smart-ID Core creates the new authentication session, generates the data structure, whose digest _H_ will be signed by the User and returns the data structure and sessionID to the RP.
4.  RP computes the Verification Code of the authentication request and displays it to the user’s browser.
5.  Smart-ID Core sends the notification about the new authentication request to the user’s mobile device, via the push messaging platform.
6.  Smart-ID App receives the push notification (or user launches the Smart-ID App manually) and connects the Smart-ID Core and requests the authentication request details.
7.  Smart-ID App computes the Verification Code and displays it with the authentication request details to the user and asks for confirmation and the PIN1.
8.  User enters the PIN1.
9.  Smart-ID App performs the share of the signature generation steps with the share of authentication key pair and sends the result to the Smart-ID Core. (Refer to the section [Smart-ID Signature Computation](#smart-id-signature-computation) for cryptographic details.)
10.  Smart-ID Core forwards the signature completion request to the Smart-ID SecureZone.
11.  Smart-ID SecureZone completes the signature generation steps with the shares of the authentication key pair and returns the composite signature to the Smart-ID Core.
12.  RP request the authentication session status from the Smart-ID Core.
13.  Smart-ID Core returns the authentication response to the RP.
14.  RP verifies the validity of the authentication response and creates new session under the identity of the authenticated user.

#### Digital signature service

The digital signature service is very similar to the authentication service. Few differences are the following:

1.  The RP sends Smart-ID Core already the digest _H_ of the document to be signed. The RP-API doesn’t see the actual contents of the document.
2.  The Smart-ID App asks for the PIN2 and uses the share of signing key pair to compute the share of the signature.

#### Registration service

Registration service has many flavours and versions, for example, depending on which kind of identity proofing documentation User presents, how old the User is and other such details. However, there are general principles and the simplified overview of the generalised registration service steps is the following.

1.  User installs the Smart-ID App and starts it for the first time.
2.  User chooses the registration process and chooses the appropriate third party authentication service. The options include the ID-card, Mobile-ID, banklink, on-site registration. Depending on the particular technology, the authentication may take place in the mobile device itself (for example, in case of Mobile-ID and banklink) or the User may be instructed to open up the desktop browser session (for example, in case of ID-card) or the User may be instructed to visit the on-site registration office.
3.  Smart-ID App begins to generate the shares of the authentication and signature key pairs (refer to the section [Smart-ID Key Pair Generation](#smart-id-key-pair-generation) for cryptographic details).
4.  User sets PINs for the shares of the key pairs.
5.  Smart-ID App initiates the registration process with the Smart-ID Core and requests for the time-limited registration nonce, computes the registration token and displays the token to the user.
6.  User enters the registration token to the identity proofing process in the desktop browser or shows the registration token to the on-site registration office employee. This binds together the fresh key material generated inside the Smart-ID App instance and the identity proofing session inside the user’s browser or on-site registration office. The identity proofing process takes place inside the RA component and when finished, sends the confirmed user’s identity details and the registration token to the Smart-ID Core.
7.  Smart-ID App proceeds with the registration process and sends the registerDevice() request with the registration nonce, “Server’s part of the private key” of the authenticate key pair and “Server’s part of the private key” of the signature key pair and other data to the Smart-ID Core.
8.  Smart-ID Core verifies the registration nonce, asks the Smart-ID SecureZone to generate its own shares of the key pairs and computes the composite public keys and generates the CSRs with the User’s identity for issuing the certificates to the authentication key pair and signature key pair.
9.  Smart-ID App receives the CSRs and asks user to confirm the identity details and to enter the PIN1 and PIN2 to sign the CSRs.
10.  User confirms the identity details and enters the PIN1 and PIN2.  

11.  Smart-ID App uses the shares of the corresponding key pairs to compute the share of the signature of CRSs and sends the result to the Smart-ID Core.
12.  Smart-ID Core forwards the signature completion request to the Smart-ID SecureZone.
13.  Smart-ID SecureZone completes the CSR signature generation steps with the shares of the key pairs and returns the composite signatures to the Smart-ID Core.
14.  Smart-ID Core sends the certificate issuing request to the Smart-ID CA along with the CSRs, User identity details and other verification data.
15.  Smart-ID CA issues the certificates.
16.  Smart-ID Core activates the User’s account.
17.  Smart-ID App informs the User that the Smart-ID account is ready for use.

## Software architecture

The high-level overview to the simplified context of the Smart-ID solution is given in the Figure 1.

![Figure 1: Simplified Smart-ID context diagram](https://github.com/SK-EID/smart-id-documentation/blob/master/images/smart-id-system-context.png) _Figure 1: Simplified Smart-ID context diagram_

### Smart-ID solution components

1.  Smart-ID App Lib - Component handles all the cryptography, account management and server communication related tasks inside the Smart-ID app.
2.  Smart-ID App UI - This handles the UI and user interactions.
3.  Smart-ID App - The Android/IOS application, which the User installs on his mobile device in order to register for the Smart-ID account and to authenticate himself to the RP services and to give digital signatures.
4.  GCM/APN - The push notification platform, such as Google Cloud Messaging and Apple Push Notification, relays the notifications from Smart-ID Core to the Smart-ID App instances.
5.  Smart-ID Core - This component implements the business logic, manages the database and provides API-s to the mobile devices, RP-s and RA. It also mediates the messages between the Smart-ID App Lib and Smart-ID SecureZone components.
6.  Smart-ID SecureZone - This is the component, which is handling the cryptography tasks and key pair generation on the server-side. The SecureZone also requests the cryptographic operations from Smart-ID HSM.
7.  RA - Registration Authority. This is the component, which implements the registration processes and performs the Registration Authority duties. There are multiple RA-s to correspond to the different registration process variants, for example, on-site registration with human employees, performing the identity proofing and self-service registration portals, which use third-party authentication services.
8.  CA - This is the CA, which is issuing the X.509 certificates for the authentication and signature key pairs. There are multiple CA-s to correspond to the different levels of Smart-ID accounts, for example, certificates for the Smart-ID Basic accounts are issued from different CA.
9.  SSCD - Secure Signature Creation Device. This is the logical component, consisting of the Smart-ID App Lib and Smart-ID SecureZone and Smart-ID HSM components and which provides same kind of services as the smart-card based SSCDs. It is considered during the security analysis and the Common Criteria evaluation.

### Smart-ID public APIs

#### Relaying Party API (RP-API)

RP-API is used by the RPs for requesting authentication of the Users and digital signatures from the Users. The RP-API uses two styles of interfaces for enabling such services.

1.  JSON/REST style interface.
2.  OpenID Connect compliant interface.

JSON/REST style interface allows RPs to simply integrate the Smart-ID solution with their custom in-house built information systems and start using the authentication and digital signature services. Also, this allows RPs to customise the authentication dialog and better integration with their web-page.

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) compliant interface allows RPs to easily and quickly integrate the Smart-ID solution with their standards compliant software. The OpenID Connect interface uses the authentication portal, in a similar way as the [Google Sign In](https://developers.google.com/identity/protocols/OpenIDConnect) and [Facebook Login](https://developers.facebook.com/docs/facebook-login) services. The OpenID Connect interface is currently in the development.

For additional details, please refer to the RP-API documentation, which is available at the https://www.smart-id.com.

### Smart-ID internal APIs

#### Mobile Device API (MD-API)

The MD-API is used by the Smart-ID App instances, to communicate with the Smart-ID Core and to run the account registration, transaction management and signature computation protocols.

The MD-API is a JSON/RPC-style API. It is built on the principle that access to the MD-API is protected with the HTTP Basic authentication. Only two methods are public methods, which can be called from unknown sources and access to those are not restricted. During the device registration, new Smart-ID App instance is assigned unique identifier and random password, which is then used as transport layer security measure, to authenticate the queries to all other methods. The access control is enforced, so that only those app instances get access to the data, which is associated with this app instance.

#### Registration Authority API (RA-API)

RA-API provides interface for the RA-s to start the registration processes and transmit the verified User identity details to the Smart-ID Core.

#### Smart-ID SecureZone API (SZ-API)

SZ-API provides interface for the Smart-ID Core to invoke following functions.

1.  Generate “Server’s share of the private key”.
2.  Register “Server’s part of the private key” transmitted from the Smart-ID App during the registration.
3.  Compute “Composite public key”.
4.  Generate the composite signature.

The functions are logically invoked by the Smart-ID App Library and the function calls are relayed by the Smart-ID Core. Sensitive data, such as “Server’s part of the private key” and “App’s share of the signature” and others are encrypted by the Smart-ID App Lib to the public key of the SecureZone.

#### Customer Service API (CS-API)

CS-API provides the self-service portals and helpdesk employees information about the status of the Smart-ID account and allow temporarily block the account and permanently close the account. Self-service portals also have a way to see the history of the transactions and events, which are registered for particular Smart-ID account.

## Cryptography

Smart-ID solution is based on the known and proven principles of the [public key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography), [digital signature schemes](https://en.wikipedia.org/wiki/Digital_signature) and [PKI (Public Key infrastructure)](https://en.wikipedia.org/wiki/Public_key_infrastructure).

Public key cryptography work on the concept of key pairs. The key pair consists of the public key and private key, where the public key is published in the X.509 certificate and bind with the verified identity of the User. Private key is confidential and usually protected inside the smart-card or equivalent device, which is under sole control of the User.

Smart-ID solution handles the protection of the User’s private key by using the results from the [https://en.wikipedia.org/wiki/Threshold_cryptosystem](https://en.wikipedia.org/wiki/Threshold_cryptosystem). Threshold cryptography studies such systems, where, in order to decrypt an encrypted message (or to sign a message), several parties must co-operate in the decryption protocol (or signature computation protocol). Threshold cryptography provides the benefit that compromise of a single party (or revealing the share of a single party) doesn’t result the compromise of the whole system. Historically, this kind of techniques are used in the highly critical systems, where [https://en.wikipedia.org/wiki/Two-man_rule](two man’s rule) must be enforced. For example, the key for the decryption may be shared between independent members of the management team and executing some critical function requires that all key holders are present.

Advances in the threshold cryptography and increase of the computing power inside the mobile devices allows us to use similar techniques for securing the private key, which is used in public key encryption algorithms. In the Smart-ID solution, the separate and independent parties, which hold the shares of the private keys are following.

1.  User’s mobile device
2.  Smart-ID service provider

With the Smart-ID solution, the private key of the User is never generated or combined in a single place of location. Instead, we use the distributed generation protocol and generate the shares of the private key in the User’s mobile device and in the Smart-ID service. The shares are generated, processed, stored and protected in the separate physical locations and never combined.

The following sections describe, how the standard RSA signature scheme is modified to work with threshold cryptography.

### RSA digital signature scheme

We repeat some of the concepts from the RSA digital signature scheme. Please refer to the [RFC3447](https://tools.ietf.org/html/rfc3447) or modern cryptography textbook for additional details.

The key pairs in the RSA crypto-system have the private key and public key with the following associated terms:

1.  RSA modulus (_n_) – public large integer, product of large prime numbers.
2.  RSA public exponent (_e_) – public fixed integer, usually 65537.
3.  RSA private exponent (_d_) – confidential large integer.
4.  RSA private key _(n, d)_
5.  RSA public key _(n, e)_

#### Generating RSA signature

Computing the RSA signature _s_ on the message _m_ with private key (_n_, _d_), means mathematically to compute the

    s = m ^ d mod n.

Generating the signature _S_ of scheme `RSASSA-PKCS1-v1_5` of the message _M_ with RSA private key _K_ means computing the output of the algorithm `RSASSA-PKCS1-V1_5-SIGN(K, M)`, as defined in the section in the 8.2.1 of the RFC3447.

#### Verifying RSA signature

Verifying the RSA signature _s_ on the message _m_, with public key _(n, e)_ means mathematically to compute the

    m' = s ^ e mod n,

and verifying if the _m’_ is the same as the _m_.

Verifying the signature _S_ of scheme `RSASSA-PKCS1-v1_5` of message _M_ with RSA public key _(n, e)_ means computing the output of the algorithm `RSASSA-PKCS1-V1_5-VERIFY((n, e), M, S)` as defined in the section 8.2.2 of the RFC3447.

### Smart-ID signature scheme

Smart-ID signature scheme extends the RSA digital signature scheme in such way, that we can generate the private key in the multiple key shares and then compute the composite signature without combining the key shares. The resulting composite signature _S_ is a RFC3447 `RSASSA-PKCS1-v1_5` compliant signature.

#### Smart-ID key pair components

Smart-ID key pair consists of the public key and private key.

The private key exists in the form of the following components:

1.  Application’s part of the private key (_d1’_) – This is generated inside the Smart-ID app and is stored inside the mobile device. It is protected with the PIN, chosen by the user during the Smart-ID account registration procedure.
2.  Server’s part of the private key (_d1’’_) – This is generated inside the Smart-ID app and transmitted from the mobile device to the Smart-ID Server and stored inside the database. It is protected with HSM keys.
3.  Server’s share of the private key (_d2_) – This is generated inside the Smart-ID server and stored inside the Smart-ID HSM. It is protected with HSM keys.

The public key exists in the form of the following components and also in the composite form:

1.  Application’s share of the public key (_n1, e_) – This is generated inside the Smart-ID app.
2.  Server’s share of the public key (_n2, e_) – This is generated inside the Smart-ID server.
3.  Composite public key (_n, e_) – This is computed inside the Smart-ID server, by multiplying together the _n1_ and _n2_.

#### Smart-ID key pair generation

The components of the Smart-ID key pair is generated during the registration process of the User. In this section, we describe the cryptographic details. Please refer to the section 12.12 for additional details about the registration processes.

From cryptography point of view, the key pair generation process is the following:

1.  Smart-ID App Library generates the 2048-bit RSA key pair with the private key (_n1, d1_) (this is called “Application’s share of the private key”) and public key (_n1, e_) (this is called “Application’s share of the public key”).
2.  Smart-ID App Library uniformly generates the random number _d1’_ from the range _0_ .. _2^len(n1)_. This will be called “Application’s part of the private key”.
3.  Smart-ID App Library computes _d1’‘= d1 - d1’ mod n1_. The _d1’’_ will be called “Server’s part of the private key”.
4.  Smart-ID App Library encrypts the output of `I2OSP (d1')` with the cipher “AES/CBC/NoPadding”, with the AES key derived from the users PIN and deletes the clear-text value of the _d1’_.
5.  Smart-ID App Library deletes the “Application’s share of the private key”.
6.  Smart-ID App Library sends the “Application’s share of the public key” and “Server’s part of the private key” to the Smart-ID SecureZone.
7.  Smart-ID SecureZone generates the 2048-bit RSA key pair with private key (_n2, d2_) (this is called “Server’s share of the private key”) and public key (_n2, e_) (this is called “Server’s share of the public key”) with the Smart-ID HSM. The private key will be protected by HSM and will be never exported.
8.  Smart-ID SecureZone computes the constants _α_ and _β_ using the Extended Euclid’s algorithm, so that _α_n1 + β_n2 = 1_.
9.  Smart-ID SecureZone computes the user’s composite public modulus _n = n1 * n2_.

#### Smart-ID signature computation

In order to compute the signature _S_, we need access to the all components of the Smart-ID private key and we use the shares to compute the individual _signature shares_.

1.  User initiates the signature process with the RP.
2.  RP computes the digest _H_ of the message _M_ and sends the signature request to the Smart-ID service.
3.  Smart-ID server sends the signature transaction request to the User’s mobile device along with the digest _H_.
4.  Smart-ID app applies the `EMSA-PKCS1-V1_5-ENCODE` to the digest _H_ (with the exception of the step 1, the RP has already applied the hash function to the message _M_ and has sent use the digest _H_) and computes the encoded message _EM_.
5.  Smart-ID app asks user for the PIN and decrypts the “Application’s part of the private key”.
6.  Smart-ID app computes the _m_ = `OS2IP (EM)`.
7.  Smart-ID app computes the signature share _s1’_ = `RSASP1 (d1', m)`.
8.  Smart-ID app sends the signature share _s1’_ to the Smart-ID server.
9.  Smart-ID server applies the `EMSA-PKCS1-V1_5-ENCODE` to the digest _H_ and computes the encoded message _EM_.
10.  Smart-ID server decrypts the “Server’s part of the private key” with the KWK with the Smart-ID HSM.
11.  Smart-ID server computes the _m_ = `OS2IP (EM)`.
12.  Smart-ID server computes the signature share _s1’’_ = `RSASP1 (d1'', m)`.
13.  Smart-ID server computes the signature share _s1_ = _s1’_ * _s1’’_ mod _n1_.
14.  Smart-ID server verifies the signature share _s1_ against the “Application’s share of the public key”.
15.  Smart-ID server computes the signature share _s2_ = `RSASP1 (d2, m)`.
16.  Smart-ID server computes the composite signature _S_ = _β_ * _n2_ * _s1_ + _α_ * _n1_ * s2 mod _n_.
17.  Smart-ID server verifies the signature _S_ against the composite public key.
18.  Smart-ID server returns the signature _S_ to the RP.

Note that this is a simplified version of the signature generation algorithm. We have omitted multiple implementation details and other security measures, for the clarity of this overview document. For example, the following security measures are not described in this section.

*   Computing and displaying the verification code to provide the context of the signature transaction to the Signatory.
*   Protection against the brute-force attack to try all possible PINs.
*   Protection against the side-channel attacks to try to deduce the “Application’s part of the private key”.
*   Detecting cloned devices with copy of the “Application’s part of the private key”.
*   Transport level protection with authenticating the Smart-ID app instances.
*   Transport level protection with the SSL/TLS channels and certificate pinning.
*   Transport level protection with encrypting the signature share _s1’_ with the public key of the Smart-ID server.

Please refer to the chapter [Security](#security) for additional discussion of these and other security measures implemented in the Smart-ID solution.

#### Smart-ID signature verification

The composite signature _S_ is a RFC3447 `RSASSA-PKCS1-v1_5` compliant signature. One can verify it with the standard RSA signature verification procedure.

## Security

Breakthrough cryptography is just one building block for the Smart-ID solution. To achieve availability, reliability and security, we have included additional security measures in the Smart-ID App and in the Smart-ID Core. In this section, we give the short overview of the security measures.

### Smart-ID Transaction Context

Smart-ID authentication and signing protocol includes the Verification Code (VC). Because Smart-ID is mostly used in the use case, where the user is accessing the RP website with a browser running on some other device, other than the Smart-ID App itself, we need a way to bind the user’s session on the browser and the authentication/signing transaction on the Smart-ID App. This allows the end-user to understand, what is the context of the authentication/signing transaction, which suddenly popped up on his mobile device. The end-user can then distinguish between the valid transaction, requested by himself on the RP website and other transactions, which might have been initiated by attacker, who is trying to perform the phishing attack.

Smart-ID has followed the good example of Estonian Mobile-ID, which includes the similar feature.

VC is computed from the data to be signed (DTBSR) by Smart-ID app and by the RP itself. The RP shows the VC to the user in the browser session and the Smart-ID App shows the VC to the user within the transaction notification.

The VC is not so useful, when the user’s browser or RP’s app is running in the same device, as the Smart-ID App, because user doesn’t usually have a good way to keep the browser window and Smart-ID App window displayed at the same time and easily compare the VC-s. For this use case, special protocol is being developed, which allows the browser or app to communicate locally with the Smart-ID App as well and this way mitigate the phishing attacks.

### Protections against brute-force attack trying all PINs

If the Smart-ID user would loose his mobile device, the obvious attack is to try all PINs. Because the space of the PINs is not very large, the attacker might easily try all the possible 10 000 PINs and try figure out, which is the correct one. Smart-ID solution includes several security measures against those kind of attacks.

#### Time-delay locks

The Smart-ID solution is built this way that the PIN itself is never stored inside the Smart-ID App. The detection of the correct PIN and wrong PIN is actually performed in the Smart-ID Core.

In case the Smart-ID Core learns that user has entered the wrong PIN to the Smart-ID App (note that the PIN is not transmitted to Smart-ID Core either, more on that later), it increases the counter stored inside the Smart-ID database. In case the counter of wrong PIN tries reaches a threshold, the user’s account is locked for the certain time period.

In case the user has entered the wrong PIN three times in a row, the user’s account is locked for 3 hours. During those 3 hours, authentication and signature transactions are not accepted and PIN tries are not allowed.

After the 3 hours time-delay lock has expired, the user’s account is unlocked and the user has 3 more tries again. In case the wrong PIN has been entered again three times in a row, the user’s account is locked for 24 hours.

After the 24 hours time-delay lock has expired, the user has 3 final tries to enter the correct PIN for new transactions. In case the wrong PIN has been entered the final three times in a row, the user’s account is closed and the certificates are revoked.

#### Protection against off-line brute-force attacks

During the normal usage of the Smart-ID App, detection of the correct PIN and wrong PIN is performed by the Smart-ID server, which applies the time-delay locking, in case the attacker might try all possible PINs. The attacker may try to analyze the locally stored information as well, to deduce the share of the private key or PIN. Smart-ID solution is carefully designed in this way that the data available to the attacker, which is stored inside the Smart-ID App doesn’t give attacker any hints about the PIN or private key.

This is achieved with the special design of the cryptographic shares of the private key and specially designed protection for the “Application’s part of the private key”.

Smart-ID App stores the “Application’s part of the private key” in the private application storage space and encrypts it with the AES key, derived from the user’s PIN.

First, the “Application’s share of the private key” has the corresponding counterpart “Application’s share of the public key”. In case the Smart-ID App would store the encrypted “Application’s share of the private key” and the attacker might have got his hands on the “Application’s share of the public key”, the attacker can simply try all possible PIN-codes, derive the AES encryption key, decrypt the “Application’s share of the private key” and then verify, which of the possible keys match with the “Application’s share of the public key”.

This is prevented by further additively sharing the “Application’s share of the private key” into two parts:

    1\. "Application's part of the private key"

    1\. "Server's part of the private key"

For those parts, the corresponding public keys do not exist. So, signature shares computed with the “Application’s part of the private key” cannot be verified for validity and the attacker doesn’t have a reference point to distinguish between the correct “Application’s part of the private key” and incorrect.

The validity of the signature share computed within the Smart-ID App can only be verified, once the share is combined with the signature share computed within the Smart-ID Core, with the “Server’s part of the private key”.

Therefore, even when the attacker has the encrypted copy of the “Application’s part of the private key”, attacker cannot launch the off-line brute-force attack and try out all possible PINs.

### Clone Detection

Clone detection is the feature, which tries to identify the situation, when the PIN of the user and the “Application’s part of the private key” of the user has been leaked and the attacker is actively trying to issue signatures under the name of the user. For example, the storage of the app may have been leaked from the backup copy of the mobile device. The PIN may have been eavesdropped by the attacker, when looking over the shoulder of the user. In a usual, smart-card based solution, such situation means that the user’s private key would have been totally compromised and the attacker can issue unlimited number of valid signatures.

Smart-ID solution has been designed in such a way that all MD-API methods, which operate on the shares of the private keys, require specific one-time content from the previous invocations. After the one-time material has been used, Smart-ID Core supplies a new randomly generated material to the mobile device and expects that to be returned next time, when the mobile device tries to perform an operation on the key share.

In case the Smart-ID Core receives such MD-API method invocation, which contains the otherwise correct information (app instance password, correctly computed signature share and other necessary details) but wrong one-time material, it means that the Smart-ID App instance is out-of-sync with the server or that there are more than two copies of that Smart-ID App instance and key pair. As a cautionary security measure, the user’s account will be then closed and the certificates will be revoked, to protect the user from the further damage. User is then notified of the suspected security breach.

Clone detection works on top of three nonces:

*   one-time password – created by Smart-ID Core in the end of each operation (incl. initialisation) and valid until the completion of next.
*   retransmit nonce – created in the beginning of each operation by Smart-ID App, the same value must be used when Smart-ID App retries messages to Smart-ID Core, related to the same operation.
*   freshness token – created by Smart-ID Core before each submission operation from Smart-ID App to Smart-ID Core. Ensures that state-changing operations get executed in the order client issued them (although some may be missing from between).

The three kind of one-time materials are required, so that the API method invocations could be safely repeated by the Smart-ID App, when connecting over the unreliable network.

The clone detection protocol is mostly intended to be used during the signature creation protocol. Additionally, the Smart-ID App registers the alarms with the mobile device operating systems and periodically performs the clone detection in the background as well, even when the user haven’t explicitly started a new authentication or signature transaction.

### Certification of Smart-ID SSCD

The Smart-ID SSCD is the logical component, which provides the same kind of features as the smart-card based Secure Signature Creation Device (SSCD). Smart-ID solution has been carefully designed from the start to fulfil all the QSCD requirements. In order to gain trust and confidence in the Smart-ID solution and to verify that the QSCD requirements have been met, the solution has been submitted to the Common Criteria evaluation.

Smart-ID SSCD consists of the following components.

*   Smart-ID App Library
*   Smart-ID SecureZone
*   Smart-ID HSM

Together, they provide the following functionality.

1.  Generation of the public key of the user and shares of the private key.
2.  Computation of the signature _S_ on the message _M_, while under the sole control of the user
3.  Destruction of the user’s key material, after the keys are no longer useful.

The Common Criteria evaluation is done on the level EAL4 augmented with the component `AVA_VAN.4`. This means that during the evaluation it is assumed that Smart-ID SSCD components are subject to deliberate attacks by experts possessing a high attack potential with advanced knowledge of security principles and concepts employed by the Smart-ID SSCD.