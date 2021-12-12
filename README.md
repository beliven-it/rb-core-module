# rb-core-module

The [Restboard](https://restboard.github.io/) core module

## Getting started

First of all, you need to install the package in your project:

```bash
npm i --save rb-core-module
```

Now, you can start to define and use your [resources](#RbResource):

```js
import { createResourceManager, createResource } from 'rb-core-module'

// Create a resource manager (optional)
const registry = createResourceManager()

// Create a new resource
const users = createResource({
  name: 'users',
  provider: ..., // The data provider used to query the API
  ...
})

// Register the resource.
// Please note that registering a resource within a resource
// manager is convenient but absolutely not mandatory.
registry.registerResource(users)

// Resources can also be created and registered directly by
// the resource manager itself:
registry.createResource(...)

// Resources can then be used to interact with the remote API:
const me = await users.getOne(1)
```

## RbResource

`RbResource` is a base class used to implement a proxy to interact with a remote API resource.

### Options

| Name            | Description                                                   |
|-----------------|---------------------------------------------------------------|
| `name`          | The unique resource name (e.g. `users`)                       |
| `path`          | The resource base path (if different than `name`)             |
| `provider`      | The data provider used to interact with the API               |
| `key`           | The identifier attribute name. *Default: `id`*                |
| `label`         | A human-readable description label for the resource. *Default: capitalized name* |
| `displayAttr`   | The attr used as representation of a single resource instance |
| `stringify`     | A function used to get a human-readable reperesentation of a single resource instance. *Default: `instance => instance[resource.displayAttr]`* |
| `schema`        | The JSON schema representing the strcuture of resource instances |
| `updateSchema`  | The JSON schema used on update. *Default: `schema`*           |
| `createSchema`  | The JSON schema used on creation. *Default: `schema`*         |
| `defaultParams` | Default params passed to the data provider when fetching the API (e.g. default filters) |
| `isKeyEditable` | If `true`, allows editing the `key` of an instance. *Default: `false`* |
| `relations`     | A map of related child resources                              |
| `actions`       | A map of actions executable on a single resource instance     |
| `ui`            | An object containing UI-specific options and methods          |

### Methods

| Signature             | Description                                             |
|-----------------------|---------------------------------------------------------|
| `getKey(instance)`    | Retrieve the primary key of the given resource instance |
| `getMany(params)`     | Retrieve a list of resource instances according to the given `params` |
| `getOne(key, params)` | Retrieve a single resource instance, identified by `key` and `params` |
| `updateOne(key, data, params)` | Update a single resource instance, identified by `key` and `params`, with the given `data` |
| `updateMany(data, params)` | Update multiple resource instances according to `data` and `params` |
| `deleteOne(key, params)` | Delete a single resource instance identified by `key` and `params` |
| `deleteMany(keys, params)` | Delete multiple resource instances identified by the `keys` array and `params` |
| `setRelation(resource, name)` | Add a relation between the current and the given resources (identified by the given `name`, if passed) |
| `getRelation(key, name)` | Return the related resource identified by `name`, scoped to the instance identified by `key` |

## RbResourceManager

`RbResourceManager` is used as a registry to register and retrieve resources by name.

It provides the following methods:

| Signature                    | Description                                      |
|------------------------------|--------------------------------------------------|
| `createResource(opts)`       | Create and register a new resource               |
| `registerResource(resource)` | Register an existing resource                    |
| `getAllResources()`          | Retrieve a list of all registered resources      |
| `getResourceByName(name)`    | Retrieve the registered resource identified by `name` |
| `getAllResourceNames()`      | Retrieve a list of all registered resource names |

## RbDataProvider

`RbDataProvider` is a generic interface used by resources to interact with a
third-party API using the correct protocol and dialect.

The following methods should be implemented by all data providers:

| Signature                                    | Description                   |
|----------------------------------------------|-------------------------------|
| `getMany(resourcePath, params)`              | See [RbResource](#RbResource) |
| `getOne(resourcePath, key, params)`          | See [RbResource](#RbResource) |
| `updateOne(resourcePath, key, data, params)` | See [RbResource](#RbResource) |
| `updateMany(resourcePath, data, params)`     | See [RbResource](#RbResource) |
| `deleteOne(resourcePath, key, params)`       | See [RbResource](#RbResource) |
| `deleteMany(resourcePath, keys, params)`     | See [RbResource](#RbResource) |

## RbAuthProvider

`RbAuthProvider` is a generic interface used to perform authentication and
authorization over a third-party API, abstracting the details of underlying
strategies.

The following methods should be implemented by all auth providers:

| Signature            | Description                                      |
|----------------------|--------------------------------------------------|
| `login(credentials)` | Attempt to log the user identified by the given `credentials` |
| `logout()`           | Terminate the current authenticated session      |
| `checkAuth()`        | Check if the current authenticated session is still valid |
| `getIdentity(user)`  | Given a `user`, retrieve its textual representation |
| `getTenantIdentity(user)` | Given a `user`, retireve its tenant identity |
| `can(user, action, subject)` | Check if the given `user` can perform `action` on the `subject` |

## Development

```bash
# Install dependencies
npm install

# Run tests
npm test
```

## Contribute

If you want, you can also freely donate to fund the project development:

[![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donate_SM.gif)](https://paypal.me/EBertoldi)

## Have you found a bug?

Please open a new issue on:

<https://github.com/restboard/rb-core-module/issues>

## License

Copyright (c) Emanuele Bertoldi

[MIT License](http://en.wikipedia.org/wiki/MIT_License)
