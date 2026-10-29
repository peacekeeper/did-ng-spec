## DID Method Operations

### Create (Register)

#### Creating a `did:ng`

To create a `did:ng`, the controller first generates a private/public key pair that controls the DID.
The public key is included as a verification method in the initial version of the DID document.

DIDs are usually created implicitly, e.g. when creating a new document.

**Example initial DID document:**

```json
{
   "@context": [
      "https://www.w3.org/ns/did/v1.1"
   ],
   "id": "did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA",
   "verificationMethod": [{
   }]
}
```

**Example DID:**

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

**Example Request to resolve a DID over HTTPS binding, with `did:ng:i` variant identifier:**

```
HTTP GET to https://uniresolver.io/1.0/identifiers/did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA
```

**Example Response:**

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/ed25519-2020/v1"
  ],
  "id": "did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA",
  "type": "ProtectedGroup",
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

**Example Request to resolve a DID over HTTPS binding, with `did:ng:o` variant identifier:**

```
HTTP GET to https://uniresolver.io/1.0/identifiers/did:ng:o:t9BlAm27i3IPG0a7GQziVtoInABr96vzjBrk2db_r1QA:v:2oNpTmCKXjK45kmPrgUPv0FVsL5avs92vUzYqZBb6LcA:c:CiA412QJHo8VnRi3TGcmdO_WUSPeLy7Se_Irj2ymhPoA:k:BIKTgk0WoYR6zyDWWw0770b9WdKY3zuqdWQcsyVmgMAA:s:-Q_t0a_p3v6hoMCu3fn83FAUD14w04xPEYnSf3Fv8hQA:k:ozwE0_R_ekcxY_eYKUSijbyTiisDfnORJelFq_OsK0IA:l:AAEBBAxuZXh0Z3JhcGguZXUAAAAAAAuPyIt2ThWdpC8Dlo6MnE4e3hWE_2StcUM1_wCt_pktAA
```

**Example Response:**

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
