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

- Mac & Linux: `~/.near-credentials/<network>/<account id>.json`
- Windows: `%USERPROFILE%/.near-credentials/<network>/<account id>.json`

### Sending tokens

:::tip
Make sure you have authenticated as the sending account using [`near login`](#authentication).
:::

```txt
near send <sender> <receiver> <# of NEAR>
```

### Executing functions

Smart contract functions can execute in two modes: **view** and **call** (or **change**).

#### View mode

View function calls are only allowed to read data from the smart contract. They do not:

- Make any on-chain modifications
- Have a signer
- Make cross-contract calls
- Transfer tokens
- Cost gas

```txt
near view <contract> <method> <JSON argumenst>
```

For example:

```txt
$ NEAR_ENV=mainnet near view wrap.near ft_balance_of '{"account_id":"root.near"}'
View call: wrap.near.ft_balance_of({ "account_id": "root.near" })
'1000000000000000000000000'
```

:::tip
If the arguments for your function call are long, complex, or are used often, you may want to put them in a separate `.json` file.

You can use them in a command like so:

```txt
near view <contract> <method> "$(<path/to/args.json)"
```

:::

#### Change mode

Regular function calls perform activity on-chain and require a signer to pay gas fees.

```txt
near call <contract> <method> <JSON arguments> --account-id <signer>
```

:::tip
Regular function calls can perform both change and view calls, but are required to pay gas fees regardless, so if you don't _have_ to perform a change, it's probably best to stick with view calls.
:::
