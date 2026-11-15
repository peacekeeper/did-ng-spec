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
**variant identifier** (`variant-id`), separated by a `:` colon character, and followed by **variant-specific
data** (`variant-data`). In some cases, this pair of variant identifier plus variant-specific data can
be repeated to form a sequence, where a subsequent pair of variant identifier and
variant-specific data refines or qualifies the previous pair.

The **variant identifier** is always a single lower-case character.

The **variant-specific data** depends on the variant identifier. It often consists of
32 bytes, prefixed by a single byte detailing the version number of the variant,
which is zero at the moment. The byte array is reversed and encoded as
base64-url strings.

The syntax is specified in two ways, using the Augmented Backus-Naur Form (ABNF) notation
[[spec:rfc2234]].

The first notation is a simplified syntax that expresses the concept of variants on a
high level, but leaves parsing and processing of specific variants to the application.
This ABNF notation is expected to be stable in the long term, even when variants are added
over time:

```abnf
ng-did = "did:ng:" variant-id ":" variant-data *( ":" variant-id ":" variant-data )
variant-id = "a" / "b" / "c" / "g" / "o" / ...
variant-data = versioned-byte-array
versioned-byte-array = 1*base64url-char
base64url-char  = ALPHA / DIGIT / "-" / "_"
```

The following table lists common variants of DIDs:

| Variant Identifier | Variant-Specific Data | Meaning         |
|--------------------|---------------------|----------------------------|
| `i`| [user-id]    | User Identity   |
| `o`| [repo-id]    | Document        |
| `a`| [repo-id]    | Public Store    |
| `b`| [repo-id]    | Protected Store |
| `c`| [repo-id]    | Private Store   |
| `g`| [repo-id]    | Group Store     |
| `j`| [object-id]    | Binary file        |
| `c`| [commit-id]    | Commit         |
| `v`| [overlay-id] | Overlay         |
| `l`| [locator]    | Locator         |
| `y`| [topic-id]    | Topic         |
| `k`| [key]    | Key         |
| `r`| [read-cap]    | Read Capability         |
| `n`| [read-token]    | Read Token         |
| `s`| [perma-cap] | Permanent Capability         |

::: note Variant Semantics
The variant identifier not only indicates the content of the variant-specific data
and subsequent structure of the identifier, but also carries semantics about the resource
that is being identified. For example, a DID starting with `:g:` always
identifies a Group Store.
This is unusual for DID methods, but fully conformant with the W3C DID Core standard.
:::

::: note Different Meanings of Variants
In a sequence of variants, variant identifiers may have different meanings depending
on where in the sequence they appear. For example, `:c:` can mean "Private Store", but also
"Commit".
:::

A second ABNF notation is provided which is an expanded version of the first ABNF notation
and expresses details of all currently known variants:

```abnf
ng-did = "did:ng:" variant *( ":" variant )
variant = variant-i / variant-o / variant-a / variant-b / variant-c / variant-g / variant-j /
          variant-v / variant-l / variant-y / variant-k / variant-r / variant-n / variant-s
variant-i = "i" ":" user-id
variant-o = "o" ":" repo-id
variant-a = "a" ":" repo-id
variant-b = "b" ":" repo-id
variant-c = "c" ":" ( repo-id / commit-id ) 
variant-g = "g" ":" repo-id
variant-j = "j" ":" object-id
variant-v = "v" ":" overlay-id
variant-l = "l" ":" locator
variant-y = "y" ":" topic-id
variant-k = "k" ":" commit-key
variant-r = "r" ":" read-cap
variant-n = "n" ":" read-token
variant-s = "s" ":" perma-cap
user-id = pubkey
repo-id = pubkey
commit-id = digest
object-id = digest
overlay-id = digest
locator = byte-array
topic-id = digest
commit-key = symkey
read-cap = pubkey-and-digest
read-token = digest
permap-cap = byte-array
pubkey = 44base64url-char
symkey = 44base64url-char
digest = 44base64url-char
pubkey-and-digest = 87base64url-char
byte-array = 1*base64url-char
base64url-char  = ALPHA / DIGIT / "-" / "_"
```

Note that some variants can also be expressed as DID parameters rather than
as part of the DID itself (see [DID Parameters](#did-parameters)).

**Examples of `did:ng`:**

DID identifying a public store (`:a:`):

```
did:ng:a:rjoQTS4LMBDcuh8CEjmTYrgALeApBg2cgKqyPEuQDUgA
```

DID identifying a document (`:o:`) with an overlay (`:v:`):

```
did:ng:o:j7sAsZNRibsTWEcLtMpYBBGwMAwkmjNNzCnuvtt7AkUA:v:4gBmGObzB_N3Bwieous7TP1DyXAelpK6aauVyxf4xcMA
```

DID combining multiple variants (note: newlines are for display purposes only, the DID is one continuous string):

```
did:ng:o:t9BlAm27i3IPG0a7GQziVtoInABr96vzjBrk2db_r1QA
    :v:2oNpTmCKXjK45kmPrgUPv0FVsL5avs92vUzYqZBb6LcA
    :c:CiA412QJHo8VnRi3TGcmdO_WUSPeLy7Se_Irj2ymhPoA
    :k:BIKTgk0WoYR6zyDWWw0770b9WdKY3zuqdWQcsyVmgMAA
    :s:-Q_t0a_p3v6hoMCu3fn83FAUD14w04xPEYnSf3Fv8hQA
    :k:ozwE0_R_ekcxY_eYKUSijbyTiisDfnORJelFq_OsK0IA
    :l:AAEBBAxuZXh0Z3JhcGguZXUAAAAAAAuPyIt2ThWdpC8Dlo6MnE4e3hWE_2StcUM1_wCt_pktAA
```
