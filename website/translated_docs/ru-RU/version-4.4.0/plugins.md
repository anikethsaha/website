---
id: version-4.4.0-plugins
title: Плагины
original_id: plugins
---

Verdaccio is an plugabble aplication. It can be extended in many ways, either new authentication methods, adding endpoints or using a custom storage.

Есть 4 типа плагинов:

* Аутентификация
* Middleware
* Хранилище
* UI тема

> Если вам нужно разработать свой плагин, почитайте секцию [разработка](dev-plugins.md).

## Использование

### Установка

```bash
$> npm install --global verdaccio-activedirectory
```

`verdaccio` as a sinopia fork it has backward compability with plugins that are compatible with `sinopia@1.4.0`. In such case the installation is the same.

```
$> npm install --global sinopia-memory
```

### Конфигурация

Open the `config.yaml` file and update the `auth` section as follows:

The default configuration looks like this, due we use a build-in `htpasswd` plugin by default that you can disable just commenting out the following lines.


### Конфигурирование аутентификации

```yaml
 htpasswd:
    file: ./htpasswd
    #max_users: 1000
```

and replacing them with (in case you decide to use a `ldap` plugin.

```yaml
auth:
  activedirectory:
    url: "ldap://10.0.100.1"
    baseDN: 'dc=sample,dc=local'
    domainSuffix: 'sample.local'
```

#### Несколько плагинов аутентификации

This is tecnically possible, making the plugin order important, as the credentials will be resolved in order.

```yaml
auth:
  htpasswd:
    file: ./htpasswd
    #max_users: 1000
  activedirectory:
    url: "ldap://10.0.100.1"
    baseDN: 'dc=sample,dc=local'
    domainSuffix: 'sample.local'
```

### Конфигурирование middleware

This is an example how to set up a middleware plugin. All middleware plugins must be defined in the **middlewares** namespace.

```yaml
middlewares:
  audit:
    enabled: true
```

> В качестве примера, можно посмотреть на [middleware-плагин для audit](https://github.com/verdaccio/verdaccio-audit).

### Конфигрирование хранилища

This is an example how to set up a storage plugin. All storage plugins must be defined in the **store** namespace.

```yaml
store:
  memory:
    limit: 1000
```

### Конфигурирование UI темы

Verdaccio allows to replace the User Interface with a custom one, we call it **theme**. По умолчанию, используется `@verdaccio/ui-theme`, который включен в поставку, но вы можете использовать что-нибудь другое, установив свой плагин.

```bash

$> npm install --global verdaccio-theme-dark

```

> Имя плагина должно начинаться с `verdaccio-theme`, иначе плагин не будет загружен.


Вы можете загрузить только одну тему. Так же, можно передать параметры в тему.

```yaml
theme:
  dark:
    option1: foo
    option2: bar
```

## Устаревшие плагины

### Sinopia Plugins

> Если вы используете плагин sinopia, помните, что все они - deprecated и могут перестать работать в будущем.

* [sinopia-npm](https://www.npmjs.com/package/sinopia-npm): auth plugin for sinopia supporting an npm registry.
* [sinopia-memory](https://www.npmjs.com/package/sinopia-memory): auth plugin for sinopia that keeps users in memory.
* [sinopia-github-oauth-cli](https://www.npmjs.com/package/sinopia-github-oauth-cli).
* [sinopia-crowd](https://www.npmjs.com/package/sinopia-crowd): auth plugin for sinopia supporting atlassian crowd.
* [sinopia-activedirectory](https://www.npmjs.com/package/sinopia-activedirectory): Active Directory authentication plugin for sinopia.
* [sinopia-github-oauth](https://www.npmjs.com/package/sinopia-github-oauth): authentication plugin for sinopia2, supporting github oauth web flow.
* [sinopia-delegated-auth](https://www.npmjs.com/package/sinopia-delegated-auth): Sinopia authentication plugin that delegates authentication to another HTTP URL
* [sinopia-altldap](https://www.npmjs.com/package/sinopia-altldap): Alternate LDAP Auth plugin for Sinopia
* [sinopia-request](https://www.npmjs.com/package/sinopia-request): An easy and fully auth-plugin with configuration to use an external API.
* [sinopia-htaccess-gpg-email](https://www.npmjs.com/package/sinopia-htaccess-gpg-email): Generate password in htaccess format, encrypt with GPG and send via MailGun API to users.
* [sinopia-mongodb](https://www.npmjs.com/package/sinopia-mongodb): An easy and fully auth-plugin with configuration to use a mongodb database.
* [sinopia-htpasswd](https://www.npmjs.com/package/sinopia-htpasswd): auth plugin for sinopia supporting htpasswd format.
* [sinopia-leveldb](https://www.npmjs.com/package/sinopia-leveldb): a leveldb backed auth plugin for sinopia private npm.
* [sinopia-gitlabheres](https://www.npmjs.com/package/sinopia-gitlabheres): Gitlab authentication plugin for sinopia.
* [sinopia-gitlab](https://www.npmjs.com/package/sinopia-gitlab): Gitlab authentication plugin for sinopia
* [sinopia-ldap](https://www.npmjs.com/package/sinopia-ldap): LDAP auth plugin for sinopia.
* [sinopia-github-oauth-env](https://www.npmjs.com/package/sinopia-github-oauth-env) Sinopia authentication plugin with github oauth web flow.

> All sinopia plugins should be compatible with all future verdaccio versions. Anyhow, we encourage contributors to migrate them to the modern verdaccio API and using the prefix as *verdaccio-xx-name*.

## Плагины Verdaccio

### Authorization Plugins

* [verdaccio-bitbucket](https://github.com/idangozlan/verdaccio-bitbucket): Bitbucket authentication plugin for verdaccio.
* [verdaccio-bitbucket-server](https://github.com/oeph/verdaccio-bitbucket-server): Bitbucket Server authentication plugin for verdaccio.
* [verdaccio-ldap](https://www.npmjs.com/package/verdaccio-ldap): LDAP auth plugin for verdaccio.
* [verdaccio-active-directory](https://github.com/nowhammies/verdaccio-activedirectory): Active Directory authentication plugin for verdaccio
* [verdaccio-gitlab](https://github.com/bufferoverflow/verdaccio-gitlab): use GitLab Personal Access Token to authenticate
* [verdaccio-gitlab-ci](https://github.com/lab360-ch/verdaccio-gitlab-ci): Enable GitLab CI to authenticate against verdaccio.
* [verdaccio-htpasswd](https://github.com/verdaccio/verdaccio-htpasswd): Auth based on htpasswd file plugin (built-in) for verdaccio
* [verdaccio-github-oauth](https://github.com/aroundus-inc/verdaccio-github-oauth): Github oauth authentication plugin for verdaccio.
* [verdaccio-github-oauth-ui](https://github.com/n4bb12/verdaccio-github-oauth-ui): GitHub OAuth plugin for the verdaccio login button.
* [verdaccio-groupnames](https://github.com/deinstapel/verdaccio-groupnames): Плагин для работы с группами с помощью синтаксиса `$group`. Лучше всего работает с плагином ldap.

### Middleware Plugins

* [verdaccio-audit](https://github.com/verdaccio/verdaccio-audit): verdaccio plugin for *npm audit* cli support (built-in) (compatible since 3.x)

* [verdaccio-profile-api](https://github.com/ahoracek/verdaccio-profile-api): verdacci plugin for *npm profile* cli support and *npm profile set password* for *verdaccio-htpasswd* based authentificaton

* [verdaccio-https](https://github.com/honzahommer/verdaccio-https) Verdaccio middleware-плагин для редиректа на https если установлен хэдер x-forwarded-proto

### Storage Plugins

* [verdaccio-memory](https://github.com/verdaccio/verdaccio-memory) Storage plugin to host packages in Memory
* [verdaccio-s3-storage](https://github.com/remitly/verdaccio-s3-storage) Storage plugin to host packages **Amazon S3**
* [verdaccio-google-cloud](https://github.com/verdaccio/verdaccio-google-cloud) Storage plugin to host packages **Google Cloud Storage**
* [verdaccio-minio](https://github.com/barolab/verdaccio-minio) A verdaccio plugin for storing data in Minio

## Предостережение

> Not all these plugins are been tested continuously, some of them might not work at all. Please if you found any issue feel free to notify the owner of each plugin.
