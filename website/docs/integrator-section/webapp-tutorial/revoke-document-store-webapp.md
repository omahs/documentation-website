---
id: revoke-document-store-webapp
title: Revoke Document with Web app
sidebar_label: Revoke Document Store
---

This guide will help you to revoke a hash using one of the available web application. You will find more details about revoking on the [main guide page](/docs/integrator-section/verifiable-document/ethereum//revoking-document)

Currently, there exists 2 web application:

- [OpenCerts Admin Portal](https://admin.opencerts.io/)
- [TradeTrust Admin Portal](https://admin.tradetrust.io/)

## Prerequisites

- Google Chrome web browser
- Metamask setup
- A document store
- A merkle root

## Issuing the document

![Revoking Interface](/docs/integrator-section/webapp-tutorial/revoking-webapp/revoking.png)

After connecting Metamask, you will be logged into the web application. First, you need to enter the `document store smart contract address` from the [previous guide](/docs/integrator-section/webapp-tutorial/issue-document-store-webapp/) into the "Store address" field. Then, select "Revoke Document Batch" on the navigation on the left.

Paste the value of `merkleRoot` from the document into the field and click on "Revoke". You will need to confirm your transaction on Metamask.

![Revoke Success](/docs/integrator-section/webapp-tutorial/revoking-webapp/success.png)

Once the merkle root has been issued, you will see the success message "Revoked document batch.".
