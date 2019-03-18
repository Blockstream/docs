---
layout: default
title: API
nav_order: 1
parent: Explorer
---

# Blockstream Explorer API
{: .no_toc }
The [Blockstream Explorer](https://blockstream.info) API provides developers with a number of tools to build applications that monitor and interact with the blockchains from:
- Bitcoin mainnet
- Bitcoin testnet
- The Liquid Network

The API uses JSON over RESTful HTTP.

**Note: amounts are always represented in satoshis.**

## Contents
{: .no_toc }

1. TOC
{:toc}

## Transactions

```json
GET /tx/:txid
```

Returns information about the transaction.

Available fields: `txid`, `version`, `locktime`, `size`, `weight`, `fee`, `vin`, `vout` and `status`
(see [transaction format](#transaction-format) for details).

```json
GET /tx/:txid/status
```

Returns the transaction confirmation status.

Available fields: `confirmed` (boolean), `block_height` (optional) and `block_hash` (optional).

```json
GET /tx/:txid/hex
```

Returns the raw transaction in hex.

<!--
### `GET /tx/:txid/merkle-proof`

Returns a merkle inclusion proof for the transaction.

*Currently uses Electrum's merkle proof format, will eventually be changed to use the `merkleblock` format.*
-->

```json
GET /tx/:txid/outspend/:vout
```

Returns the spending status of a transaction output.

Available fields: `spent` (boolean), `txid` (optional), `vin` (optional) and `status` (optional, the status of the spending tx).

```json
GET /tx/:txid/outspends
```

Returns the spending status of all transaction outputs.

```json
GET /broadcast?tx=<rawtx>
```

Broadcast `<rawtx>` (the raw transaction in hex) to the network.
Returns the `txid` on success.

## Addresses

```json
GET /address/:address
GET /scripthash/:hash
```

Get information about an address/scripthash.

Available fields: `address`/`scripthash`, `chain_stats` and `mempool_stats`.

`{chain,mempool}_stats` each contain an object with `tx_count`, `funded_txo_count`, `funded_txo_sum`, `spent_txo_count` and `spent_txo_sum`.

Elements-based chains don't have the `{funded,spent}_txo_sum` fields.

```json
GET /address/:address/txs
GET /scripthash/:hash/txs
```

Get transaction history for the specified address/scripthash, sorted with newest first.

Returns up to 50 mempool transactions plus the first 25 confirmed transactions.
You can request more confirmed transactions using `:last_seen_txid`(see below).

```json
GET /address/:address/txs/chain[/:last_seen_txid]
GET /scripthash/:hash/txs/chain[/:last_seen_txid]
```

Get confirmed transaction history for the specified address/scripthash, sorted with newest first.

Returns 25 transactions per page. More can be requested by specifying the last txid seen by the previous query.

```json
GET /address/:address/txs/mempool
GET /scripthash/:hash/txs/mempool
```

Get unconfirmed transaction history for the specified address/scripthash.

Returns up to 50 transactions (no paging).

```json
GET /address/:address/utxo
GET /scripthash/:hash/utxo
```

Get the list of unspent transaction outputs associated with the address/scripthash.

Available fields: `txid`, `vout`, `value` and `status` (with the status of the funding tx).
Elements-based chains have an additional `asset` field.

## Blocks

```json
GET /block/:hash
```

Returns information about a block.

Available fields: `id`, `height`, `version`, `timestamp`, `bits`, `nonce`, `merkle_root`, `tx_count`, `size`, `weight` and `previousblockhash`.
Elements-based chains have an additional `proof` field.
See [block format](#block-format) for more details.

The response from this endpoint can be cached indefinitely.

```json
GET /block/:hash/status
```

Returns the block status.

Available fields: `in_best_chain` (boolean, false for orphaned blocks), `next_best` (the hash of the next block, only available for blocks in the best chain).

```json
GET /block/:hash/txs[/:start_index]
```

Returns a list of transactions in the block (up to 25 transactions beginning at `start_index`).

Transactions returned here do not have the `status` field, since all the transactions share the same block and confirmation status.

The response from this endpoint can be cached indefinitely.

```json
GET /block/:hash/txids
```

Returns a list of all txids in the block.

The response from this endpoint can be cached indefinitely.

```json
GET /block-height/:height
```

Returns the hash of the block currently at `height`.

```json
GET /blocks[/:start_height]
```

Returns the 10 newest blocks starting at the tip or at `start_height` if specified.

```json
GET /blocks/tip/height
```

Returns the height of the last block.

```json
GET /blocks/tip/hash
```

Returns the hash of the last block.

## Transaction format

- `txid`
- `version`
- `locktime`
- `size`
- `weight`
- `fee`
- `vin[]`
  - `txid`
  - `vout`
  - `is_coinbase`
  - `scriptsig`
  - `scriptsig_asm`
  - `sequence`
  - `witness[]`
  - `prevout` (previous output in the same format as in `vout` below)
  - *(Elements only)*
  - `is_pegin`
  - `issuance` (available for asset issuance transactions, `null` otherwise)
    - `is_reissuance`
    - `asset_blinding_nonce`
    - `asset_entropy`
    - `assetamount` or `assetamountcommitment`
    - `tokenamount` or `tokenamountcommitment`
- `vout[]`
  - `scriptpubkey`
  - `scriptpubkey_asm`
  - `scriptpubkey_type`
  - `scriptpubkey_address`
  - `value`
  - *(Elements only)*
  - `valuecommitment`
  - `asset` or `assetcommitment`
  - `pegout` (available for peg-out outputs, `null` otherwise)
    - `genesis_hash`
    - `scriptpubkey`
    - `scriptpubkey_asm`
    - `scriptpubkey_address`
- `status`
  - `confirmed` (boolean)
  - `block_height` (available for confirmed transactions, `null` otherwise)
  - `block_hash` (available for confirmed transactions, `null` otherwise)
  - `block_time` (available for confirmed transactions, `null` otherwise)

## Block format

- `id`
- `height`
- `version`
- `timestamp`
- `bits`
- `nonce`
- `merkle_root`
- `tx_count`
- `size`
- `weight`
- `previousblockhash`
- *(Elements only)*
- `proof`
  - `challenge`
  - `challenge_asm`
  - `solution`
  - `solution_asm`
