# aerodrome

A [nuthatch](https://github.com/nuthatch-org/nuthatch) nest: **Aerodrome on Base**.

The ve(3,3) DEX: every pool the factory creates, its swaps, and the fee events that make ve(3,3) different.

One binary, one config file, no graph-node, no gateway, no query fees.

## What it indexes

**Chain:** `base`. **1 contract**, **12 tables**.

| alias | address |
|---|---|
| `factory` | `0x420dd381b31aef6683db6b902084cb0ffece40da` |

## Verified

Indexed blocks **50,111,962 to 50,311,269** and sealed **83,099 events**. Every table below is generated from the vendored ABIs, and the run above is what this nest actually decoded, not an estimate.

## Read this before trusting it

- `PoolCreated` carries an indexed `stable` bool: one factory, two different curves, and the flag arrives free in the topics so telling them apart needs no contract call.
- The pool ABI could not be read from a pool - Aerodrome pools are minimal-proxy clones, unverified on both Sourcify and Blockscout. The factory's own `implementation()` names the contract it clones, and *that* is verified.

## Run it

```sh
nuthatch init --from https://github.com/nuthatch-org/aerodrome
cd aerodrome
nuthatch dev --dir . --backfill 50000 --seal-direct
nuthatch sql --dir . "SELECT count(*) FROM \"factory__pool_created\""
```

The endpoint in `nuthatch.toml` is keyless and public, so this file is publishable: a `nuthatch.toml` is pinned into the nest's content address and must never carry a credential. It is enough to follow the tip. A **backfill** wants archive depth it may not have: pass your own with `--rpc`, and check it first with `nuthatch doctor --rpc <url>`.

## Tables

```
factory__pool_created
factory__set_custom_fee
factory__set_fee_manager
factory__set_pause_state
factory__set_pauser
factory__set_voter
pool__burn
pool__claim
pool__fees
pool__mint
pool__swap
pool__sync
```
