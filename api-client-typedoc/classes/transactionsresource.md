> ## [@liskhq/lisk-api-client](../README.md)

[TransactionsResource](transactionsresource.md) /

# Class: TransactionsResource

## Hierarchy

* [APIResource](apiresource.md)

  * **TransactionsResource**

### Index

#### Constructors

* [constructor](transactionsresource.md#constructor)

#### Properties

* [apiClient](transactionsresource.md#apiclient)
* [broadcast](transactionsresource.md#broadcast)
* [get](transactionsresource.md#get)
* [path](transactionsresource.md#path)

#### Accessors

* [headers](transactionsresource.md#headers)
* [resourcePath](transactionsresource.md#resourcepath)

#### Methods

* [handleRetry](transactionsresource.md#handleretry)
* [request](transactionsresource.md#request)

## Constructors

###  constructor

\+ **new TransactionsResource**(`apiClient`: [APIClient](apiclient.md)): *[TransactionsResource](transactionsresource.md)*

*Overrides [APIResource](apiresource.md).[constructor](apiresource.md#constructor)*

*Defined in [resources/transactions.ts:63](url)*

**Parameters:**

Name | Type |
------ | ------ |
`apiClient` | [APIClient](apiclient.md) |

**Returns:** *[TransactionsResource](transactionsresource.md)*

___

## Properties

###  apiClient

● **apiClient**: *[APIClient](apiclient.md)*

*Inherited from [APIResource](apiresource.md).[apiClient](apiresource.md#apiclient)*

*Defined in [api_resource.ts:25](url)*

___

###  broadcast

● **broadcast**: *[APIHandler](../README.md#apihandler)*

*Defined in [resources/transactions.ts:49](url)*

Submits a signed transaction object for processing by the transaction pool.

### Usage Example
```ts
client.accounts.broadcast({
id: '222675625422353767',
amount: '150000000',
fee: '1000000',
type: 0,
timestamp: 28227090,
senderId: '12668885769632475474L',
senderPublicKey: '2ca9a7143fc721fdc540fef893b27e8d648d2288efa61e56264edf01a2c23079',
senderSecondPublicKey: '2ca9a7143fc721fdc540fef893b27e8d648d2288efa61e56264edf01a2c23079',
recipientId: '12668885769632475474L',
signature: '2821d93a742c4edf5fd960efad41a4def7bf0fd0f7c09869aed524f6f52bf9c97a617095e2c712bd28b4279078a29509b339ac55187854006591aa759784c205',
signSignature: '2821d93a742c4edf5fd960efad41a4def7bf0fd0f7c09869aed524f6f52bf9c97a617095e2c712bd28b4279078a29509b339ac55187854006591aa759784c205',
signatures: [
'72c9b2aa734ec1b97549718ddf0d4737fd38a7f0fd105ea28486f2d989e9b3e399238d81a93aa45c27309d91ce604a5db9d25c9c90a138821f2011bc6636c60a',
],
asset: {},
})
.then(res => {
console.log(res.data);
});
```

___

###  get

● **get**: *[APIHandler](../README.md#apihandler)*

*Defined in [resources/transactions.ts:62](url)*

Searches for a specified transaction in the system.

### Usage Example
```ts
client.accounts.get({ id: '222675625422353767' })
.then(res => {
console.log(res.data);
});
```

___

###  path

● **path**: *string*

*Overrides [APIResource](apiresource.md).[path](apiresource.md#path)*

*Defined in [resources/transactions.ts:63](url)*

___

## Accessors

###  headers

● **get headers**(): *[HashMap](../interfaces/hashmap.md)*

*Inherited from [APIResource](apiresource.md).[headers](apiresource.md#headers)*

*Defined in [api_resource.ts:33](url)*

**Returns:** *[HashMap](../interfaces/hashmap.md)*

___

###  resourcePath

● **get resourcePath**(): *string*

*Inherited from [APIResource](apiresource.md).[resourcePath](apiresource.md#resourcepath)*

*Defined in [api_resource.ts:37](url)*

**Returns:** *string*

___

## Methods

###  handleRetry

▸ **handleRetry**(`error`: `Error`, `req`: `AxiosRequestConfig`, `retryCount`: number): *`Promise<APIResponse>`*

*Inherited from [APIResource](apiresource.md).[handleRetry](apiresource.md#handleretry)*

*Defined in [api_resource.ts:41](url)*

**Parameters:**

Name | Type |
------ | ------ |
`error` | `Error` |
`req` | `AxiosRequestConfig` |
`retryCount` | number |

**Returns:** *`Promise<APIResponse>`*

___

###  request

▸ **request**(`req`: `AxiosRequestConfig`, `retry`: boolean, `retryCount`: number): *`Promise<APIResponse>`*

*Inherited from [APIResource](apiresource.md).[request](apiresource.md#request)*

*Defined in [api_resource.ts:66](url)*

**Parameters:**

Name | Type | Default |
------ | ------ | ------ |
`req` | `AxiosRequestConfig` | - |
`retry` | boolean | - |
`retryCount` | number | 1 |

**Returns:** *`Promise<APIResponse>`*

___