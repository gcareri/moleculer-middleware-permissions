# Moleculer Middleware Permissions

Check action permissions.

[![codecov](https://codecov.io/gh/Embraser01/moleculer-middleware-permissions/branch/master/graph/badge.svg)](https://codecov.io/gh/Embraser01/moleculer-middleware-permissions)
[![Travis (.com)](https://img.shields.io/travis/com/Embraser01/moleculer-middleware-permissions.svg)](https://github.com/Embraser01/moleculer-middleware-permissions)
![NpmLicense](https://img.shields.io/npm/l/moleculer-middleware-permissions.svg)
![npm](https://img.shields.io/npm/v/moleculer-middleware-permissions.svg)
![node](https://img.shields.io/node/v/moleculer-middleware-permissions.svg)


## Install

> This module requires at least Node v8.3.0.

```bash
yarn add moleculer-middleware-permissions
```

## Usage

```js
// moleculer.config.js
const PermissionGuard = require('moleculer-middleware-permissions');

const guard = new PermissionGuard({options});

module.exports = {
    ...
    middlewares: [
        guard.middleware(),
    ],
};
```

```js
// service.js
module.exports = {
  name: 'awesome.service',
   actions: {
       hello: {
         // The user must have both 'hello:read' AND 'hello:name'
         // You can override this behaviour by passing your 'checkFunction'
         permissions: ['hello:read', 'hello:name'],
         handler (ctx) {
           const {name} = ctx.params;
           return `Hello ${name}`;
         }
       },
       me: {
          // Will check for these permissions: ['awesome.service.me']
          permissions: true,
          handler (ctx) {
            return `Hello me`;
          }
        }
     }
};
```

# Get a list of permissions

You can get the list of every permissions declared in services using the middleware.
It can be useful to dynamically handle permissions.

It uses the [`serviceCreated`](https://moleculer.services/docs/0.13/middlewares.html) middleware.

```js
const PermissionGuard = require('moleculer-middleware-permissions');

const guard = new PermissionGuard();
// Add it as middleware
[...]

// Get all permissions after the broker is created
const perms = guard.getPermissions();
```

It returns an _Array_ with every permissions checked by the middleware (without duplication).

# Options

- `checkFunction`: A function that return `true` if the request has enough permissions.
    Else, the return value will be send in the rejected `PermissionError`.
    For the default behaviour search for `basicPermissionCheck` in `src/index.js`.
- `permissionsPath`: Path to look for the request permissions (from the `ctx` object).
    Default to `meta.user.permissions`,
- `permissionsSep`: Separator used to transform the action name to a permission name (default: `.`).
- `pathSeparator`: Separator to use when there is a `.` in a property.

# License

MIT
