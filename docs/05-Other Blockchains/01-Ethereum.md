## Account Identifiers

### Ethereum Addresses

Ethereum addresses are derived from the account's public key, through a process something like this:

```python
# public_key is a byte array
# keccak256() returns a 64-byte array
address = "0x" + keccak256(public_key)[12:].hex()
```

In other words: "the hexadecimal encoding of the last 40 bytes of the Keccak-256 hash of the account's public key, prepended with `0x`".

:::note
There is also a [mixed-case checksum encoding step (EIP-55)](https://eips.ethereum.org/EIPS/eip-55), but that is beyond the scope of this document.
:::

This results in an address that looks something like: `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

### NEAR Account IDs

NEAR account identifiers can be one of two types: human-readable and implicit.

Human-readable account IDs must be created by their immediate parent before they can be used. For example, the account identifier `app.example.near` can only be created by `example.near`.

Implicit accounts may be used before they are created. (Or, they are created if/when they are used.) They work similarly to Ethereum accounts in that the identifier of an implicit account is derived from the public key that controls the account.

## Keys

Externally-owned Ethereum accounts can only be controlled by the private key corresponding to the public key from which the account address is derived.

NEAR accounts can have any number of access keys attached to them. These keys may be full-access (allowed to sign any transaction from the account), or they may be permissioned, that is, allowed to sign only a subset of viable transactions.

## Storage

## Smart Contracts

## Integration & Interoperability
