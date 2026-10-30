## Identifier

### Target System

The target system of the `did:ng` DID method is the Nextgraph platform, which is
a local-first, encrypted, replicated data platform built around CRDTs, append-only
commit histories, content-addressed encrypted storage, and an associated
peer-to-peer networking protocol. It combines aspects of distributed databases,
cryptographic storage, publish/subscribe messaging protocol, and Semantic Web
technologies.

Its architecture is described in more detail in [Nextgraph Architecture](https://docs.nextgraph.org/en/architecture/?utm_source=chatgpt.com).

### Method Name

The namestring that identifies this DID method is: `ng`. A DID that uses this
method MUST begin with the following prefix: `did:ng`. Per the DID
specification, this string MUST be in lower-case.

The remainder of the DID, after the prefix, is the [method-specific identifier](#method-specific-identifier),
specified below.

### Method-Specific Identifier

The `did:ng` method-specific identifier supports different variants for identifying
a range of resources within Nextgraph. It is prefixed by a 
**variant identifier**, separated by a colon character, and followed by **variant-specific
data**. In some cases, this pair of variant identifier plus variant-specific data can
be repeated to form a sequence, where a subsequent pair of variant identifier and
variant-specific data refines or qualifies the previous pair.

The **variant identifier** is always a single lower-case character.

The **variant-specific data** depends on the variant identifier. It often consists of
32 bytes, prefixed by a single byte detailing the version number of the variant,
which is zero at the moment. The byte array is reversed and encoded as
base64-url strings.

The syntax is specified in the following Augmented Backus-Naur Form (ABNF) notation
[[spec:rfc2234]]:

```abnf
ng-did = "did:ng:" variant-id ":" variant-data *( ":" variant-id ":" variant-data )
variant-id = "a" / "b" / "g" / "o" / ...
variant-data = pubkey / symkey / digest
pubkey = versioned-byte-array
symkey = versioned-byte-array
digest = versioned-byte-array
versioned-byte-array = 44base64url-char
base64url-char  = ALPHA / DIGIT / "-" / "_"
```

The variant identifier not only indicates the content of the variant-specific data
and subsequent structure of the identifier, but also carries semantics about the resource
that is being identified. This is unusual for DID methods, but fully conformant with
the W3C DID Core standard.

List of common variants of DIDs:

| Variant identifier | Variant data | Meaning         |
|--------------------|--------------|-----------------|
| did:ng:i:          | [user-id]    | User Identity   |
| did:ng:o:          | [repo-id]    | Document        |
| did:ng:v:          | [overlay-id] | Overlay         |
| did:ng:l:          | [locator]    | Locator         |
| did:ng:a:          | [repo-id]    | Public Store    |
| did:ng:b:          | [repo-id]    | Protected Store |
| did:ng:c:          | [repo-id]    | Private Store   |
| did:ng:g:          | [repo-id]    | Group Store     |

**Examples of `did:ng`:**

DID identifying a public store (`:a:`):

```
did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA
```

DID identifying a document (`:o:`) with an overlay (`:v:`):

```
did:ng:o:j7sAsZNRibsTWEcLtMpYBBGwMAwkmjNNzCnuvtt7AkUA:v:4gBmGObzB_N3Bwieous7TP1DyXAelpK6aauVyxf4xcMA```
```

DID combining multiple variants:

```
did:ng:o:t9BlAm27i3IPG0a7GQziVtoInABr96vzjBrk2db_r1QA:v:2oNpTmCKXjK45kmPrgUPv0FVsL5avs92vUzYqZBb6LcA:c:CiA412QJHo8VnRi3TGcmdO_WUSPeLy7Se_Irj2ymhPoA:k:BIKTgk0WoYR6zyDWWw0770b9WdKY3zuqdWQcsyVmgMAA:s:-Q_t0a_p3v6hoMCu3fn83FAUD14w04xPEYnSf3Fv8hQA:k:ozwE0_R_ekcxY_eYKUSijbyTiisDfnORJelFq_OsK0IA:l:AAEBBAxuZXh0Z3JhcGguZXUAAAAAAAuPyIt2ThWdpC8Dlo6MnE4e3hWE_2StcUM1_wCt_pktAA
```
