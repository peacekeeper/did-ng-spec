## DID Parameters

The `did:ng` method specification defines a number of [DID parameters](https://www.w3.org/TR/did-resolution/#did-parameters)
that can be used to construct DID URLs.

These parameters are meant to be used in situations when additional information is needed for resolving a DID to a
DID document, but that additional information does not change the DID subject that is identified by the DID.

Note that in some versions of Nextgraph, the information expressed by these parameters is included in the DID itself
in the form of **variant identifiers** and **variant-specific data** (see [Method-Specific Identifier](#method-specific-identifier)).

List of supported DID parameters:

| DID Parameter | Parameter Value | Meaning         |
|--------------------|--------------|-----------------|
| `?l=`          | [locator]    | Locator         |
| `?y=`          | [topic-id]    | Topic         |
| `?k=`          | [commit-key]    | Key         |
| `?r=`          | [read-cap]    | Read Capability         |
| `?n=`          | [read-token]    | Read Token         |
| `?s=`          | [perma-cap] | Permanent Capability         |

Example DID using only variant identifiers (note: newlines are for display purposes only, the DID is one continuous string):

```
did:ng:o:t9BlAm27i3IPG0a7GQziVtoInABr96vzjBrk2db_r1QA
    :v:2oNpTmCKXjK45kmPrgUPv0FVsL5avs92vUzYqZBb6LcA
    :c:CiA412QJHo8VnRi3TGcmdO_WUSPeLy7Se_Irj2ymhPoA
    :k:BIKTgk0WoYR6zyDWWw0770b9WdKY3zuqdWQcsyVmgMAA
    :s:-Q_t0a_p3v6hoMCu3fn83FAUD14w04xPEYnSf3Fv8hQA
    :k:ozwE0_R_ekcxY_eYKUSijbyTiisDfnORJelFq_OsK0IA
    :l:AAEBBAxuZXh0Z3JhcGguZXUAAAAAAAuPyIt2ThWdpC8Dlo6MnE4e3hWE_2StcUM1_wCt_pktAA
```

Example DID using a combination of variant identifiers and DID parameters:

```
did:ng:o:t9BlAm27i3IPG0a7GQziVtoInABr96vzjBrk2db_r1QA
    :v:2oNpTmCKXjK45kmPrgUPv0FVsL5avs92vUzYqZBb6LcA
    :c:CiA412QJHo8VnRi3TGcmdO_WUSPeLy7Se_Irj2ymhPoA
    ?k=BIKTgk0WoYR6zyDWWw0770b9WdKY3zuqdWQcsyVmgMAA
    &s=-Q_t0a_p3v6hoMCu3fn83FAUD14w04xPEYnSf3Fv8hQA
    &k=ozwE0_R_ekcxY_eYKUSijbyTiisDfnORJelFq_OsK0IA
    &l=AAEBBAxuZXh0Z3JhcGguZXUAAAAAAAuPyIt2ThWdpC8Dlo6MnE4e3hWE_2StcUM1_wCt_pktAA
```

Note that in the second example, if the values of `k`, `s`, `k`, `l` change,
the DID itself does not change, which indicates that the concept or object
identified by the DID is still the same, only the information needed to access
it has changed.
