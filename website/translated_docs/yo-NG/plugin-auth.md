---
id: plugin-auth
title: "Ohun elo Ifasẹsi"
---

## What's an Authentication Plugin?

Is a sort plugin that allows to handle who access or publish to a specific package. By default the `htpasswd` is built-in, but can easily be replaced by your own.

<div id="codefund">''</div>

 ## Getting Started

The authentication plugins are defined in the `auth:` section, as follows:

```yaml
auth:
  htpasswd:
    file: ./htpasswd
```

also multiple plugins can be chained:

```yaml
auth:
  htpasswd:
    file: ./htpasswd
  anotherAuth:
    foo: bar
    bar: foo
  lastPlugin:
    foo: bar
    bar: foo
```

> If one of the plugin in the chain is able to resolve the request, the next ones will be ignored.

## How to the authentication plugin works?

Ni pataki julọ a ni lati da ohun kan pada pẹlu ọna kan ti a n pe ni `ifasẹsi` ti o ma gba awọn ariyanjiyan mẹta (`olumulo, ọrọ igbaniwọle, ipe pada`).

On each request, `authenticate` will be triggered and the plugin should return the credentials, if the `authenticate` fails, it will fallback to the `$anonymous` role by default.

### API

```typescript
  interface IPluginAuth<T> extends IPlugin<T> {
    authenticate(user: string, password: string, cb: AuthCallback): void;
    adduser?(user: string, password: string, cb: AuthCallback): void;
    changePassword?(user: string, password: string, newPassword: string, cb: AuthCallback): void;
    allow_publish?(user: RemoteUser, pkg: AllowAccess & PackageAccess, cb: AuthAccessCallback): void;
    allow_access?(user: RemoteUser, pkg: AllowAccess & PackageAccess, cb: AuthAccessCallback): void;
    allow_unpublish?(user: RemoteUser, pkg: AllowAccess & PackageAccess, cb: AuthAccessCallback): void;
    apiJWTmiddleware?(helpers: any): Function;
  }
```
> Only `adduser`, `allow_access`, `apiJWTmiddleware`, `allow_publish`  and `allow_unpublish` are optional, verdaccio provide a fallback in all those cases.

#### `apiJWTmiddleware` method

Lati `v4.0.0`

`apiJWTmiddleware` jẹ sisafihan lori [PR#1227](https://github.com/verdaccio/verdaccio/pull/1227) lati le ni iṣakoso ti olutọju aami ni kikun, fifagbara bori ọna yii ma yọ atilẹyin `login/adduser`. A ṣe igbaniyanju pe ki o ma se ṣe amulo ọna yii ayafi ti o ba pọn dandan. Wo apẹẹrẹ ni kikun kan [nibi](https://github.com/verdaccio/verdaccio/pull/1227#issuecomment-463235068).


## What should I return in each of the methods?

Verdaccio relies on `callback` functions at time of this writing. Each method should call the method and what you returns is important, let's review how to do it.


### `authentication` callback

Lọgan ti ifasẹsi naa ti waye awọn aṣayan meji lo wa lati fun `verdaccio` ni esi.

##### If the authentication fails

If the auth was unsuccessful, return `false` as the second argument.

```typescript
callback(null, false)
```

##### If the authentication success

Ifasẹsi naa jẹ aṣeyọri.


`awọn ẹgbẹ` jẹ oriṣi eto ti awọn okun nibi ti olumulo naa ti jẹ ara ti.

```
 callback(null, groups);
```

##### If the authentication produce an error

The authentication service might fails, and you might want to reflect that in the user response, eg: service is unavailable.

```
 import { getInternalError } from '@verdaccio/commons-api';

 callback(getInternalError('something bad message), null);
```

> A failure on login is not the same as service error, if you want to notify user the credentails are wrong, just return `false` instead string of groups. The behaviour mostly depends of you.


### `adduser` callback

##### If adduser success

If the service is able to create an user, return `true` as the second argument.

```typescript
callback(null, true)
```

##### If adduser fails

Any other action different than success must return an error.

```typescript
import { getConflict } from '@verdaccio/commons-api';

const err = getConflict('maximum amount of users reached');

callback(err);
```

### `changePassword` callback

##### If the request success

If the service is able to create an user, return `true` as the second argument.

```typescript
const user = serviceUpdatePassword(user, password, newPassword);

callback(null, user)
```

##### If the request fails

Any other action different than success must return an error.

```typescript
import { getNotFound } from '@verdaccio/commons-api';

 const err = getNotFound('user not found');

callback(err);
```

### `allow_access`, `allow_publish`, or `allow_unpublish` callback

These methods aims to allow or deny trigger some actions.

##### If the request success

If the service is able to create an user, return a `true` as the second argument.

```typescript

allow_access(user: RemoteUser, pkg: PackageAccess, cb: Callback): void {
  const isAllowed: boolean = checkAction(user, pkg);

  callback(null, isAllowed)
}
```

##### If the request fails

Any other action different than success must return an error.

```typescript
import { getNotFound } from '@verdaccio/commons-api';

 const err = getForbidden('not allowed to access package');

callback(err);
```

## Generate an authentication plugin

For detailed info check our [plugin generator page](plugin-generator). Run the `yo` command in your terminal and follow the steps.

```
➜ yo verdaccio-plugin

Just found a `.yo-rc.json` in a parent directory.
Setting the project root at: /Users/user/verdaccio_yo_generator

     _-----_     ╭──────────────────────────╮
    |       |    │        Welcome to        │
    |--(o)--|    │ generator-verdaccio-plug │
   `---------´   │   in plugin generator!   │
    ( _´U`_ )    ╰──────────────────────────╯
    /___A___\   /
     |  ~  |
   __'.___.'__
 ´   `  |° ´ Y `

? What is the name of your plugin? service-name
? Select Language typescript
? What kind of plugin you want to create? auth
? Please, describe your plugin awesome auth plugin
? GitHub username or organization myusername
? Author's Name Juan Picado
? Author's Email jotadeveloper@gmail.com
? Key your keywords (comma to split) verdaccio,plugin,auth,awesome,verdaccio-plugin
   create verdaccio-plugin-authservice-name/package.json
   create verdaccio-plugin-authservice-name/.gitignore
   create verdaccio-plugin-authservice-name/.npmignore
   create verdaccio-plugin-authservice-name/jest.config.js
   create verdaccio-plugin-authservice-name/.babelrc
   create verdaccio-plugin-authservice-name/.travis.yml
   create verdaccio-plugin-authservice-name/README.md
   create verdaccio-plugin-authservice-name/.eslintrc
   create verdaccio-plugin-authservice-name/.eslintignore
   create verdaccio-plugin-authservice-name/src/index.ts
   create verdaccio-plugin-authservice-name/index.ts
   create verdaccio-plugin-authservice-name/tsconfig.json
   create verdaccio-plugin-authservice-name/types/index.ts
   create verdaccio-plugin-authservice-name/.editorconfig

I'm all done. Running npm install for you to install the required dependencies. If this fails, try running the command yourself.


⸨ ░░░░░░░░░░░░░░░░░⸩ ⠋ fetchMetadata: sill pacote range manifest for @babel/plugin-syntax-jsx@^7.7.4 fetc
```

After the install finish, access to your project scalfold.

```
➜ cd verdaccio-plugin-service-name
➜ cat package.json

  {
  "name": "verdaccio-plugin-service-name",
  "version": "0.0.1",
  "description": "awesome auth plugin",
  ...
```

## Full implementation ES5 example

```javascript
function Auth(config, stuff) {
  var self = Object.create(Auth.prototype);
  self._users = {};

  // config for this module
  self._config = config;

  // verdaccio logger
  self._logger = stuff.logger;

  // pass verdaccio logger to ldapauth
  self._config.client_options.log = stuff.logger;

  return self;
}

Auth.prototype.authenticate = function (user, password, callback) {
  var LdapClient = new LdapAuth(self._config.client_options);
  ....
  LdapClient.authenticate(user, password, function (err, ldapUser) {
    ...
    var groups;
     ...
    callback(null, groups);
  });
};

module.exports = Auth;
```

Atipe iṣeto naa yoo dabi:

```yaml
auth:
  htpasswd:
    file: ./htpasswd
```

Nibi ti `htpasswd` ti jẹ afikun ipari ti orukọ ohun elo naa. fun apẹẹrẹ: `verdaccio-htpasswd` ati awọn iyoku ti ara ma jẹ awọn odiwọn iṣeto ohun elo naa.

### List Community Authentication Plugins

* [verdaccio-bitbucket](https://github.com/idangozlan/verdaccio-bitbucket): Ohun elo ifasẹsi Bitbucket fun verdaccio.
* [verdaccio-bitbucket-server](https://github.com/oeph/verdaccio-bitbucket-server): Ohun elo ifasẹsi Olupese ti Bitbucket fun verdaccio.
* [verdaccio-ldap](https://www.npmjs.com/package/verdaccio-ldap): Ohun elo ifasẹsi LDAP fun sinopia.
* [verdaccio-active-directory](https://github.com/nowhammies/verdaccio-activedirectory): Ohun elo ifasẹsi Active Directory fun verdaccio
* [verdaccio-gitlab](https://github.com/bufferoverflow/verdaccio-gitlab): lo Aami Iwọlesi Aladani ti GitLab lati se ifasẹsi
* [verdaccio-gitlab-ci](https://github.com/lab360-ch/verdaccio-gitlab-ci): Ṣe imuṣiṣẹ GitLab CI lati se ifasẹsi ni ilodi si verdaccio.
* [verdaccio-htpasswd](https://github.com/verdaccio/verdaccio-htpasswd): Ifasẹsi to da lori ohun elo faili htpasswd (alakọ-sinu) fun verdaccio
* [verdaccio-github-oauth](https://github.com/aroundus-inc/verdaccio-github-oauth): Ohun elo ifasẹsi Github oauth fun verdaccio.
* [verdaccio-github-oauth-ui](https://github.com/n4bb12/verdaccio-github-oauth-ui): Ohun elo GitHub OAuth fun bọtini iwọle verdaccio naa.
* [verdaccio-groupnames](https://github.com/deinstapel/verdaccio-groupnames): Ohun elo lati mojuto awọn ẹgbẹ akojọ alaidurolojukan ti nipa lilo sintasi `$group`. O ma n ṣiṣẹ dara julọ pẹlu ohun elo ldap.

**Have you developed a new plugin? Add it here !**
