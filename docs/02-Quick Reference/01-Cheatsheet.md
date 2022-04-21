## `near-cli`

### Installation

```txt
npm install -g near-cli
```

[`near-cli` on npmjs.com](https://www.npmjs.com/package/near-cli)

### Network selection

The NEAR CLI reads the `NEAR_ENV` environment variable to determine the RPC configuration to use when making network requests. Options include: `testnet`, `betanet`, `mainnet`.

Set the network for the current shell:

```txt
export NEAR_ENV=mainnet
```

Or for a single command:

```txt
NEAR_ENV=mainnet <command>
```

### Authentication

Create a full access key and add it to an account for use by the CLI.

```txt
near login
```

### Viewing stored keys

Keypairs are stored with the associated account ID in the following location:

* Mac & Linux: `~/.near-credentials/<network>/<account id>.json`
* Windows: `%USERPROFILE%/.near-credentials/<network>/<account id>.json`

### Sending tokens

:::tip
Make sure you have authenticated as the sending account using [`near login`](#authentication).
:::

```txt
near send <sender> <receiver> <# of NEAR>
```
