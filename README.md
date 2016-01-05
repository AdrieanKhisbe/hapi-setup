# hapi-setup

[![npm version][npm-badge]][npm-url]
[![Build Status][travis-badge]][travis-url]
[![Coverage Status][coveralls-badge]][coveralls-url]
[![Dependency Status][david-badge]][david-url]

hapi plugin that exposes a `setup` method for retrieving the server configuration. Provides information such as the version of Node running, the hapi server connections, routing tables per connection, and plugin information.

## Example Usage

The example below registers the `hapi-setup` plugin. In the handler for the `/about` route, the `hapi-setup.setup()` function is executed, which returns an object, and the result is then used to reply to the incoming request.

```js
var server = new Hapi.Server();
server.connection();

server.register(require('hapi-setup'), function (err) {
  server.route({
    method: 'GET',
    path: '/about',
    handler: function (request, reply) {
      reply(request.server.plugins['hapi-setup'].setup());
    }
  })
});

```

## API

### `setup()`

Exposed plugin method used for returning the setup information of the current hapi server.

Returns the following object:
- `runtime` - Object. Contains information about the Node process.
  - `versions` - Object. The version of Node, as well as Node's dependencies.
  - `execPath` - String. The path to the Node executable.
  - `argv` - Array of strings. Command line arguments used to start the server.
  - `execArgv` - Array of strings. Command line options passed to Node.
  - `env` - Object. Environment variables of the running process.
- `connections` - Array of objects. Contains information about individual hapi server connections. Each object contains the following keys.
  - `uri` - String. URI of the connection.
  - `labels` - Array of strings. Contains the labels associated with the connection.
  - `routes` - Array of objects. Each object represents a route registered on the connection, and contains the following keys.
    - `method` - String. HTTP verb of the route.
    - `path` - String. Path of the route.
    - `plugin` - String or `null`. The name of the plugin that registered the route. If a plugin did not register the route, then this is `null`.
  - `plugins` - Object. Each key represents the name of a registered plugin, and contains the following keys.
    - `name` - String. The name of the plugin.
    - `version` - String. The version of the plugin.
    - `multiple` - Boolean. `true` if the plugin can be registered multiple times. `false` otherwise.
    - `options` - Object. Contains the options passed to the plugin's `register()` function.
    - `attributes` - Object. Contains the attributes used when registering the plugin. This can include the entire contents of the plugin's `package.json`.

[npm-badge]: https://img.shields.io/npm/v/hapi-setup.svg
[npm-url]: https://npmjs.com/package/hapi-setup
[travis-badge]: https://api.travis-ci.org/continuationlabs/hapi-setup.svg
[travis-url]: https://travis-ci.org/continuationlabs/hapi-setup
[coveralls-badge]:https://coveralls.io/repos/continuationlabs/hapi-setup/badge.svg?branch=master&service=github
[coveralls-url]: https://coveralls.io/github/continuationlabs/hapi-setup?branch=master
[david-badge]: https://david-dm.org/continuationlabs/hapi-setup.svg
[david-url]: https://david-dm.org/continuationlabs/hapi-setup