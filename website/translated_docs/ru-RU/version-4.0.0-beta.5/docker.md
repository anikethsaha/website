---
id: version-4.0.0-beta.5-docker
title: Docker
original_id: docker
---

![alt Docker Pulls Count](https://dockeri.co/image/verdaccio/verdaccio "Docker Pulls Count")

Для скачивания последней версии [Docker образа](https://hub.docker.com/r/verdaccio/verdaccio/):

```bash
docker pull verdaccio/verdaccio
```

![Docker pull](assets/docker_verdaccio.gif)

<div id="codefund">''</div>

## Версии с меткой

Начиная с версии `v2.x` вы можете скачать Docker образ [тег](https://hub.docker.com/r/verdaccio/verdaccio/tags/), так:

Для базовых версий:

```bash
docker pull verdaccio/verdaccio:3
```

Для минорной версии:

```bash
docker pull verdaccio/verdaccio:3.0
```

Конкретная версия (патч):

```bash
docker pull verdaccio/verdaccio:3.0.1
```

Для следующей мастер-ветки используйте тэг `master`.

```bash
docker pull verdaccio/verdaccio:master
```

Для следующего мажорного релиза используйте тэг `4.x-next` (для ветки 4.x).

```bash
docker pull verdaccio/verdaccio:4.x-next
```

> Если вас интересует весь список тегов, [посетите нашу страницу на сайте Docker Hub](https://hub.docker.com/r/verdaccio/verdaccio/tags/).

## Запуск verdaccio с использованием Docker

Запуск Docker контейнера:

```bash
docker run -it --rm --name verdaccio -p 4873:4873 verdaccio/verdaccio
```

The last argument defines which image to use. The above line will pull the latest prebuilt image from dockerhub, if you haven't done that already.

Если этот образ [у вас уже скачан](#build-your-own-docker-image) используйте `verdaccio` в качестве последнего аргумента.

Вы можете использовать `-v` для того, что бы примонтировать каталоги `conf`, `storage` и `plugins` к основной (host) файловой системе:

```bash
V_PATH=/path/for/verdaccio; docker run -it --rm --name verdaccio -p 4873:4873 \
  -v $V_PATH/conf:/verdaccio/conf \
  -v $V_PATH/storage:/verdaccio/storage \
  -v $V_PATH/plugins:/verdaccio/plugins \
  verdaccio/verdaccio
```

> Примечание: внутри контейнера Verdaccio запускается не из под root (uid=100, gid=101), и если вы используете кастомное монтирование каталогов, вам необходимо убедиться, что у пользователя будет доступ к этим каталогам. В примере выше, вам нужно выполнить `sudo chown -R 100:101 /opt/verdaccio`, иначе вы получите ошибку прав доступа во время запуска контейнера. [Используйте docker разделы](https://docs.docker.com/storage/volumes/) при монтировании каталогов.

### Плагины

Плагины могут быть установлены в отдельную директорию и смонтированы с использованием Docker или Kubernetes. Однако вам нужно убедиться, что вы используете встроенные плагины с родными зависимостями, использующими такой же базовый образ как и в Verdaccio Dockerfile.

```docker
FROM verdaccio/verdaccio

RUN npm install verdaccio-s3-storage
```

### Docker и кастомная конфигурация порта

В настоящее время любой `host:port`, настроенный в `conf/config.yaml` в опции `listen` игнорируется при использовании Докер.

Если вам необходимо чтобы docker-экземпляр verdaccio работал на другом порту, скажем на `5000`, в вашей `docker run` команде нужно заменить `-p 4873:4873` на `-p 5000:4873`.

В том случае, когда вам нужно указать какой порт слушать **в docker контейнере**, начиная с версии 2.?.?, вы можете указать дополнительный аргумент в `docker run`: `--env PORT=5000` Это изменит порт, который docker контейнер будет слушать и порт, который будет слушать verdaccio.

Разумеется, числа указываемые в параметре `-p` должны соответствовать, по этому предпологается, что все они должны быть одинаковыми и вы можете скопировать, вставить и запускать:

```bash
PORT=5000; docker run -it --rm --name verdaccio \
  --env PORT -p $PORT:$PORT
  verdaccio/verdaccio
```

### Использование HTTPS с Docker

Вы можете настроить протокол, который verdaccio будет слушать, аналогично тому, как ранее конфигурировался порт. Необходмило переопределить значение по умолчанию ("http") переменной окружения `PROTOCOL` на "https", после того, как вы укажете сертификаты в config.yaml.

```bash
PROTOCOL=https; docker run -it --rm --name verdaccio \
  --env PROTOCOL -p 4873:4873
  verdaccio/verdaccio
```

### Использование docker-compose

1. Возьмите последнюю версию [docker-compose](https://github.com/docker/compose).
2. Соберите и запустите контейнер:

```bash
$ docker-compose up --build
```

Вы можете указать используемый порт (внутри контейнера и снаружи) дописав перед командой `PORT=5000`.

Docker сгенерирует именованный раздел, в котором будут храниться данные приложения. Вы можете использовать `docker inspect` или `docker volume inspect` для определения физического местоположения и изменения конфигурации, например:

```bash
$ docker volume inspect verdaccio_verdaccio
[
    {
        "Name": "verdaccio_verdaccio",
        "Driver": "local",
        "Mountpoint": "/var/lib/docker/volumes/verdaccio_verdaccio/_data",
        "Labels": null,
        "Scope": "local"
    }
]

```

## Сборка собственного Docker образа

```bash
docker build -t verdaccio .
```

Есть так же npm скрипт для сборки Docker образа, по этому вы можете выполнить:

```bash
npm run build:docker
```

Примечание: Первая сборки может занять несколько минут, потому что нужно выполнить `npm install`, это будет занимать много времени, всякий раз, как вы измените, что либо, что не перечислено в `.dockerignore`.

Имейте в виду, что для выполнения всех, представленных выше команд, Docker должен быть установлен на вашем компьютере и исполняемый файл должен быть представлен в переменной окружения `$PATH`.

## Примеры

Это отдельные репозитории, которые содержат многожество конфигураций для сборки Docker образов с `verdaccio`, для запуска обратного прокси:

<https://github.com/verdaccio/docker-examples>

## Пользовательские сборки

* [docker-verdaccio-gitlab](https://github.com/snics/docker-verdaccio-gitlab)
* [docker-verdaccio](https://github.com/deployable/docker-verdaccio)
* [docker-verdaccio-s3](https://github.com/asynchrony/docker-verdaccio-s3) Контейнер с приватным NPM, который может сохранять резервные копии на s3
* [docker-verdaccio-ldap](https://github.com/snadn/docker-verdaccio-ldap)
* [verdaccio-ldap](https://github.com/nathantreid/verdaccio-ldap)
* [verdaccio-compose-local-bridge](https://github.com/shingtoli/verdaccio-compose-local-bridge)
* [docker-verdaccio](https://github.com/Global-Solutions/docker-verdaccio)
* [verdaccio-docker](https://github.com/idahobean/verdaccio-docker)
* [verdaccio-server](https://github.com/andru255/verdaccio-server)
* [coldrye-debian-verdaccio](https://github.com/coldrye-docker/coldrye-debian-verdaccio) docker образ, предоставляющий verdaccio от coldrye-debian-nodejs.