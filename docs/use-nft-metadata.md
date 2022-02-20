---
tags: [nft, metadata, ipfs]
---

# How to store metadata to IPFS and include it in an NFT

*Use the Tatum native IPFS integration and free storage to easily upload NFT metadata.*

---

Making NFTs with Tatum is just a matter of a [few simple API calls](../token/docs/How-to-create-NFT.md). A very important step in the process is uploading your metadata (image, video, audio file, etc.) to IPFS and including it in your NFT. In this guide, you’ll learn how to do exactly that.

## Why IPFS? 

[The InterPlanetary File System](https://ipfs.io/) (IPFS) is a distributed file storage system that is perfect for storing NFT metadata. It ensures long-term data resilience and security, and now that Tatum has partnered with [NFT.storage](https://nft.storage/), you can store your files on it for free!

## How to do it

1. We’ve built a native IPFS integration into our platform, so you just need a couple of API calls to store your metadata.
First, upload your metadata (image, video, audio file, etc.) to IPFS using the following [API call](../storage/b3A6MjgzMjc3NzQ-store-data-to-ipfs):

**Request example**
```json
curl --request POST \
  --url https://api-eu1.tatum.io/v3/ipfs \
  --header 'Content-Type: multipart/form-data' \
  --header 'content-type: multipart/form-data; boundary=---011000010111000001101001' \
  --header 'x-api-key: ' \
  --form file=
```
2. A successful call will return a response that includes an IPFS hash.
**Response example**
```json
{
  "ipfsHash": "bafybeihrumg5hfzqj6x47q63azflcpf6nkgcvhzzm6f6nhic2eg6uvozlq/test-356.jpg"
}
```
3. Using this hash, create a JSON metadata scheme in your development environment and save it. The required fields are:
- NFT name
- NFT description
- IPFS hash

4. Here is an example [JSON metadata scheme](https://gateway.pinata.cloud/ipfs/bafybeidi7xixphrxar6humruz4mn6ul7nzmres7j4triakpfabiezll4ti/metadata.json) which is connected to [this NFT](https://testnets.opensea.io/assets/0x0ff74c54dcca05ba1c0a8c3c00d96b53296fc220/559114) for reference:
```json
{
  "name": "Lorem Ipsum Dolor Test",
  "description": "Friendly Lorem OpenSea Creature that enjoys long swims in the ocean.",
  "image": "ipfs://bafybeihrumg5hfzqj6x47q63azflcpf6nkgcvhzzm6f6nhic2eg6uvozlq/test-356.jpg"
}
```
5. Now, upload your saved JSON metadata scheme to IPFS using the [same API call](../storage/b3A6MjgzMjc3NzQ-store-data-to-ipfs) as above.
6. Again, a successful upload will return an IPFS hash in the response.
7. Include this hash in the `url` field of the Mint NFT API call when you [mint a new NFT](../token/b3A6MzA3Mjc3Mzc-mint-nft).

**Request example**
```json
curl --request POST \
  --url https://api-eu1.tatum.io/v3/blockchain/nft/mint \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: ' \
  --header 'x-testnet-type: ' \
  --data '{
  "tokenId": "100000",
  "to": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "contractAddress": "0x687422eEA2cB73B5d3e242bA5456b782919AFc85",
  "url": "https://my_token_data.com",
  "authorAddresses": [
    "0x687422eEA2cB73B5d3e242bA5456b782919AFc85"
  ],
  "cashbackValues": [
    "0.5"
  ],
  "fromPrivateKey": "0x05e150c73f1920ec14caa1e0b6aa09940899678051a78542840c2668ce5080c2",
  "nonce": 0,
}'
```
**Response example**
```json
{
  "txId": "c83f8818db43d9ba4accfe454aa44fc33123d47a4f89d47b314d6748eb0e9bc9",
  "failed": false
}
```
And there you have it! You've successfully uploaded your metadata to IPFS, created a JSON metadata scheme containing the file URL, uploaded the scheme, and minted an NFT connected to it.
