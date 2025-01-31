---
id: issuing-document
title: Issuing Documents
sidebar_label: Issuing Documents
---

After wrapping the documents and obtaining a merkle root, the documents are ready to be issued on the document store smart contract. To issue a batch of documents, we will use the merkle root that will be appended to the list of issued documents on the document store. This issuance only needs to be done once for all documents in a batch.

## Issuing the documents

```bash
open-attestation document-store issue --address 0xBBb55Bd1D709955241CAaCb327A765e2b6D69c8b --hash 0x80cc53b77c0539fc383f8d434ac5ffad281f3d64ae5a0e59e9f36f19548e1fff  --network goerli --encrypted-wallet-path wallet.json
```

In the example above:

- `address` is the document store address, for instance the one created with the [Deploying Document Store](/docs/integrator-section/verifiable-document/ethereum/document-store) guide.
- `hash` is the merkle root hash, for instance generated while [Wrapping Documents](/docs/integrator-section/verifiable-document/ethereum/wrapping-document)

You will be prompted for the password that you used while creating the wallet. You will see a message after completion of the command:

```text
✔  success   Document/Document Batch with hash 0x80cc53b77c0539fc383f8d434ac5ffad281f3d64ae5a0e59e9f36f19548e1fff has been issued on 0xBBb55Bd1D709955241CAaCb327A765e2b6D69c8b
```

## Verifying the documents

Head to `dev.opencerts.io` or `dev.tradetrust.io` and drag and drop one of the wrapped documents. The document will be verified, then displayed.

![Successful verification](/docs/integrator-section/verifiable-document/ethereum/issuing-document/verifying.png)

🎉 Congratulations, you have completed the getting started guide to create your own Verifiable Document!
