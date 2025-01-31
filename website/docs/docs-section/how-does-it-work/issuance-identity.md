---
title: Issuance Identity
sidebar_label: Issuance Identity
---

OpenAttestation uses the Domain Name System (DNS) as the method of issuer identity verification. A one-liner introduction to the DNS system can be summarised as: "Phonebook for the Internet". It's primary purpose is to resolve human readable names such as "google.com", or "openattestation.com", etc. to a set of records. The most common records are 'A records', which resolve to IP addresses - this allows network routing to operate over the Internet.

For OpenAttestation, we are using the TXT type of record, which simply allows us to store textual data. The textual data we store indicates the Document Store that the domain administrator trusts.

By allowing the DNS system to be used as an identity registry, we let domain name owners claim ownership of an OpenAttestation Document Store smart contract on the Ethereum Blockchain.

## Rationale

The DNS system is a key part of Internet infrastructure, and is a decentralised system - this means that there is a low barrier to entry and does not have a single point of failure. It allows issuers to simply tie their issuance to their domain name, (e.g example.openattestation.com). When a user views a document issued under this model, they will see "Document issued by example.openattestation.com".

## How it works

Under [IETF RFC 1464](https://tools.ietf.org/html/rfc1464), it is possible to store arbitrary string attributes as part of a domain's record set. This method is currently widely used for email server authentication (SPF, DMARC, DKIM). Our DNS identity proof technique was largely inspired by [Keybase DNS proofs](https://github.com/keybase/keybase-issues/issues/367).

Only domain name owners (and the registrar that they trust) have the authority to make changes to the records associated with that domain name. Thus when a DNS record endorses a certain fact, it transitively asserts that this fact is believed to be true by the domain name owner.

In an OpenAttestation DNS-TXT identity proof, we record a Document Store address and the network (e.g Ethereum, Main Net) it is on. In the OpenAttestation document itself, we declare the domain name to search for the record as well as the Document Store Ethereum address. This forms a bi-directional trust assertion, and if the Document's cryptographic proof is issued on that Document Store - we can say that the domain name owner has endorsed the issuance of this document.

A deeper technical discussion of this topic can be found at [OpenAttestation's Decentralised Identity Proof using DNS-TXT Architecture Decision Record](https://github.com/Open-Attestation/adr/blob/master/decentralized_identity_proof_DNS-TXT.md).

## How to create DNS TXT Record

Every OA document's provenance can be verified and traced back to its creator or issuer. This is achieved by embedding an `identityProof` property in the document, which serves as a claim for identity. During the verification phase, the claim is checked against external records.

In this section, we will bind the document issuer's identity to a valid domain name. This domain will be displayed as issuer every time the document is rendered in an OA-compliant decentralized renderer.

## Prerequisites

- Domain name
- Edit access to your domain's DNS records
- A document store

To bind the domain name to the issuer's identity, you must be able to change the DNS record of the domain name.

## Inserting the DNS Record for Ethereum Smart contracts

You will need to add a DNS `TXT` record to your domain name. The exact steps to achieve this can be confirmed with your domain registrar, this is usually achieved through your domain registrar or DNS provider's web UI.

While we have provided [links to guides](#additional-note-for-adding-dns-txt-records) on adding DNS `TXT` records for some common domain registrars and DNS providers, the steps below is a generic procedure for any DNS provider.

Select a domain name that you will like to associate with your documents. The domain can either be the root domain (e.g. `openattestation.com`) or a subdomain (e.g. `issuer.openattestation.com`). Using the root domain is recommended as it will be easier for viewers of your documents to recognize visually.

Within your domain registrar or DNS provider's web UI, insert a `TXT` record into the DNS in the following format:

| Type | Name        | Value                                                           |
| ---- | ----------- | --------------------------------------------------------------- |
| TXT  | example.com | "openatts net=ethereum netId=3 addr=`<DOCUMENT_STORE_ADDRESS>`" |

The `<DOCUMENT_STORE_ADDRESS>` in the `Value` field above is the document store smart contract address obtained. Please note that the document store address needs to be prepended with `addr`.

> The quotes around the value are necessary. They are used to delimit each different records that you might have to be bound to the same domain.

An example of a valid DNS `TXT` record is as shown:

| Type | Name                     | Value                                                                           |
| ---- | ------------------------ | ------------------------------------------------------------------------------- |
| TXT  | demo.openattestation.com | "openatts net=ethereum netId=3 addr=0xED2E50434Ac3623bAD763a35213DAD79b43208E4" |

The `netId` corresponds to the [network ID for the different Ethereum networks](https://chainid.network/). We generally use only the following networks:

| Network ID | Name                     | Network   |
| ---------- | ------------------------ | --------- |
| `1`        | Ethereum Mainnet         | `mainnet` |
| `5`        | Ethereum Testnet Goerli  | `goerli`  |
| `11155111` | Ethereum Testnet Sepolia | `sepolia` |

For more information on switching to production mode, refer to the [Additional Note for Identity Proof in Production](#additional-note-for-identity-proof-in-production) section below.

### Testing the DNS Record

![Google DNS to Test](/docs/developer-section/quickstart/configuring-dns/google-dns.png)

> The DNS propagation should take a few minutes, though in some cases you may need to wait up to 24 hours. Continue with the other parts of the guide while waiting for DNS to propagate.

After adding the `TXT` record, we recommend you to check that the record has been inserted correctly by viewing with [Google DNS](https://dns.google.com/). Make sure to select `TXT` in the _RR Type_ dropdown.

### Additional Note for Identity Proof in Production

The `TXT` record above is for use for documents issued on the Ethereum `goerli` network. To bind the identity in production where your documents are issued in the Ethereum `mainnet` network, you will have to change `netId` to `1`.

An example of a valid `TXT` record for Ethereum `mainnet` network is as shown:

| Type | Name                     | Value                                                                           |
| ---- | ------------------------ | ------------------------------------------------------------------------------- |
| TXT  | demo.openattestation.com | "openatts net=ethereum netId=1 addr=0x9db35C07350e9a16C828dAda37fd9c2923c75812" |

## Inserting the DNS Record for DID

This is very similar to Ethereum Smart Contracts. Only the shape of the data change.
Within your domain registrar or DNS provider's web UI, insert a `TXT` record into the DNS in the following format:

| Type | Name        | Value                                   |
| ---- | ----------- | --------------------------------------- |
| TXT  | example.com | "openatts a=dns-did; p=`<DID>`; v=1.0;" |

The `<DID>` in the `Value` field above is DID public key id, as resolved by your DID. For instance, check [this DID](https://dev.uniresolver.io/1.0/identifiers/did:ethr:0xaCc51f664D647C9928196c4e33D46fd98FDaA91D). The expected value is `did:ethr:0xaCc51f664D647C9928196c4e33D46fd98FDaA91D#controller`, similar to `didDocument.publicKey[0].id`.

## Additional Note for Adding DNS `TXT` Records

Below is a list of guides provided by some of the common domain registrars and DNS providers. This list is by no means comprehensive.

- [Amazon Route 53](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/dns-txt-records.html)
- [Cloudflare](https://support.cloudflare.com/hc/en-us/articles/360019093151-Managing-DNS-records-in-Cloudflare)
- [Name.com](https://www.name.com/support/articles/115004972547-Adding-a-TXT-Record)
- [Namecheap](https://www.namecheap.com/support/knowledgebase/article.aspx/317/2237/how-do-i-add-txtspfdkimdmarc-records-for-my-domain)
- [GoDaddy](https://sg.godaddy.com/help/add-a-txt-record-19232)
