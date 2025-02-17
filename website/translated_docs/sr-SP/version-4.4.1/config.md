---
id: version-4.4.1-configuration
title: Фајл за конфигурисање
original_id: конфигурисање
---

Овај фајл је основа verdaccio-a. У оквиру њега, можете вршити измене задатих подешавања, можете активирати plugin-е и спољашње ресурсе (features).

A default configuration file `config.yaml` is created the very first time you run `verdaccio`.

<div id="codefund">''</div>

## Подразумеване поставке (Default Configuration)

The default configuration has support for **scoped** packages and allow any user to access all packages but only **authenticated users to publish**.

```yaml
storage: ./storage
auth:
  htpasswd:
    file: ./htpasswd
uplinks:
  npmjs:
    url: https://registry.npmjs.org/
packages:
  '@*/*':
    access: $all
    publish: $authenticated
    proxy: npmjs
  '**':
    proxy: npmjs
logs:
  - {type: stdout, format: pretty, level: http}
```

## Секције

Секција у наставку даје објашњења за свако својство и опцију.

### Меморија за складиштење

Storage је локација у коју се смештају подаци. **Verdaccio is by default based on local file system**.

```yaml
storage: ./storage
```

### Plugins

Ово је локација директоријума плугина. Ова опција је јако корисна за Docker/Kubernetes deployment.

```yaml
plugins: ./plugins
```

### Authentification

Овде се врши подешавање (set up). Подразумевана auth је базирана на `htpasswd` и већ је уграђена. Можете извршити модификације начина рада (behaviour) путем [plugin-a](plugins.md). За више информација о овој секцији, прочитајте [auth страну](auth.md).

```yaml
auth:
  htpasswd:
    file: ./htpasswd
    max_users: 1000
```

### Сигурност

<small>Since: `verdaccio@4.0.0` [#168](https://github.com/verdaccio/verdaccio/pull/168)</small>

Сигурносни блок Вам омогућава да прилагодите потпис за токен (token signature). Како бисте [JWT (json web token) учинили активним ](https://jwt.io/) нови потпис који требате за додавање блока `jwt` у `api` секцију,  `web` по правилу користи `jwt`.

Конфигурација је подељена у две секције, `api` и `web`. Како бисте користили JWT у `api`, морате га дефинисати, у супротном ће користити наслеђени потпис за токен (legacy token signature) (`aes192`). За JWT можете прилагодити [потпис](https://github.com/auth0/node-jsonwebtoken#jwtsignpayload-secretorprivatekey-options-callback) и токен [верификацију](https://github.com/auth0/node-jsonwebtoken#jwtverifytoken-secretorpublickey-options-callback) уписивањем параметара по својој жељи.

```
security:
  api:
    legacy: true
    jwt:
      sign:
        expiresIn: 29d
      verify:
        someProp: [value]
   web:
     sign:
       expiresIn: 7d # 7 days by default
     verify:
        someProp: [value]
```
> Јако Вам препоручујемо да се пребаците на JWT пошто је legacy signature (`aes192`) застарео и неће га бити у новијим верзијама.

### Сервер

Скуп својстава за мењање понашања сервер апликације, посебно API-ја (Express.js).

> Можете задати да HTTP/1.1 сервер одржава време после којег се буди за долазне конекције. Ако задате вредност 0, http сервер ће се понашати слично као Node.js верзије старије од 8.0.0, које нису имале уграђену функцију: keep-alive timeout. ЗАОБИЛАЖЕЊЕ: Датим конфигурисањем, можете заобићи следећи проблем: https://github.com/verdaccio/verdaccio/issues/301. Set to 0 in case 60 is not enough.

```yaml
server:
  keepAliveTimeout: 60
```


### Web UI (кориснички интерфејс)

Ово својство Вам омогућава да измените изглед и функционалност Веб Корисничког Интерфејса. За више информација о овој секцији прочитајте [web ui страну](web.md).

```yaml
web:
  enable: true
  title: Verdaccio
  logo: logo.png
  scope:
```

### Uplinks

Uplinks пружају могућност систему да хвата (fetch) пакете из удаљених регистрија ако ти пакети нису локално доступни. За више информација о овој секцији прочитајте на [uplinks страници](uplinks.md).


```yaml
uplinks:
  npmjs:
    url: https://registry.npmjs.org/
```

### Пакети

Под Пакетима (Packages) имате контролу над тиме како ће се приступати пакетима. За више информација о овој секцији, прочитајте на [packages страници](packages.md).


```yaml
packages:
  '@*/*':
    access: $all
    publish: $authenticated
    proxy: npmjs
```

## Напредна подешавања

### Публиковање offline

By default `verdaccio` does not allow to publish when the client is offline, that behavior can be overridden by setting this to *true*.

```yaml
publish:
  allow_offline: false
```


<small>Since: `verdaccio@2.3.6` due [#223](https://github.com/verdaccio/verdaccio/pull/223)</small>

### URL Префикс

```yaml
url_prefix: /verdaccio/
```

> We recommend use a subdirectory `/verdaccio/` instead a URI.

### Максимална величина body секције документа

Према задатим подешавањима, максимална величина за body JSON документа је `10mb`. Ако добијете грешку `"request entity too large"` могли бисте да повећате ову вредност.

```yaml
max_body_size: 10mb
```

### Listen Порт

`verdaccio` се по правилу покреће на порту `4873`. Промена порта се може обавити преко [cli](cli.md) или у фајлу за конфигурисање, а опције из наведеног примера су валидне.

```yaml
listen:
# - localhost:4873            # default value
# - http://localhost:4873     # same thing
# - 0.0.0.0:4873              # listen on all addresses (INADDR_ANY)
# - https://example.org:4873  # if you want to use https
# - "[::1]:4873"                # ipv6
# - unix:/tmp/verdaccio.sock    # unix socket
```

### HTTPS

To enable `https` in `verdaccio` it's enough to set the `listen` flag with the protocol *https://*. Више детаља можете наћи на [ssl страници](ssl.md).


```yaml
https:
    key: ./path/verdaccio-key.pem
    cert: ./path/verdaccio-cert.pem
    ca: ./path/verdaccio-csr.pem
```

### Proxy

Proxies су HTTP сервери посебне намене дизајнирани да преносе податке од удаљених сервера до локалних клијената.

#### http_proxy i https_proxy

Ако имате proxy у својој мрежи, можете подесити `X-Forwarded-For` header користећи следеће уносе за својствa (properties).

```yaml
http_proxy: http://something.local/
https_proxy: https://something.local/
```

#### no_proxy

Ова варијабла би требало да садржи comma-separated (поља одвојена запетом) листу екстензија домена за коју proxy не би требало да се користи.

```yaml
no_proxy: localhost,127.0.0.1
```

### Нотификације

Давање дозвола алатима других произвођача (third-party) је релативно једноставно путем технике web hooks. За више информација о овој секцији, прочитајте на [notifications страници](notifications.md).

```yaml
notify:
  method: POST
  headers: [{'Content-Type': 'application/json'}]
  endpoint: https://usagge.hipchat.com/v2/room/3729485/notification?auth_token=mySecretToken
  content: '{"color":"green","message":"New package published: * {{ name }}*","notify":true,"message_format":"text"}'
```


> За детаљније опције подешавања, молимо Вас да [погледате source code](https://github.com/verdaccio/verdaccio/tree/master/conf).


### Audit (ревизија)

<small>Since: `verdaccio@3.0.0`</small>

`npm audit` је нова команда доступна од верзије [npm 6.x](https://github.com/npm/npm/releases/tag/v6.1.0). Verdaccio укључује и уграђени middleware plugin (помоћни software) како би извршио дату команду.

> Ако имате нову инсталацију, све је већ укључено у оквиру ње. У супротном, треба да додате наведене додатке (props) у Ваш config фајл

```yaml
middlewares:
  audit:
    enabled: true
```

### Experiments

This release includes a new property named `experiments` that can be placed in the `config.yaml` and is completely optional.

We want to be able to ship new things without affecting production environments. This flag allows us to add new features and get feedback from the community that wants to use them.

The features that are under this flag might not be stable or might be removed in future releases.

Here one example:

```yaml
experiments:
  token: false
```

> To disable the experiments warning in the console, you must comment out the whole `experiments` section.
