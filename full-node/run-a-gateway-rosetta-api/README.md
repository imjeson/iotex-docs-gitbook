# Run a Gateway / Rosetta API

This guide provides information and links to the relevant documentation for exchanges to integrate with the IoTeX blockchain.

### Deploy IoTeX Full Nodes

You'll need to deploy a few full nodes, for reading from and writing to the network. The full node stores the entire blockchain state, including pending actions/actions, and broadcasts new states (blocks, actions/actions) to the network.

Please follow [this guide](https://github.com/iotexproject/iotex-bootstrap#iotex-delegate-manual) to setup a full node running with the `gateway` plugin enabled.

Optionally, please follow [this guide](https://github.com/iotexproject/iotex-bootstrap/tree/master/infra/monitoring) to configure a dashboard for node monitoring. Alternatively, if you used the [one-line upgrader ](https://github.com/iotexproject/iotex-bootstrap#upgrade)to install the full node, you can just answer "Yes" to the following installer question:

```
Do you want to monitor the status of the node [Y/N] (Default: N)?) to setup the dashboard for monitoring.
```

### Interact with IoTeX Full Nodes <a href="#interact-with-iotex-full-nodes" id="interact-with-iotex-full-nodes"></a>

Once the full nodes are fully synced, one can communicate with them via [gRPC ](https://grpc.io/)service specified in the configuration file: see the full [API reference docs](../../reference/node-core-api-grpc.md) and the corresponding `proto` files on [GitHub](https://github.com/iotexproject/iotex-proto).

### Generate New Accounts <a href="#generate-new-accounts" id="generate-new-accounts"></a>

A IoTeX account can be generated via [this algorithm](https://github.com/iotexproject/iotex-address/blob/master/README.md).

{% hint style="info" %}
If you have not installed the latest **ioctl Command line client**, please check out the installation [instructions](../../reference/ioctl-cli-reference/).
{% endhint %}

New accounts can be generated using ioctl command line client, which will provide the private key and corresponding public keys and IoTeX address. See [this example](../../get-started/iotex-wallets/command-line-client/create-an-iotex-account.md) for more details about how to create a IoTeX account with ioctl.

{% hint style="warning" %}
gRPC Core API does not support the creation of new accounts.
{% endhint %}

### Send IOTX <a href="#send-iotx" id="send-iotx"></a>

To send a signed action, the [/APIService/SendAction](../../reference/node-core-api-grpc.md#sendaction) gRPC endpoint can be used, or ioctl command line client can be used also: see [this example](broken-reference) for more details on how to send a signed action using ioctl.

{% hint style="warning" %}
For safety and auditing reasons, after a new action gets added to the blockchain (whether it's a user deposit or a withdraw) the exchange must retrieve the action data and verify the following:

1. The status of the receipt must equal 1 (_success_)
2. The action type must be "transfer"
3. The amount equals to the deposit \[withdraw] amount
4. The sender and recipient matches with intended parties
5. The timestamp of the action roughly equals the actual time of sending the raw action to blockchain (within 5\~10 seconds range)
{% endhint %}

Here's an [example ](https://iotexscan.io/action/355bd7b93dadc18c2d2689cd400272d28ad28df8e6a1555086233c4b619adfee)of the data associated to a IoTeX action.

### Retrieve actions <a href="#retrieve-actions" id="retrieve-actions"></a>

One can retrieve confirmed actions as well as pending (unconfirmed) actions by using `/APIService/GetActions` endpoint. Examples are given below:

1. [Get action by action hash](../../reference/node-core-api-grpc.md#getactionbyhash)
2. [Get confirmed actions by address](../../reference/node-core-api-grpc.md#getactionsbyaddress)
3. [Get unconfirmed actions by address](https://docs.iotex.io/developer/core-api/api.html#getunconfirmedactionsbyaddress)
4. [Get actions by block](../../reference/node-core-api-grpc.md#getactionsbyblock)

One can also use ioctl command line tool to [query an action by hash](https://docs.iotex.io/developer/ioctl/action.html#query-action).

### Retrieve actions via contracts <a href="#retrieve-actions-via-contracts" id="retrieve-actions-via-contracts"></a>

One can retrieve confirmed actions via contracts by using `/APIService/GetEVMTransfersByBlockHeight` and `/APIService/GetEVMTransfersByActionHash` endpoints.&#x20;

Examples are given below:

1. [Get EVM transfers by block height](https://docs.iotex.io/developer/core-api/api.html#getevmtransfersbyactionhash)
2. [Get EVM transfers by action hash](https://docs.iotex.io/developer/core-api/api.html#getevmtransfersbyblockheight)

\*\*Note that you need to turn on the System Action Log feature if you run your own node as gateway (see `enableSystemLog` setting in your `config.yaml` node configuration).

### Retrieve Blocks <a href="#retrieve-blocks" id="retrieve-blocks"></a>

One can retrieve blocks that contain the target transfers by using `/APIService/GetBlockMetas` gRPC endpoint.&#x20;

Examples are given below:

1. [Get block metadata by index](../../reference/node-core-api-grpc.md#getblockmetasbyindex)
2. [Get block metadata by block hash](../../reference/node-core-api-grpc.md#getblockmetasbyhash)

One can also use ioctl command line tool to [query a block by height or hash](../../reference/ioctl-cli-reference/query-the-blockchain.md#query-block).

### Retrieve transaction Log <a href="#retrieve-transaction-log" id="retrieve-transaction-log"></a>

One can retrieve transaction logs that contain the target transfers by using `/APIService/GetactionLogByBlockHeight` and `/APIService/GetactionLogByActionHash` endpoints.&#x20;

Examples are given below:

1. [Get transaction logs by block height](../../reference/node-core-api-grpc.md#gettransactionlogbyactionhash)
2. [Get transaction logs by action hash](../../reference/node-core-api-grpc.md#gettransactionlogbyblockheight)

{% hint style="warning" %}
If you get an "**unimplemented**" error code, with a message like "**feature not supported**" when calling an action log APIs, please resync your node from 0, or download our latest snapshot with index data. Check out the [Gateway node setup and snapshot download guides ](https://github.com/iotexproject/iotex-bootstrap#mainnet)for more instructions.
{% endhint %}

### Rosetta API support <a href="#rosetta-api-support" id="rosetta-api-support"></a>

One can deploy a rosetta gateway along with a IoTeX mainnet node and use the rosetta API instead. For more about Rosetta support see the [Rosetta Integration document](rosetta-api.md).

### Testing <a href="#testing" id="testing"></a>

Once you've fully integrated with the IoTeX blockchain, please make accurate tests on both testnet and mainnet. All states of the IoTeX blockchain can be queried through either the [ioctl command line tool ](../../reference/ioctl-cli-reference/) or the official IoTeX explorers for ([Mainnet ](https://iotexscan.io/)and [Testnet](https://testnet.iotexscan.io/)).

Please reach out to IoTeX Team if you have any question.
