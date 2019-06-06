# Cashback App

A simple application which rewards its users for sending tokens. 

> Check out the full code example for the [Cashback App on Github](https://github.com/LiskHQ/lisk-sdk-test-app).

## 1. Set up Lisk SDK

First, you need to set up the Lisk SDK. Please follow the instructions in the [Lisk SDK - Usage](../lisk-sdk/introduction.md#usage) section.

## 2. Configure the application

Next, let's configure the application, to provide basic information about the app we are going to build:

```js
//index.js
const { Application, genesisBlockDevnet, configDevnet } = require('lisk-sdk'); // require application class, the default genesis block and the default config for the application

const app = new Application(genesisBlockDevnet, configDevnet); // create the application instance
```

In the `line 2`, we require the needed dependencies from the `lisk-sdk` package.
The most important one is the `Application` class, which is used in `line 4` to create the application instance.
The application instance will start the whole application at the bottom of `index.js`.

In `line 4` , the application instance gets initialized.
By passing the parameters for the [genesis block](../lisk-sdk/configuration.md) and the [configuration template](https://github.com/LiskHQ/lisk-sdk/blob/development/sdk/src/samples/config_devnet.json), the application is configured with most basic configurations to start the node.

## 3. Create a new transaction type

Now, we want to create a new [custom transaction type](custom-transactions.md) `CashbackTransaction`: 
It extends the pre-existing transaction type `TransferTransaction`.
In difference to the normal `TransferTransaction`, the Cashback transaction type pays out a 10% bonus reward to the sender of any Cashback transaction type.

So e.g. if Alice sends 100 token to Bob as a Cashback transaction, Bob would receive the 100 token and Alice would receive additional 10 tokens as a cashback.

> If you compare the methods below with the methods we implemented for the `HelloTransaction`, you will notice, that we implement less methods for the `CashbackTransaction`.
> This is because we extend the `CashbackTransaction` from an already existing transaction type `TransferTransaction`.
> As a result, all required methods are implemented already inside the `Transfertransaction` class, and we only need to overwrite/extend explicitely the methods we want to customize.

Now, let's create a new file `cashback_transaction.js`, which is defining the new transaction type `CashbackTransaction`:

```js
//cashback_transaction.js
const {	TransferTransaction, BigNum } = require('lisk-sdk'); // import the TransferTransaction class and Bignum from the Lisk SDK package

class CashbackTransaction extends TransferTransaction { // let the CashbackTransaction become a child class of TransferTransaction

// add the all methods described below here

}

module.exports = CashbackTransaction;
``` 

> You find a version of [the complete `cashback_transaction.js` file](https://github.com/LiskHQ/lisk-sdk-examples/blob/development/cashback/transactions/cashback_transaction.js) on Github.

### TYPE

Set the Cashback transaction TYPE to `11`.
The first 10 types, from `0-9` is reserved for the default Lisk Network functions.
Type `10` was used previously for the `HelloTransaction`, so we set it to `11`, but any other integer value (that is not already used by another transaction type) is a valid value.
```js
static get TYPE () {
    return 11;
}
```

### applyAsset

The CashbackTransaction adds an inflationary 10% to senders account.

Invoked as part of the `apply` step of the BaseTransaction and block processing.  
```js
applyAsset(store) {
    super.applyAsset(store); // transfer the tokens to the recipient account, executes applyAsset() of TransferTransaction

    const sender = store.account.get(this.senderId); // get sender id
    const updatedSenderBalanceAfterBonus = new BigNum(sender.balance).add( // add 1/10 of the transaction amount to senders account balance
        new BigNum(this.amount).div(10) 
    );
    const updatedSender = { // update senders account balance
        ...sender,
        balance: updatedSenderBalanceAfterBonus.toString(),
    };
    store.account.set(sender.address, updatedSender); // push updated account back to database

    return []; // array of TransactionErrors, returns empty array if no errors are thrown
}
```

### undoAsset

Inverse of `applyAsset`.
Undoes the changes made in `applyAsset` step: It sends the transaction amount back to the sender and substracts 1/10 of the transaction amount from the senders account balance.
```js
undoAsset(store) {
    super.undoAsset(store); // transfer the tokens back from the recipient account to the senders account, executes undoAsset() of TransferTransaction

    const sender = store.account.get(this.senderId); // get sender id
    const updatedSenderBalanceAfterBonus = new BigNum(sender.balance).sub( // substract 1/10 of the transaction amount from senders account balance
        new BigNum(this.amount).div(10)
    );
    const updatedSender = { // update senders account balance
        ...sender,
        balance: updatedSenderBalanceAfterBonus.toString(),
    };
    store.account.set(sender.address, updatedSender); // push updated account back to database

    return []; // array of TransactionErrors, returns empty array if no errors are thrown
}
```

## 4. Register the new transaction type

Right now, your project should have the following file structure:

```bash
/cashback-app # root directory of the application
/cashback-app/cashback_transaction.js # the custom transaction, created in step 3
/cashback-app/index.js # the index file of your application, created in step 1, extended in step 2 and 4
/cashback-app/node_modules/ # project dependencies, created in step 1
/cashback-app/package.json # project manifest file, created in step 1
```

Add the new transaction type to your application, by registering it to the application instance:

```js
//index.js
const { Application, genesisBlockDevnet, configDevnet} = require('lisk-sdk'); // require application class, the default genesis block and the default config for the application
const CashbackTransaction = require('./cashback_transaction'); // require the newly created transaction type 'CashbackTransaction'

const app = new Application(genesisBlockDevnet, configDevnet); // create the application instance

app.registerTransaction(11, CashbackTransaction); // register the 'CashbackTransaction' 


// the code block below starts the application and doesn't need to be changed
app
    .run()
    .then(() => app.logger.info('App started...'))
    .catch(error => {
        console.error('Faced error in application', error);
        process.exit(1);
    });
```

## 5. Start the network

Now, let's start our customized blockchain network for the first time.

The parameter `configDevnet`, which we pass to our `Application` instance in step 3, is preconfigured to start the node with a set of dummy delegates, that have enabled forging by default.
These dummy delegates stabilize the new network and make it possible to test out the basic functionality of the network with only one node immediately.

This creates a simple Devnet, which is beneficial during development of the blockchain application.
The dummy delegates can be replaced by real delegates later on.

To start the network, execute the following command:

```bash
node index.js | npx bunyan -o short
```

`node index.js` will start the node, and `| npx bunyan -o short` will pretty-print the logs in the console.

Check the logs, to verify the network has started successfully.

If something went wrong, the process should stop and an error with debug information is displayed.

If everything is ok, you should be able to see similar logs like the ones displayed in [step 5 of the Hello World app tutorial](#5-start-the-network).

For further interaction with the network, you can run the process in the background by executing:

```bash
npx pm2 start --name cashback index.js # add the application to pm2 under the name 'cashback'
npx pm2 stop cashback # stop the cashback app
npx pm2 start cashback # start the cashback app
```

## 6. Interact with the network

Now that the network is started, let's try to send a `CashbackTransaction` to our node to see if it gets accepted.

As first step, create the transaction object.

First, let's reuse the script [create_sendable_transaction.js](https://github.com/LiskHQ/lisk-sdk-examples/blob/development/scripts/create_sendable_transaction_base_trs.js) which we already described in [step 6 of Hello World app](#6-interact-with-the-network).

We can use this function in our script for printing a sendable cashback transaction object:

```js
//print_sendable_cashback.js
const createSendableTransaction = require('./create_sendable_transaction_base_trs');
const CashbackTransaction = require('../cashback-app/cashback_transaction');

let c = createSendableTransaction(CashbackTransaction, { // the desired transaction gets created and signed
	type: 11, // we want to send a transaction type 11 (= CashbackTransaction)
	data: null,
	amount: `${10 ** 8}`, // we set the amount to 1 LSK
	fee: `${10 ** 7}`, // we set the fee to 0.1 LSK
 	recipientId: '10881167371402274308L', // recipient address: dummy delegate genesis_100
 	recipientPublicKey: 'addb0e15a44b0fdc6ff291be28d8c98f5551d0cd9218d749e30ddb87c6e31ca9', // public key of the recipient 
 	senderPublicKey: 'c094ebee7ec0c50ebee32918655e089f6e1a604b83bcaa760293c61e0f18ab6f', // the senders publickey
 	passphrase: 'wagon stock borrow episode laundry kitten salute link globe zero feed marble', // the senders passphrase, needed to sign the transaction
 	secondPassphrase: null,
 	timestamp: 2,
});

console.log(c); // the transaction is displayed as JSON object in the console
```

Now that we have a sendable transaction object, let's send it to our node and see how it gets processed by analyzing the logs.

For this, we utilize the API of the node and post the created transaction object to the transaction endpoint of the API.

Because the API of every node is only accessible form localhost by default, you need to execute this query on the same server that your node is running on, unless you changed the config to make your API accessible to others or to the public.

```bash
node print_sendable_cashback.js | curl -X POST -H "Content-Type: application/json" -d @- localhost:4000/api/transactions
```

> __Optional:__ After first successful verification, you may wan to reduce the default console log level (info) and file log level (debug).<br> 
> You can do so, by passing a copy of the config object `configDevnet` with customized config for the logger component:

```js
//index.js
const { Application, genesisBlockDevnet, configDevnet} = require('lisk-sdk'); // require application class, the default genesis block and the default config for the application
const CashbackTransaction = require('./cashback_transaction'); // require the newly created transaction type 'CashbackTransaction'

configDevnet.components.logger.fileLogLevel = "error"; // will only log errors and fatal errors in the log file
configDevnet.components.logger.consoleLogLevel = "none"; // no logs will be shown in console

const app = new Application(genesisBlockDevnet, configDevnet); // create the application instance
```

As next step, you can use a wallet software like e.g. a customized [Lisk Hub](https://lisk.io/hub), so that users can utlize the new transaction type.

See also section [Interact with the network](interact-with-network.md).