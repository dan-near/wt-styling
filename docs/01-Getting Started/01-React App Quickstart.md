## React Quickstart

To start integrating `NEAR` in your React application, you can use the [`create-react-app`](https://create-react-app.dev/) template to bootstrap your project.

**Note**: If you are looking for a more step by step approach you can check out the full tutorial [here](/docs/Getting%20Started/React%20Tutorial).

Simply run the following command in your terminal:

```bash
npx create-react-app my-near-app --template near-js
```

or if using `yarn`:

```bash
yarn create-react-app my-near-app --template near-js
```

This will give you a new directory called `my-near-app` with a setup that allows you easy usage of [`near-api-js`](https://github.com/near/near-api-js) in your React application.

Your `App.jsx` file will look something like this:

```jsx
import React, { useEffect, useState } from 'react';
import { connect, WalletConnection, utils, Contract } from 'near-api-js';
import { getConfig } from './config';

const {
  format: { formatNearAmount },
} = utils;

const App = () => {
  const [wallet, setWallet] = useState(null);
  const [contract, setContract] = useState(null);
  const [counter, setCounter] = useState(0);
  const [deposit, setDeposit] = useState(0);
  const [balance, setBalance] = useState('');

  useEffect(() => {
    connect(getConfig()).then((near) => setWallet(new WalletConnection(near)));
  }, []);

  useEffect(() => {
    if (wallet) {
      setContract(
        new Contract(wallet.account(), 'counter.testnet', {
          viewMethods: ['getCounter'],
          changeMethods: [
            'resetCounter',
            'incrementCounter',
            'decrementCounter',
          ],
        })
      );

      wallet
        .account()
        .getAccountBalance()
        .then(({ available }) => setBalance(available));
    }
  }, [wallet]);

  useEffect(() => {
    if (wallet && wallet.isSignedIn() && contract) {
      contract.getCounter().then((counter) => {
        setCounter(counter);
      });
    }
  }, [wallet, contract]);

  const handleLogin = () => {
    wallet.requestSignIn({
      contractId: 'counter.testnet',
      methodNames: [
        'resetCounter',
        'incrementCounter',
        'decrementCounter',
        'getCounter',
      ],
    });
  };

  const handleReset = async () => {
    await contract.resetCounter({
      args: {},
      amount: deposit.toFixed(0),
    });

    setCounter(await contract.getCounter());
  };

  const handleIncrement = async () => {
    await contract.incrementCounter({
      args: { value: 1 },
      amount: deposit.toFixed(0),
    });

    setCounter(await contract.getCounter());
  };

  const handleDecrement = async () => {
    await contract.decrementCounter({
      args: { value: 1 },
      amount: deposit.toFixed(0),
    });

    setCounter(await contract.getCounter());
  };

  return (
    <section>
      <h1>🎉 Congrats on starting your NEAR journey in React! 🎉</h1>
      {wallet && wallet.isSignedIn() ? (
        <div>
          <div>Hi, {wallet.getAccountId()}!</div>
          <p>
            Your account ballance is{' '}
            <strong>{formatNearAmount(balance, 4)}</strong>
          </p>
          <p>
            The current value of the counter is: <strong>{counter}</strong>
          </p>
          <label htmlFor="deposit">
            <span>Deposit value (in yoctoNEAR): </span>
            <input
              id="deposit"
              type="number"
              min={1}
              value={deposit}
              onChange={({ target: { value } }) => setDeposit(parseInt(value))}
            />
          </label>
          <div
            style={{ display: 'flex', flexDirection: 'column', width: '50%' }}
          >
            <button onClick={() => handleReset()}>Reset Counter</button>
            <button onClick={() => handleIncrement()}>Increment counter</button>
            <button onClick={() => handleDecrement()}>Decrement counter</button>
          </div>
        </div>
      ) : (
        <div>
          <button onClick={() => handleLogin()}>Login with NEAR</button>
        </div>
      )}
    </section>
  );
};

export default App;
```

Here you have usage examples of the four main functionalities you will be using to interact with `NEAR`.

You can find the usage example of how to use `wallets` to sign users in and query the users data.

```js
const [wallet, setWallet] = useState(null);

const handleLogin = () => {
  wallet.requestSignIn({
    contractId: 'counter.testnet',
    methodNames: [
      'resetCounter',
      'incrementCounter',
      'decrementCounter',
      'getCounter',
    ],
  });
};

wallet
  .account()
  .getAccountBalance()
  .then(({ available }) => setBalance(available));
```

How to call view methods on smart contracts:

```js
contract.getCounter().then((counter) => {
  setCounter(counter);
});
```

And how to call change methods on smart contracts:

```js
await contract.incrementCounter({
  args: { value: 1 },
  amount: deposit.toFixed(0),
});

setCounter(await contract.getCounter());
```

For a more in depth walkthrough of how to integrate NEAR in your React app, you can follow the [full tutorial](/docs/Getting%20Started/React%20Tutorial).
