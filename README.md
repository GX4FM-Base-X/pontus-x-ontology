# Pontus-X Ontology based on Ocean Protocol and Gaia-X

Guide to the ontology of the metadata in Pontus-X, based on Ocean Protocol. For details follow the original [Ocean Protocol documentation under docs.oceanprotocol.com](https://docs.oceanprotocol.com) and [docs.pontus-x.eu](https://docs.pontus-x.eu/).

**Ocean Aquarius**, the metadata cache of Ocean Protocol, is an off-chain, multi-chain cache for metadata that is published on chain, connected to an Elasticsearch database. Aquarius continually monitors the chains for MetadataCreated and MetadataUpdated events, processes these events and adds them to the database. The Aquarius API offers a convenient way to access the medatata without scanning the chain yourself. 
Ocean Protocol metadata verification is also located here. 
Repository: [https://github.com/oceanprotocol/aquarius/tree/main](https://github.com/oceanprotocol/aquarius/tree/main)

**Ocean DID Documentation**, can be found here: [https://docs.oceanprotocol.com/developers/identifiers](https://docs.oceanprotocol.com/developers/identifiers)

**Ocean DDO Documentation**, can be found here: [https://docs.oceanprotocol.com/developers/ddo-specification](https://docs.oceanprotocol.com/developers/ddo-specification)

For Reference and Comparison also the **[Gaia-X Ontology and Shapes](https://gitlab.com/gaia-x/lab/compliance/gx-registry/-/tree/development/src/static/shapes)** have been added. They are provided under [Eclipse Public License - v 2.0](https://gitlab.com/gaia-x/lab/compliance/gx-registry/-/blob/development/LICENSE).

Since [Pontus-X](https://www.pontus-x.eu/) is building on the Ethereum ecosystem, wie also use the **[Ethereum Ontology](https://ethon.consensys.io/index.html)** with regard to participant identification, ERC20 tokens and smart contract implementations.

# Metadata

Metadata plays a **crucial role** in asset **discovery**, providing essential information such as **asset type, name, creation date, and licensing details**. Each data asset can have a decentralized identifier (DID) that resolves to a DID document (DDO) containing associated metadata. In Pontus-X two DDOs exists for each DID, one Ocean Protocol specific DDO and one Gaia-X specific service credential. The DDO is essentially a collection of fields in a [JSON](https://www.json.org/) object. To understand working with Ocean Protocol DIDs, you can refer to the DID documentation (below). For a more comprehensive understanding of metadata structure, the DDO specification documentation (below) provides in-depth information.

In general, any service within the Ocean and Pontus-X ecosystem is required to store metadata for every listed dataset. The metadata is useful to determine which services are relevant.

So, for example, imagine you're searching for data on Spanish almond production in an Ocean-powered data space. You might find a large number of datasets, making it difficult to identify the most relevant one. What can we do about it? :thinking: This is where metadata is useful! The metadata provides valuable information that helps you identify the most relevant dataset. This information can include:

* **name**, e.g. “Largueta Almond Production: 1995 to 2005”
* **dateCreated**, e.g. “2007–01–20”
* **datePublished**, e.g. “2022–11–10T12:32:15Z”
* **author**, e.g. “Spanish Almond Board”
* **license**, e.g. “SAB Data License”
* technical information about the **files**, such as the content type.

Other metadata might also be available. For example:

* **categories**, e.g. \[“agriculture”, “economics”]
* **tags**, e.g. \[“Europe”, “Italy”, “nuts”, “almonds”]
* **description**, e.g. “2002 Italian almond production statistics for 14 varieties and 20 regions.”
* **additionalInformation** can be used to store any other facts about the asset.


### **Overview for DIDs and DDOs**

DIDs and DDOs follow the [specification defined by the World Wide Web Consortium (W3C)](https://w3c-ccg.github.io/did-spec/).

[**Decentralized identifiers**](identifiers.md) (DIDs) are a type of identifier that enable verifiable, decentralized digital identity. Each DID is associated with a unique entity, and DIDs may represent humans, objects, and more. A **DID Document** (DDO) is a JSON blob that holds information about the DID. Given a DID, a _resolver_ will return the DDO of that DID.

Decentralized identifiers (DIDs) are a type of identifier that enable verifiable, decentralized digital identity. Each DID is associated with a unique entity, and DIDs may represent humans, objects, and more.

#### Rules for DID & DDO

An _asset_ in Ocean represents a downloadable file, compute service, or similar. Each asset is a _resource_ under the control of a _publisher_. The Ocean network itself does _not_ store the actual resource (e.g. files).

An _asset_ has a DID and DDO. The DDO should include metadata about the asset, and define access in at least one [service](ddo-specification.md#services). Only _owners_ or _delegated users_ can modify the DDO.

All DDOs are stored on-chain in encrypted form to be fully GDPR-compatible. A metadata cache like [_Aquarius_](aquarius/README.md) can help in reading, decrypting, and searching through encrypted DDO data from the chain. Because the file URLs are encrypted on top of the full DDO encryption, returning unencrypted DDOs e.g. via an API is safe to do as the file URLs will still stay encrypted.




# Identifiers (DIDs)

### Identifiers

We use decentralized identifiers (DIDs) to identify assets within the network. Decentralized identifiers (DIDs) are a type of identifier that enables verifiable, decentralized digital identity. In contrast to typical, centralized identifiers, DIDs have been designed so that they may be decoupled from centralized registries, identity providers, and certificate authorities. Specifically, while other parties might be used to help enable the discovery of information related to a DID, the design enables the controller of a DID to prove control over it without requiring permission from any other party. DIDs are URIs that associate a DID subject with a DID document allowing trustable interactions associated with that subject.

### Examples

DIDs in Ocean follow [the generic DID scheme](https://w3c-ccg.github.io/did-spec/#the-generic-did-scheme), they look like this:

```
did:op:0ebed8226ada17fde24b6bf2b95d27f8f05fcce09139ff5cec31f6d81a7cd2ea
```

### Examples

DIDs in Ocean follow [the generic DID scheme](https://w3c-ccg.github.io/did-spec/#the-generic-did-scheme), they look like this:

```
did:op:0ebed8226ada17fde24b6bf2b95d27f8f05fcce09139ff5cec31f6d81a7cd2ea
```

The part after `did:op:` is the ERC721 contract address(in checksum format) and the chainId (expressed to 10 decimal places). The following javascript example shows how to calculate the DID for the asset:

```runkit  nodeVersion="18.x.x"
const CryptoJS = require('crypto-js')

const dataNftAddress = '0xa331155197F70e5e1EA0CC2A1f9ddB1D49A9C1De'
const chainId = 1
const checksum = CryptoJS.SHA256(dataNftAddress + chainId.toString(10))
const did = 'did:op:' + checksum

console.log(did)

```

# DDO Specification

### DDO Schema - High Level

The below diagram shows the high-level DDO schema depicting the content of each data structure and the relations between them.

Please note that some data structures apply only on certain types of services or assets.


```mermaid
---
title: DDO High Level Diagram
---
classDiagram

    class DDO{
    }

    class Metadata{
    }
    class Credentials{
    }

    class AlgorithmMetadata["AlgorithmMetadata\n(for algorithm type)"] {
    }
  
    class Container{
    }
    class Service{
    }
    class ConsumerParameters["Consumer\nParameters"]{
    }
   class Compute{
    }
DDO "1" --> "1" Metadata
DDO "1" --> "0..n" Credentials
DDO "1" --> "1..*" Service

Metadata "1" --> "0..1" AlgorithmMetadata
AlgorithmMetadata "1" --> "1..*" Container
AlgorithmMetadata "1" --> "1..*" ConsumerParameters

Service "1" --> "0..n" Compute
Service "1" --> "0..n" ConsumerParameters
```

### Required Attributes

A DDO in Ocean has these required attributes:

<table><thead><tr><th width="169.6737288135593">Attribute</th><th width="164">Type</th><th>Description</th></tr></thead><tbody><tr><td><strong><code>@context</code></strong></td><td>Array of <code>string</code></td><td>Contexts used for validation.</td></tr><tr><td><strong><code>id</code></strong></td><td><code>string</code></td><td>Computed as <code>sha256(address of ERC721 contract + chainId)</code>.</td></tr><tr><td><strong><code>version</code></strong></td><td><code>string</code></td><td>Version information in <a href="https://semver.org">SemVer</a> notation referring to this DDO spec version, like <code>4.1.0</code>.</td></tr><tr><td><strong><code>chainId</code></strong></td><td><code>number</code></td><td>Stores the chainId of the network the DDO was published to.</td></tr><tr><td><strong><code>nftAddress</code></strong></td><td><code>string</code></td><td>NFT contract linked to this asset</td></tr><tr><td><strong><code>metadata</code></strong></td><td><a href="ddo-specification.md#metadata">Metadata</a></td><td>Stores an object describing the asset.</td></tr><tr><td><strong><code>services</code></strong></td><td><a href="ddo-specification.md#services">Services</a></td><td>Stores an array of services defining access to the asset.</td></tr><tr><td><strong><code>credentials</code></strong></td><td><a href="ddo-specification.md#credentials">Credentials</a></td><td>Describes the credentials needed to access a dataset in addition to the <code>services</code> definition.</td></tr></tbody></table>

### Metadata

This object holds information describing the actual asset.

<table><thead><tr><th width="260.3333333333333">Attribute</th><th width="166">Type</th><th>Description</th></tr></thead><tbody><tr><td><strong><code>created</code></strong></td><td><code>ISO date/time string</code></td><td>Contains the date of the creation of the dataset content in ISO 8601 format preferably with timezone designators, e.g. <code>2000-10-31T01:30:00Z</code>.</td></tr><tr><td><strong><code>updated</code></strong></td><td><code>ISO date/time string</code></td><td>Contains the date of last update of the dataset content in ISO 8601 format preferably with timezone designators, e.g. <code>2000-10-31T01:30:00Z</code>.</td></tr><tr><td><strong><code>description</code></strong>*</td><td><code>string</code></td><td>Details of what the resource is. For a dataset, this attribute explains what the data represents and what it can be used for.</td></tr><tr><td><strong><code>copyrightHolder</code></strong></td><td><code>string</code></td><td>The party holding the legal copyright. Empty by default.</td></tr><tr><td><strong><code>name</code></strong>*</td><td><code>string</code></td><td>Descriptive name or title of the asset.</td></tr><tr><td><strong><code>type</code></strong>*</td><td><code>string</code></td><td>Asset type. Includes <code>"dataset"</code> (e.g. csv file), <code>"algorithm"</code> (e.g. Python script). Each type needs a different subset of metadata attributes.</td></tr><tr><td><strong><code>author</code></strong>*</td><td><code>string</code></td><td>Name of the entity generating this data (e.g. Tfl, Disney Corp, etc.).</td></tr><tr><td><strong><code>license</code></strong>*</td><td><code>string</code></td><td>Short name referencing the license of the asset (e.g. Public Domain, CC-0, CC-BY, No License Specified, etc. ). If it's not specified, the following value will be added: "No License Specified".</td></tr><tr><td><strong><code>links</code></strong></td><td>Array of <code>string</code></td><td>Mapping of URL strings for data samples, or links to find out more information. Links may be to either a URL or another asset.</td></tr><tr><td><strong><code>contentLanguage</code></strong></td><td><code>string</code></td><td>The language of the content. Use one of the language codes from the <a href="https://tools.ietf.org/html/bcp47">IETF BCP 47 standard</a></td></tr><tr><td><strong><code>tags</code></strong></td><td>Array of <code>string</code></td><td>Array of keywords or tags used to describe this content. Empty by default.</td></tr><tr><td><strong><code>categories</code></strong></td><td>Array of <code>string</code></td><td>Array of categories associated to the asset. Note: recommended to use <code>tags</code> instead of this.</td></tr><tr><td><strong><code>additionalInformation</code></strong></td><td>Object</td><td>Stores additional information, this is customizable by publisher</td></tr><tr><td><strong><code>algorithm</code></strong>**</td><td><a href="compute-to-data/compute-to-data-algorithms.md#algorithm-metadata">Algorithm Metadata</a></td><td>Information about asset of <code>type</code> <code>algorithm</code></td></tr></tbody></table>

\* Required

\*\* Required for algorithms only

<details>

<summary>Metadata Example</summary>

```json
{
  "metadata": {
    "created": "2020-11-15T12:27:48Z",
    "updated": "2021-05-17T21:58:02Z",
    "description": "Sample description",
    "name": "Sample asset",
    "type": "dataset",
    "author": "OPF",
    "license": "https://market.oceanprotocol.com/terms"
  }
}
```

</details>

#### Services

Services define the access for an asset, and each service is represented by its respective datatoken.

An asset should have at least one service to be actually accessible and can have as many services which make sense for a specific use case.

<table><thead><tr><th width="259.3333333333333">Attribute</th><th width="121">Type</th><th>Description</th></tr></thead><tbody><tr><td><strong><code>id</code></strong>*</td><td><code>string</code></td><td>Unique ID</td></tr><tr><td><strong><code>type</code></strong>*</td><td><code>string</code></td><td>Type of service <code>access</code>, <code>compute</code>, <code>wss</code> etc.</td></tr><tr><td><strong><code>name</code></strong></td><td><code>string</code></td><td>Service friendly name</td></tr><tr><td><strong><code>description</code></strong></td><td><code>string</code></td><td>Service description</td></tr><tr><td><strong><code>datatokenAddress</code></strong>*</td><td><code>string</code></td><td>Datatoken</td></tr><tr><td><strong><code>serviceEndpoint</code></strong>*</td><td><code>string</code></td><td>Provider URL (schema + host)</td></tr><tr><td><strong><code>files</code></strong>*</td><td><a href="#files">Files</a></td><td>Encrypted file.</td></tr><tr><td><strong><code>timeout</code></strong>*</td><td><code>number</code></td><td>Describing how long the service can be used after consumption is initiated. A timeout of <code>0</code> represents no time limit. Expressed in seconds.</td></tr><tr><td><strong><code>compute</code></strong>**</td><td><a href="compute-to-data/compute-options.md">Compute</a></td><td>If service is of <code>type</code> <code>compute</code>, holds information about the compute-related privacy settings &#x26; resources.</td></tr><tr><td><strong><code>consumerParameters</code></strong></td><td><a href="compute-to-data/compute-options.md#consumer-parameters">Consumer Parameters</a></td><td>An object the defines required consumer input before consuming the asset</td></tr><tr><td><strong><code>additionalInformation</code></strong></td><td>Object</td><td>Stores additional information, this is customizable by publisher</td></tr></tbody></table>

\* Required

\*\* Required for compute assets only

#### Files

The `files` field is returned as a `string` which holds the encrypted file URLs.

<details>

<summary>Files Example</summary>

{% code overflow="wrap" %}
```json
{
  "files": "0x044736da6dae39889ff570c34540f24e5e084f4e5bd81eff3691b729c2dd1465ae8292fc721e9d4b1f10f56ce12036c9d149a4dab454b0795bd3ef8b7722c6001e0becdad5caeb2005859642284ef6a546c7ed76f8b350480691f0f6c6dfdda6c1e4d50ee90e83ce3cb3ca0a1a5a2544e10daa6637893f4276bb8d7301eb35306ece50f61ca34dcab550b48181ec81673953d4eaa4b5f19a45c0e9db4cd9729696f16dd05e0edb460623c843a263291ebe757c1eb3435bb529cc19023e0f49db66ef781ca692655992ea2ca7351ac2882bf340c9d9cb523b0cbcd483731dc03f6251597856afa9a68a1e0da698cfc8e81824a69d92b108023666ee35de4a229ad7e1cfa9be9946db2d909735"
}
```
{% endcode %}

</details>

#### Credentials

By default, a consumer can access a resource if they have 1 datatoken. _Credentials_ allow the publisher to optionally specify more fine-grained permissions.

Consider a medical data use case, where only a credentialed EU researcher can legally access a given dataset. Ocean supports this as follows: a consumer can only access the resource if they have 1 datatoken _and_ one of the specified `"allow"` credentials.

This is like going to an R-rated movie, where you can only get in if you show both your movie ticket (datatoken) _and_ some identification showing you're old enough (credential).

Only credentials that can be proven are supported. This includes Ethereum public addresses and in the future [W3C Verifiable Credentials](https://www.w3.org/TR/vc-data-model/) and more.

Ocean also supports `deny` credentials: if a consumer has any of these credentials, they can not access the resource.

Here's an example object with both `allow` and `deny` entries:

<details>

<summary>Credentials Example</summary>

```json
{
  "credentials": {
    "allow": [
      {
        "type": "address",
        "values": ["0x123", "0x456"]
      }
    ],
    "deny": [
      {
        "type": "address",
        "values": ["0x2222", "0x333"]
      }
    ]
  }
}
```

</details>

#### DDO Checksum

In order to ensure the integrity of the DDO, a checksum is computed for each DDO:

```js
const checksum = sha256(JSON.stringify(ddo));
```

The checksum hash is used when publishing/updating metadata using the `setMetaData` function in the ERC721 contract, and is stored in the event generated by the ERC721 contract.

<details>

<summary>MetadataCreated and MetadataUpdated smart contract events</summary>

```solidity
event MetadataCreated(
  address indexed createdBy,
  uint8 state,
  string decryptorUrl,
  bytes flags,
  bytes data,
  bytes metaDataHash,
  uint256 timestamp,
  uint256 blockNumber
);

event MetadataUpdated(
  address indexed updatedBy,
  uint8 state,
  string decryptorUrl,
  bytes flags,
  bytes data,
  bytes metaDataHash,
  uint256 timestamp,
  uint256 blockNumber
);
```

</details>

_Aquarius_ should always verify the checksum after data is decrypted via a _Provider_ API call.

#### State

Each asset has a state, which is held by the NFT contract. The possible states are:

<table><thead><tr><th width="95">State</th><th width="271">Description</th><th width="155">Discoverable in Ocean Market</th><th>Ordering allowed</th><th>Listed under profile</th></tr></thead><tbody><tr><td><strong><code>0</code></strong></td><td>Active</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td><strong><code>1</code></strong></td><td>End-of-life</td><td>Yes</td><td>No</td><td>No</td></tr><tr><td><strong><code>2</code></strong></td><td>Deprecated (by another asset)</td><td>No</td><td>No</td><td>No</td></tr><tr><td><strong><code>3</code></strong></td><td>Revoked by publisher</td><td>No</td><td>No</td><td>No</td></tr><tr><td><strong><code>4</code></strong></td><td>Ordering is temporary disabled</td><td>Yes</td><td>No</td><td>Yes</td></tr><tr><td><strong><code>5</code></strong></td><td>Asset unlisted.</td><td>No</td><td>Yes</td><td>Yes</td></tr></tbody></table>

States details:

1. **Active**: Assets in the "Active" state are fully functional and available for discovery in Ocean Market, and other components. Users can search for, view, and interact with these assets. Ordering is allowed, which means users can place orders to purchase or access the asset's services.
2. **End-of-life**: Assets in the "End-of-life" state remain discoverable but cannot be ordered. This state indicates that the assets are usually deprecated or outdated, and they are no longer actively promoted or maintained.
3. **Deprecated (by another asset)**: This state indicates that another asset has deprecated the current asset. Deprecated assets are not discoverable, and ordering is not allowed. Similar to the "End-of-life" state, deprecated assets are not listed under the owner's profile.
4. **Revoked by publisher**: When an asset is revoked by its publisher, it means that the publisher has explicitly revoked access or ownership rights to the asset. Revoked assets are not discoverable, and ordering is not allowed.
5. **Ordering is temporarily disabled**: Assets in this state are still discoverable, but ordering functionality is temporarily disabled. Users can view the asset and gather information, but they cannot place orders at that moment. However, these assets are still listed under the owner's profile.
6. **Asset unlisted**: Assets in the "Asset unlisted" state are not discoverable. However, users can still place orders for these assets, making them accessible. Unlisted assets are listed under the owner's profile, allowing users to view and access them.

### Aquarius Enhanced DDO Response

The following fields are added by _Aquarius_ in its DDO response for convenience reasons, where an asset returned by _Aquarius_ inherits the DDO fields stored on-chain.

These additional fields are never stored on-chain and are never taken into consideration when [hashing the DDO](ddo-specification.md#ddo-checksum).

#### NFT

The `nft` object contains information about the ERC721 NFT contract which represents the intellectual property of the publisher.

<table><thead><tr><th width="144.70989551321452">Attribute</th><th width="234.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td><strong><code>address</code></strong></td><td><code>string</code></td><td>Contract address of the deployed ERC721 NFT contract.</td></tr><tr><td><strong><code>name</code></strong></td><td><code>string</code></td><td>Name of NFT set in contract.</td></tr><tr><td><strong><code>symbol</code></strong></td><td><code>string</code></td><td>Symbol of NFT set in contract.</td></tr><tr><td><strong><code>owner</code></strong></td><td><code>string</code></td><td>ETH account address of the NFT owner.</td></tr><tr><td><strong><code>state</code></strong></td><td><code>number</code></td><td>State of the asset reflecting the NFT contract value. See <a href="ddo-specification.md#state">State</a></td></tr><tr><td><strong><code>created</code></strong></td><td><code>ISO date/time string</code></td><td>Contains the date of NFT creation.</td></tr><tr><td><strong><code>tokenURI</code></strong></td><td><code>string</code></td><td>tokenURI</td></tr></tbody></table>

<details>

<summary>NFT Object Example</summary>

```json
{
  "nft": {
    "address": "0x000000",
    "name": "Ocean Protocol Asset",
    "symbol": "OCEAN-A",
    "owner": "0x0000000",
    "state": 0,
    "created": "2000-10-31T01:30:00Z"
  }
}
```

</details>

#### Datatokens

The `datatokens` array contains information about the ERC20 datatokens attached to [asset services](ddo-specification.md#services).

<table><thead><tr><th width="160.77255871446232">Attribute</th><th width="128.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td><strong><code>address</code></strong></td><td><code>string</code></td><td>Contract address of the deployed ERC20 contract.</td></tr><tr><td><strong><code>name</code></strong></td><td><code>string</code></td><td>Name of NFT set in contract.</td></tr><tr><td><strong><code>symbol</code></strong></td><td><code>string</code></td><td>Symbol of NFT set in contract.</td></tr><tr><td><strong><code>serviceId</code></strong></td><td><code>string</code></td><td>ID of the service the datatoken is attached to.</td></tr></tbody></table>

<details>

<summary>Datatokens Array Example</summary>

```json
{
  "datatokens": [
    {
      "address": "0x000000",
      "name": "Datatoken 1",
      "symbol": "DT-1",
      "serviceId": "1"
    },
    {
      "address": "0x000001",
      "name": "Datatoken 2",
      "symbol": "DT-2",
      "serviceId": "2"
    }
  ]
}
```

</details>

#### Event

The `event` section contains information about the last transaction that created or updated the DDO.

<details>

<summary>Event Example</summary>

{% code overflow="wrap" %}
```json
{
  "event": {
    "tx": "0x8d127de58509be5dfac600792ad24cc9164921571d168bff2f123c7f1cb4b11c",
    "block": 12831214,
    "from": "0xAcca11dbeD4F863Bb3bC2336D3CE5BAC52aa1f83",
    "contract": "0x1a4b70d8c9DcA47cD6D0Fb3c52BB8634CA1C0Fdf",
    "datetime": "2000-10-31T01:30:00"
  }
}
```
{% endcode %}

</details>

#### Purgatory

Contains information about an asset's purgatory status defined in [`list-purgatory`](https://github.com/oceanprotocol/list-purgatory). Marketplace interfaces are encouraged to prevent certain user actions like adding liquidity on assets in purgatory.

<table><thead><tr><th width="145.92125984251967">Attribute</th><th width="134">Type</th><th>Description</th></tr></thead><tbody><tr><td><strong><code>state</code></strong></td><td><code>boolean</code></td><td>If <code>true</code>, asset is in purgatory.</td></tr><tr><td><strong><code>reason</code></strong></td><td><code>string</code></td><td>If asset is in purgatory, contains the reason for being there as defined in <code>list-purgatory</code>.</td></tr></tbody></table>

<details>

<summary>Purgatory Example</summary>

```json
{ 
    "purgatory": {
      "state": true,
      "reason": "Copyright violation" 
      } 

}
```

```json
{
  "purgatory": {
    "state": false
  }
}
```

</details>

#### Statistics

The `stats` section contains different statistics fields.

<table><thead><tr><th width="142.75440658049354">Attribute</th><th width="125">Type</th><th>Description</th></tr></thead><tbody><tr><td><strong><code>orders</code></strong></td><td><code>number</code></td><td>How often an asset was ordered, meaning how often it was either downloaded or used as part of a compute job.</td></tr></tbody></table>

<details>

<summary>Statistics Example</summary>

```json
{
  "stats": {
    "orders": 4
  }
}
```

</details>

### Compute to data

For algorithms and datasets that are used for compute to data, there are additional fields and objects within the DDO structure that you need to consider. These include:

* `compute` attributes
* `publisherTrustedAlgorithms`
* `consumerParameters`

Details for each of these are explained on the [Compute Options page](compute-to-data/compute-options.md).

### DDO Schema - Detailed

The below diagram shows the detailed DDO schema depicting the content of each data structure and the relations between them.

Please note that some data structures apply only on certain types of services or assets.

```mermaid
---
title: DDO Detailed Diagram
---
classDiagram

    class DDO{
	+@context
	+id
	+version
	+chainId
	+nftAddress
        +Metadata
        +Credentials
        +Service
    }

    class Metadata{
        +created
	+updated
	+description
	+name
        +type ["dataset"/"algorithm"]
	+author
	+license
	+tags
	+links
	+contentLanguage
	+categories
	+copyrightHolder
	+additionalInformation
	+AlgorithmMetadata [for "algorithm" type]
    }
    class Credentials{
        +allow
	+deny
    }

    class AlgorithmMetadata["AlgorithmMetadata (for algorithm)"] {
        +language
	+version
	+ConsumerParameters
	+Container
    }
  
    class Container{
        +entrypoint
	+image
	+tag
	+checksum
    }
    class Service{
        +id
	+type ["access"/"compute"]
	+files
	+name
	+description
	+datatokenAddress
	+serviceEndpoint
	+timeout 
        +additionalInformation
	+ConsumerParameters
	+Compute
    }
    class ConsumerParameters{
	+type
	+name
	+label
	+required
	+description
	+default
	+options
    }
   class Compute{
        +publisherTrustedAlgorithms
	+publisherTrustedAlgorithmPublishers
    }
DDO "1" --> "1" Metadata
DDO "1" --> "1..n" Service
DDO "1" --> "*" Credentials


Metadata "1" --> "0..1" AlgorithmMetadata
AlgorithmMetadata "1" --> "1..*" Container
AlgorithmMetadata "1" --> "1..*" ConsumerParameters

Service "1" --> "0..n" Compute
Service "1" --> "0..n" ConsumerParameters
```
