---
title: Revoking Documents (Document Store)
sidebar_label: Revoking Documents (Document Store)
---

It's possible to revoke a signed document only if you fulfill certain **prerequisites**. Any future progress can be followed [here](https://github.com/Open-Attestation/adr/blob/master/issuing_using_did.md#for-documents-that-are-signed-directly).

## Prerequisites:

- an `ethr` DID (if you've been following the above steps, you should have one already)
- some kind of DID documents:
  1. base DID document (`ISSUER_IDENTITY`: `identityProof.identifier` against `ethr` DID)
  2. DID-DNS document (`ISSUER_IDENTITY`: `identityProof.identifier` against a [DNS-TXT](/docs/docs-section/how-does-it-work/issuance-identity))
- a deployed `documentStore`, click [here](/docs/integrator-section/verifiable-document/ethereum/document-store) for how to do so.
- before you wrap your document with the wrapper SDK, add a `revocation` block with these keys:

```json
{
  "$template": {
    "name": "main",
    "type": "EMBEDDED_RENDERER",
    "url": "https://tutorial-renderer.openattestation.com"
  },
  "recipient": {
    "name": "John Doe"
  },
  "issuers": [
    {
      ....
      "name": "Demo Issuer",
      "revocation": {
        "type": "REVOCATION_STORE",
        "location": "<DEPLOYED_DOCUMENT_STORE_LOCATION>"
      },
      ...
    }
  ]
}
```

<!-- TBD v3 document sample when that releases -->

- note that the `<DEPLOYED_DOCUMENT_STORE_LOCATION>` is your `documentStore` location, it must be **deployed** on the ethereum block chain (for now. Further implementations will relax this requirement so that DID documents will truly be gasless).
- wrap your document, sign it and appending the signature to the document (follow the tutorial)

## Revoking a document:

- once you have achieved the prerequisites, you would have a document that could be revoked on a deployed `documentStore`.
- how one does this is by simply invoking the command to revoke a document from the ethereum flow (use the cli):

```bash
open-attestation document-store revoke --address <DOCUMENT_STORE_LOCATION> --hash <HASH_OF_DOC(S)>  --network <NETWORK> --encrypted-wallet-path <PATH_OF_WALLET>
```

## Misc questions:

Q: hey you [mentioned](/docs/docs-section/how-does-it-work/comparison#price) that if I use DID documents, I would not need to pay for transactions, but following this flow, I would still have to pay for at least 1 transaction (deploying a `documentStore`), what gives?

A: yes, you are right, for now this implementation will still need at least 1 transaction to the ethereum blockchain. We are working on this so please be patient and watch this space.`

Q: this might be a weird question but I did not issue any documents from the deployed `documentStore`, how am I able to revoke this document from said `documentStore` when in the first place, I did not even issue anything?

A: long story short, the revocation mapping in the `documentStore` is a separate mapping from the issued mapping.
