## DID Method Operations

### Create (Register)

DIDs are created implicitly when various concepts and objects in the Nextgraph
architecture and protocol are created, such as user identities, documents, etc.

To create a Nextgraph DID, the following algorithm can be used. It takes as input a list
of tuples, each consisting of a **variant identifier** (`variant-id`) and
**variant-specific data** (`variant-data`). This list must contain at least one tuple.

1. Initialize a string named *ng-did* with value `did:ng`.
1. For each tuple in the list:
   1. Append `:` to *ng-did*.
   1. Append the `variant-id` to *ng-did*.
   1. Append `:` to *ng-did*.
   1. Initialize a byte array *versioned-byte-array* containing `variant-data`.
   1. Preprend the byte `0x00` to *versioned-byte-array*.
   1. Reverse the byte array *versioned-byte-array*.
   1. Encode *versioned-byte-array* using Base64URL-encoding ([[spec:rfc4648]]) and append the result to *ng-did*.
1. Return *ng-did*.

**Example:**

Given the following list of tuples:

| Variant Identifier | Variant-Specific data |
|--------------------|---------------------|
| `a`          | 48 0D 90 4B 3C B2 AA 80 9C 0D 06 29 E0 2D 00 B8 62 93 39 12 02 1F BA DC 10 30 0B 2E 4D 10 3A AE    |

The resulting DID is:

```
did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA
```

### Read (Resolve)

To resolve a `did:ng`, the [`resolve` function](https://www.w3.org/TR/did-resolution/#resolving) of the
W3C [[spec: DID-RESOLUTION]] specification is used.

Note: A DID document is not the same as a Nextgraph document.

DIDs using the `did:ng` method are not only used for discovering cryptographic verification
methods and other technical metadata about a subject, they also directly identify
specific types of resources in graph, and they carry semantics about these resources within
the identifier itself. During resolution, this is reflected by including a `type` property
in the resolved DID document.

**Example DID with `did:ng:a` variant identifier:**

```
did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA
```

**Example DID document:**

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/ed25519-2020/v1"
  ],
  "id": "did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA",
  "type": "ProtectedStore",
  "verificationMethod": [
    {
      "id": "did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA#key-1",
      "type": "Ed25519VerificationKey2020",
      "controller": "did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA",
      "publicKeyMultibase": "z6MkExamplePublicKeyMaterial"
    }
  ],
  "authentication": [
    "did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA#key-1"
  ],
  "assertionMethod": [
    "did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA#key-1"
  ]
}
```

**Example DID with `did:ng:o` variant identifier:** (note: newlines are for display purposes only, the DID is one continuous string)

```
did:ng:o:t9BlAm27i3IPG0a7GQziVtoInABr96vzjBrk2db_r1QA
    :v:2oNpTmCKXjK45kmPrgUPv0FVsL5avs92vUzYqZBb6LcA
    :c:CiA412QJHo8VnRi3TGcmdO_WUSPeLy7Se_Irj2ymhPoA
    :k:BIKTgk0WoYR6zyDWWw0770b9WdKY3zuqdWQcsyVmgMAA
    :s:-Q_t0a_p3v6hoMCu3fn83FAUD14w04xPEYnSf3Fv8hQA
    :k:ozwE0_R_ekcxY_eYKUSijbyTiisDfnORJelFq_OsK0IA
    :l:AAEBBAxuZXh0Z3JhcGguZXUAAAAAAAuPyIt2ThWdpC8Dlo6MnE4e3hWE_2StcUM1_wCt_pktAA
```

**Example DID document:**

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/ed25519-2020/v1"
  ],
  "id": "did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA",
  "type": "Document"
}
```

### Update (Rotate)

The resources identified using `did:ng` (such as Nextgraph documents) can change, however this
does not constitute an update as defined by DID specifications. The DID documents themselves
are not updated. When identifiers with new cryptographic keys are needed, new DIDs are created
rather than updating existing ones.

### Deactivate (Revoke)

The `did:ng` method does not support deactivation.
