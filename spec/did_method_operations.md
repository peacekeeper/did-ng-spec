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

#### Examples

**Example 1: Single variant (Public Store)**

Given the following list of tuples:

| Variant Identifier | Variant-Specific data |
|--------------------|---------------------|
| `a`          | ```48 0D 90 4B 3C B2 AA 80 9C 0D 06 29 E0 2D 00 B8```<br/>```62 93 39 12 02 1F BA DC 10 30 0B 2E 4D 10 3A AE``` |

The resulting DID is:

```
did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA
```

**Example 2: Multiple variants**

::: todo
Add more complex examples using a sequence of variants.
:::

### Read (Resolve)

To resolve a `did:ng`, the [`resolve()` function](https://www.w3.org/TR/did-resolution/#resolving) of the
W3C [[spec: DID-RESOLUTION]] specification is used.

::: note Nextgraph documents
A DID document is not the same as a Nextgraph document.
For more information about Nextgraph documents, see https://docs.nextgraph.org/en/documents/.
:::

The following rules apply when resolving a `did:ng` DID to a DID document:

1. The value of the `@context` property MUST be a list containing the single string `https://www.w3.org/ns/did/v1.1`.
1. The value of the `id` property MUST be the DID that is being resolved.
1. The `controller` property MUST NOT be present.
1. Add a `type` property with a string value corresponding to the table of common variants in [Method-Specific Identifier](#method-specific-identifier).
1. Parse the DID using the ABNF notation specified in [Method-Specific Identifier](#method-specific-identifier) to
   determine the list of tuples, each consisting of a **variant identifier** and **variant-specific data**.
1. For the first tuple in the list, use the table of common variants in [Method-Specific Identifier](#method-specific-identifier)
   to determine if the **variant-specific data** is a public key.
   1. If yes,
      1. Add a [verification method](https://www.w3.org/TR/cid-1.0/#verification-methods) to the DID document.
         1. The value of the `id` property MUST be the DID that is being resolved followed by the string `#key-1`.
         1. The value of the `type` property MUST be the string `Multikey`.
         1. The value of the `controller` property MUST be the DID that is being resolved.
         1. The value of the `publicKeyMultibase` property MUST be the **variant-specific data**,
            encoded using [[multicodec]].
      1. Add a [verification relationship](https://www.w3.org/TR/cid-1.0/#verification-relationships) to the DID document.
         1. The verification relationship MUST be `assertionMethod`.
         1. The verification relationship MUST reference the `#key-1` verification method.
      1. If the **variant identifier** equals `i`, add an additional [verification relationship](https://www.w3.org/TR/cid-1.0/#verification-relationships) to the DID document.
         1. The verification relationship MUST be `authentication`.
         1. The verification relationship MUST reference the `#key-1` verification method.

::: todo
Add more information about the value of the `type` property, and potentially additional
properties to be added to the DID document depending on the variant.
:::

::: todo
Fix reference to multibase/multicodec spec, and provide more details.
:::

::: note Identifier Semantics
DIDs using the `did:ng` method are not only used for discovering cryptographic verification
methods and other technical metadata about a subject, they also directly identify
specific types of resources in graph, and they carry semantics about these resources within
the identifier itself. During resolution, this is reflected by including a `type` property
in the resolved DID document.
:::

#### Examples

**Example 1: DID with `did:ng:a` variant identifier:**

```
did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA
```

Resolved DID document:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1.1",
  ],
  "id": "did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA",
  "type": "ProtectedStore",
  "verificationMethod": [
    {
      "id": "did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA#key-1",
      "type": "Multikey",
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

::: todo
Update above example to contain correct `publicKeyMultibase` value
that actually corresponds to the method-specific identifier.
:::

**Example 2: DID with `did:ng:o` variant identifier:** (note: newlines are for display purposes only, the DID is one continuous string)

```
did:ng:o:t9BlAm27i3IPG0a7GQziVtoInABr96vzjBrk2db_r1QA
    :v:2oNpTmCKXjK45kmPrgUPv0FVsL5avs92vUzYqZBb6LcA
    :c:CiA412QJHo8VnRi3TGcmdO_WUSPeLy7Se_Irj2ymhPoA
    :k:BIKTgk0WoYR6zyDWWw0770b9WdKY3zuqdWQcsyVmgMAA
    :s:-Q_t0a_p3v6hoMCu3fn83FAUD14w04xPEYnSf3Fv8hQA
    :k:ozwE0_R_ekcxY_eYKUSijbyTiisDfnORJelFq_OsK0IA
    :l:AAEBBAxuZXh0Z3JhcGguZXUAAAAAAAuPyIt2ThWdpC8Dlo6MnE4e3hWE_2StcUM1_wCt_pktAA
```

Resolved DID document:

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
