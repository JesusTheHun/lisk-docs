> ## [@liskhq/lisk-api-client](../README.md)

[DappsResource](dappsresource.md) /

# Class: DappsResource

## Hierarchy

* [APIResource](apiresource.md)

  * **DappsResource**

### Index

#### Constructors

* [constructor](dappsresource.md#constructor)

#### Properties

* [apiClient](dappsresource.md#apiclient)
* [get](dappsresource.md#get)
* [path](dappsresource.md#path)

#### Accessors

* [headers](dappsresource.md#headers)
* [resourcePath](dappsresource.md#resourcepath)

#### Methods

* [handleRetry](dappsresource.md#handleretry)
* [request](dappsresource.md#request)

## Constructors

###  constructor

\+ **new DappsResource**(`apiClient`: [APIClient](apiclient.md)): *[DappsResource](dappsresource.md)*

*Overrides [APIResource](apiresource.md).[constructor](apiresource.md#constructor)*

*Defined in [resources/dapps.ts:34](url)*

**Parameters:**

Name | Type |
------ | ------ |
`apiClient` | [APIClient](apiclient.md) |

**Returns:** *[DappsResource](dappsresource.md)*

___

## Properties

###  apiClient

● **apiClient**: *[APIClient](apiclient.md)*

*Inherited from [APIResource](apiresource.md).[apiClient](apiresource.md#apiclient)*

*Defined in [api_resource.ts:25](url)*

___

###  get

● **get**: *[APIHandler](../README.md#apihandler)*

*Defined in [resources/dapps.ts:33](url)*

Searches for a specified dapp in the system.

### Usage Example
```ts
client.accounts.get({ name: 'LiskKitties' })
.then(res => {
console.log(res.data);
});
```

___

###  path

● **path**: *string*

*Overrides [APIResource](apiresource.md).[path](apiresource.md#path)*

*Defined in [resources/dapps.ts:34](url)*

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