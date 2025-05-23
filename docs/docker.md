# Docker

# Содержание

* [Начало работы](#начало-работы)
    * [Установка](#установка)
    * [Проверка установки](#проверка-установки)
    * [Настройка после установки](#настройка-после-установки)

* [Словарь](#словарь)

* [Команды Docker](#команды-docker)
    * [Клонирование образа (pull)](#клонирование-образа)
    * [Поиск образа (search)](#просмотр-контейнеров)
    * [Просмотр контейнеров (ps, inspect, logs)](#просмотр-контейнеров)
    * [Запуск контейнера (run)](#запуск-контейнера)
    * [Повторный запуск контейнера (start)](#повторный-запуск-контейнера)
    * [Присоединение к контейнеру (attach)](#присоединение-к-контейнеру)
    * [Остановка контейнера (stop, kill)](#остановка-контейнера)
    * [Удаление контейнера или образа (rm)](#удаление-контейнера-или-образа)
    * [Запуск команд в контейнере (exec)](#запуск-команд-в-контейнере)
    * [Сохранение изменений в контейнере (commit)](#сохранение-изменений-в-контейнере)
    * [Отправка данных на Docker Hub (login, push)](#отправка-данных-на-docker-hub)
    * [Копирование файлов (cp)](#копирование-файлов)
    * [Информация об использовании диска (system df)](#информация-об-использовании-диска)
    * [Очистка Docker (system df)](#очистка-docker)

* [Dockerfile](#dockerfile)
    * [FROM](#from)
    * [LABEL](#label)
    * [ENV](#env)
    * [WORKDIR](#workdir)
    * [COPY](#copy)
    * [ADD](#add)
    * [RUN](#run)
    * [CMD](#cmd)
    * [ENTRYPOINT](#entrypoint)
    * [VOLUME](#volume)
    * [EXPOSE](#expose)
    * [STOPSIGNAL](#stopsignal)
    * [USER](#user)
    * [ARG](#arg)
    * [HEALTHCHECK](#healthcheck)
    * [Сборка проекта](#сборка-проекта)
    * [Уменьшение размера образа](#уменьшение-размера-образа)
    * [Многоэтапная сборка (multi-stage builds)](#многоэтапная-сборка)
    * [Dockerignore](#dockerignore)

* [Docker Compose](#docker-compose)
    * [Установка Docker Compose](#установка-docker-compose)

* [Команды Docker Compose](#команды-docker-compose)
    * [Сборка проекта Docker Compose (build)](#сборка-проекта-docker-compose)
    * [Запуск проекта Docker Compose (up)](#запуск-проекта-docker-compose)
    * [Остановка проекта Docker Compose (stop, down)](#остановка-проекта-docker-compose)
    * [Просмотр состояния Docker Compose (ps, logs, images)](#просмотр-состояния-docker-compose)
    * [Отображение запущенных процессов Docker Compose (top)](#отображение-запущенных-процессов-docker-compose)
    * [Просмотр образов Docker Compose (images)](#просмотр-образов-docker-compose)
    * [Выполнение команды внутри контейнера Docker Compose (exec)](#выполнение-команды-внутри-контейнера-docker-compose)
    * [Просмотр версии Docker Compose (version)](#просмотр-версии-docker-compose)
    * [Прочие команды Docker Compose](#прочие-команды-docker-compose)

* [Docker Compose file](#docker-compose-file)
    * [version](#version)
    * [services](#services)
    * [build](#build)
    * [image](#image)
    * [ports](#ports)
    * [expose](#expose)
    * [env_file](#env_file)
    * [environment](#environment)
    * [volumes](#volumes)
    * [container_name](#container_name)
    * [entrypoint](#entrypoint)
    * [command](#command)
    * [depends_on](#depends_on)
    * [restart](#restart)
    * [healthcheck](#healthcheck)
    * [extends](#extends)

* [Полезные инструменты](#полезные-инструменты)


# Начало работы

## Установка

[Наверх](#содержание)

[Начальная установка Docker](https://docs.docker.com/engine/install/).

Для Windows и Mac OS систем, требуется установить Docker Desktop c
[официального сайта](https://www.docker.com/products/docker-desktop/).


## Настройка после установки

[Наверх](#содержание)

Для Linux, чтобы каждый раз не нужно было писать sudo.

    sudo groupadd docker
    sudo usermod -aG docker $USER

И дальше требуется перезагрузить компьютер или перелогиниться,
чтобы изменения вступили в силу.

Если требуется, чтобы Docker daemon автоматически загружался
при старте операционной системы нужно указать это в настройках (systemd).

    sudo systemctl enable docker.service
    sudo systemctl enable containerd.service

Для отключения этого.

    sudo systemctl disable docker.service
    sudo systemctl disable containerd.service


## Проверка установки

Для проверки работы Docker.

    docker run hello-world
    docker ps -a

И после этого должен отображаться контейнер hello-world.

Дальше этот образ не требуется, поэтому его можно удалять.

    docker rm $(docker ps -aq)
    docker rmi hello-world


# Словарь

[Наверх](#содержание)

* `host` — операционная система, на которую устанавливают Docker и на которой он работает.
* `daemon` — служба, которая управляет Docker-объектами:
сетями, хранилищами, образами и контейнерами.
* `client` — консольный клиент, при помощи которого пользователь
взаимодействует с Docker daemon и отправляет ему команды.
* `image` — это неизменяемый образ, из которого разворачивается контейнер.
* `container` — развёрнутое и запущенное приложение.
* `volumes` - тома для постоянного хранения информации.
* `registry` — репозиторий, в котором хранятся образы.
* `Docker Hub` - Популярный публичный репозиторий, используемый по умолчанию в Docker.
* `Dockerfile` — файл-инструкция для сборки образа.
* `Docker Compose` — инструмент для управления несколькими контейнерами.
Он позволяет создавать контейнеры и задавать их конфигурацию.
* `Docker Desktop` — Это GUI-клиент, который отображает все сущности Docker.
Работает на Linux, macOS и Windows.


# Команды Docker

## Клонирование образа

[Наверх](#содержание)

Команда для клонирования образа.

    docker pull <image-name>

По стандарту будет скачиваться latest версия,
но через двоеточие можно указать версию image.

    docker pull <image-name>:<version>

Склонированный образ является неизменяемым.

*Примеры:*

Клонирование образа nginx.

    docker pull nginx:1.23.1

Клонирование образа debian.

    docker pull debian:11.4-slim


## Поиск образа

[Наверх](#содержание)

Для поиска образа используется команда `search`.
Она будет выводить все найденные варианты с Docker Hub.

    docker search <image-name>

Официальный сайт, где можно найти все контейнеры и описание к ним
[Docker Hub](https://hub.docker.com/search?q=).

*Примеры:*

Поиск образов для debian.

    docker search debian


## Просмотр контейнеров

[Наверх](#содержание)

Команда `docker ps` показывает все запущенные контейнеры.

Опции:
* `a` - показать все контейнеры, даже остановленные.
* `s` - показать размер контейнера.
* `q` - показать только id контейнера.

*Примеры:*

Показать запущенные контейнеры.

    docker ps

Показать все контейнеры.

    docker ps -a

Показать все контейнеры и их размер.

    docker ps -as

Вывести id запущенных контейнеров.

    docker ps -q


## Логи контейнера

[Наверх](#содержание)

Для просмотра логов, которые отдаёт контейнер есть атрибут `logs`.

    docker logs <container-name>
    docker logs <container-hash>


## Сборка проекта

[Наверх](#содержание)

Сборка всего проекта в один контейнер.

Опции.

* `-t / --tag` - тег для контейнера.
* `-f / --file` - путь до Dockerfile.
* `--squash` - все слои будут соединены в один.

Точка в конце указывает текущую директорию как место сборки.
Также можно указать путь до директории.

    docker build -t <username>/<container-name>:<vesion> .

Полная документация по
[docker build](https://docs.docker.com/engine/reference/commandline/build/).

*Примеры:*

Сборка контейнера с именем.

    docker build -t kotdimos/my_nginx:1.23.1 .

Сборка контейнера из определённого Dockerfile.

    docker build -t kotdimos/my_nginx:1.23.1 -f Dockerfile.nginx .


## Просмотр информации об объекте

[Наверх](#содержание)

`inspect` - позволяет просмотреть низкоуровневую информацию
о различных объектах в docker.

    docker inspect <object>

*Примеры:*

Просмотреть данные контейнера.

    docker inspect 852e9526a135

Просмотреть данные образа.

    docker inspect mysql:8.0.31

Просмотреть данные network.

    docker inspect network_deafault

Просмотреть данные volume.

    docker inspect database


## Просмотр образов

[Наверх](#содержание)

Для просмотра существующих образов.

    docker images


## Запуск контейнера

[Наверх](#содержание)

Изначальный запуск контейнера.
Если образ никогда не был установлен,
то сначала Docker скачает образ, а потом запустит его.

    docker run <image-name>:<version>

Для работы внутри контейнера требуется указать
флаги `-it` (-i interactive, -t - tty).

    docker run -it <image-name>:<version>

Для старта работы контейнера в фоновом режиме
нужно использовать флаг `-d` (-d - detach).

    docker run -d <image-name>:<version>

При обычном запуске генерируется рандомное имя контейнера.
Для того, чтобы дать самостоятельно имя требуется использовать флаг `--name`.

    docker run -it --name <container-name> <image-name>:<version>

Для того, чтобы контейнеры взаимодействовали с основной машиной,
нужно установить им порты, по которым они будут взаимодействовать.
Для этого используется флаг `-p`,
и после этого указывается порт вашей машины,
и порт, который будет в докер контейнере.

    docker run -d -p 8080:8080 <image-name>:<version>

Для того, чтобы передать контейнеру файлы основной машины,
можно воспользоваться флагом `-v`, где требуется указать директорию,
которая будет монтироваться, и потом, куда она будет монтироваться.

    docker run -it -v <path-to-host-directory>:<path-to-container-directory> <image-name>:<version>

*Примеры:*

Запуск nginx.

    docker run nginx:1.23.1

Проброс портов для nginx.

    docker run -p 80:80 nginx:1.23.1

Добавление имени к контейнеру.

    docker run --name my_nginx -p 80:80 nginx:1.23.1


## Повторный запуск контейнера

[Наверх](#содержание)

Если использовать `run`, то Docker будет создавать новый контейнер.
Но если требуется запустить контейнер, который уже когда-то использовался,
используется команда `start`.
Запускать контейнер можно как и по имени контейнера, так и по его id.

    docker start -it <container-name>
    docker start -it <container-id>


## Присоединение к контейнеру

[Наверх](#содержание)

Команда для присоединения к рабочему контейнеру.

    docker attach <container-name>
    docker attach <container-id>

Для выхода из него обычно используется `exit`.


## Остановка контейнера

[Наверх](#содержание)

Для остановки контейнера используется команда `stop`.

    docker stop <container-name>
    docker stop <container-id>

Для остановки всех контейнеров.

    docker stop $(docker ps -aq)

Принудительное завершение контейнера.

    docker kill <container-name>
    docker kill <container-id>


## Удаление контейнера или образа

[Наверх](#содержание)

Для удаления контейнера нужно использовать rm,
удалять можно как и по имени контейнера, так и по его id.

    docker rm <container-id>
    docker rm <container-name>

Если требуется удалить все контейнеры.

    docker rm $(docker ps -aq)

Если требуется удалить контейнеры по имени образа.

    docker rm $(docker ps -a | awk '$2=="<image-name>" { print $1 }')

Для удаления образа нужно использовать rmi,
при этом все контейнеры основанные на нём должны быть удалены.

    docker rmi <image-name>

Для удаления всех образов.

    docker rmi $(docker images -q)


## Запуск команд в контейнере

[Наверх](#содержание)

Для запуска команд используется команда `exec`.

    docker exeс -it <container-name> <command>
    docker exeс -it <container-id> <command>

Если нужно запусить команду под определённым пользователем используется флаг `-u`.

    docker exec -u <user-id> -it <container-id> <command>
    docker exec -u <user-name> -it <container-id> <command>

*Примеры:*

Просмотреть корректность заполнения конфига в nginx.

    docker exec -it 17911b46528e nginx -t

Вход под root пользователем в контейнере.

    docker exec -u 0 -it b6910f29ad52 /bin/bash


## Сохранение изменений в контейнере

[Наверх](#содержание)

Для сохранения изменений требуется его закоммитить.

    docker commit <container-name> <username>/<container-name>:<vesion>
    docker commit <container-id> <username>/<container-name>:<vesion>


## Отправка данных на Docker Hub

[Наверх](#содержание)

Для отправки данных требуется авторизация

    docker login

После этой команды консоль запрашивает логин и пароль для авторизации
на сайте [docker](https://hub.docker.com/).

После этого можно передать данные на Docker Hub.

    docker push


## Копирование файлов

[Наверх](#содержание)

Команда `cp` позволяет копировать файлы к контейнер и из контейнера.

Опции:
* `-a / --archive` - копирование всей инфромации (uid/gid).
* `-L / --follow-link` - следовать символьной ссылке.
* `-q / --quiet` - подавить вывод во время копирования.

*Примеры:*

Копирование файла в контейнер.

    docker cp ./file_name container1:/home/user/file_name

Копирование файла из контейнера.

    docker cp container1:/home/user/file_name ./file_name


## Информация об использовании диска

[Наверх](#содержание)

Команда, для того чтобы получить сколько использует docker дискового пространства.

    docker system df

Подробная информация об каждом объекте.

    docker system df -v


## Очистка Docker

[Наверх](#содержание)

Docker при долгом использовании очень сильно загрезняет диск.

Очистка всего docker от мусора.

    docker system prune -a

Будут удалены:
* все остановленные контейнеры.
* все сети, не используемые хотя бы одним контейнером.
* все тома, не используемые хотя бы одним контейнером.
* все образы, с которыми не связан хотя бы один контейнер.
* весь кэш сборки.

Такой вариант может быть довольно жёстким для очищения.


Отдельное удалеение:

Удаление всех dangling образов.

    docker image prune

Удаление всех неиспользуемых сетей.

    docker network prune

Удаление volumes.

    docker volume prune

Удаление всех остановленных контейнеров.

    docker container prune


# Dockerfile

[Наверх](#содержание)

Dockerfile является настройкой собственного контейнера.
Каждая новая инструкция будет создавать новый слой контейнера.

[Документация по всем командам](https://docs.docker.com/engine/reference/builder/).


## FROM

[Наверх](#содержание)

Наследование от другого образа, с которого будет происходить старт.

    FROM <image-name>:<version>

*Примеры.*

    FROM ubuntu:20.04

    FROM debian:11.4-slim


## LABEL

[Наверх](#содержание)

Метаданные для образа.

    LABEL <key>=<value> [<key>=<value> ...]

*Примеры.*

    LABEL maintainer="user@example.org" version="1.1.1"


## ENV

[Наверх](#содержание)

Регистрация переменной окружения.

    ENV <key> <value>

*Примеры.*

    ENV LEVEL debug
    ENV NGINX_VERSION=1.23.1


## WORKDIR

[Наверх](#содержание)

Указания рабочей директории, в какой директории будет находиться контейнер,
от куда будет запуск команд.
Если директория не создана, тогда docker сам создаст эту директорию.

    WORKDIR <path>

*Примеры.*

    WORKDIR /home/user
    WORKDIR /app


## COPY

[Наверх](#содержание)

Копирование файлов из хоста в образ.
Копирование файлов будет происходить из хоста,
где находится Dockerfile и ниже, выше подниматься нельзя.

    COPY <src> [<src> ...] <dst>
    # <src> - файл или директория внутри build контекста
    # <dst> - файл или директория внутри контейнеры

*Примеры.*

    COPY . /home/user/
    COPY nginx_config/ /home/user/nginx_config

## ADD

[Наверх](#содержание)

Добавление файлов работают ровно так же, как и COPY,
но также можно добавлять вместо локальных файлов файлы из интернета.

    ADD <src> [<src> ...] <dst>
    # <src> - файл или директория внутри build контекста
    # <dst> - файл или директория внутри контейнеры

*Примеры.*

    # добавление по url
    ADD <url> /home/user/


## RUN

[Наверх](#содержание)

Вызов команд. Для уменьшения слоев можно писать несколько команд в одном запуске.

    RUN <command>

*Примеры.*

    RUN apt update && apt install -y nginx

    RUN apt install gcc -y && \
        gcc -Wall -o main main.c

## CMD

[Наверх](#содержание)

Указание какая команда будет выполняться при старте контейнера.

    CMD <command>

*Примеры.*

    CMD /start.sh

    CMD ["nginx", "-g", "daemon off;"]


## ENTRYPOINT

[Наверх](#содержание)

Аналог CMD - ENTRYPOINT.

    ENTRYPOINT <command>

*Примеры.*

    ENTRYPOINT /start.sh

    ENTRYPOINT ["nginx", "-g", "daemon off;"]


## VOLUME

[Наверх](#содержание)

Указание точки монтирования томов внутри образа.

    VOLUME <dst> [<dst> ...]
    # <dst> - директория монтирования для volume'a

*Примеры.*

    VOLUME /app /db /data

    VOLUME ["/var/log"]


## EXPOSE

[Наверх](#содержание)

Указание портов, которые слушает сервис в запущенном контейнере.

    EXPOSE <port>[/<proto>] [<port>[/<proto>] ...]
    # <port> - порт сервиса внутри контейнера
    # <proto> - tcp или udp

*Примеры.*

    EXPOSE 8080/tcp 3389/udp
    EXPOSE 8080
    EXPOSE 80 443


## STOPSIGNAL

[Наверх](#содержание)

Указывается сигнал, который посылается процессу при остановке контейнера.

    STOPSIGNAL <sig>

*Примеры.*

    STOPSIGNAL SIGKILL


## USER

[Наверх](#содержание)

Имя (ID) пользователя, от которого выполняются директивы RUN, CMD, ENTRYPOINT.

    USER <user>[:<group>]

*Примеры.*

    USER user
    USER user:user


## ARG

[Наверх](#содержание)

Почти как ENV, но задаёт параметры только для docker build.
Если в ENV мы задавали константные переменные, то здесь они будут задаваться динамически.

    ARG <variable>=<value>

*Примеры.*

    ARG FOO=bar


## HEALTHCHECK

[Наверх](#содержание)

Команда, которой можно проверить состояние сервиса.

    HEALTHCHECK <command>


## Уменьшение размера образа

[Наверх](#содержание)

1) Избегать установки лишних пакетов и упаковки лишних данных в образы.
2) Использовать связанные команды для RUN инструкций.

``` Docker
RUN command_1 && command_2 && command_3
```

Или можно привести к более читаемому виду

``` Docker
RUN command_1 && \
    command_2 && \
    command_3
```

3) Следить за последовательностью описания Dockerfile.
4) Уменьшать количество слоёв.
5) Продумывать порядок слоёв -
сначала выполняются слои которые не будут обновляться после каждой пересборки,
после, те которые могут подвергаться изменениям.
6) Один контейнер - одна задача.
7) Чистить за собой в контейнере.
8) Использовать multi-stage сборки (для компилируемых языков).
9) Использовать scratch образ для собственных приложений.

`scratch` - зарезервированный образ Docker, который является нулевым образом.


## Многоэтапная сборка

[Наверх](#содержание)

Многоэтапная сборка (multi-stage build) -
предназначена для оптимизации конечного контейнера.

> Что было до появления многоэтапной сборки?

Создавались 2 Dockerfile-а, один для разработки
(содержит все необходимое для создания вашего приложения),
другой оптимизированный для создания итогового образа,
Эта практика в свое время получила название «шаблон строителя».
Однако, поддерживать в актуальном состоянии два Dockerfile-а очень трудно.
От этой концепции отказались когда пришла многоэтапная сборка.

> Зачем нужна многоэтапная сборка?

Многоэтапная сборка нужна для оптимизации конечного контейнера.
Суть подхода заключается в том,
чтобы не заботиться о количестве получающихся слоев
в процессе сборки вашего приложения
и копировать результаты сборки из одного образа в другой.

К примеру, при компиляции приложения на c++,
требуются специальные библиотеки, компилятор и исходный код,
а в конце мы получаем лишь скомпилированный файл,
который весит пару МиБ.
И тогда можно просто перкопировать скомпилированный файл на другой контейнер,
и в итоге мы получим очень легковестный контейнер.

Для копирования файлов с одного контейнера
на другой используется команда `COPY` с опцией `--from`,
и через равно указывается номер контейнера от куда будет происходить копирование.

``` dockerfile
COPY --from=0 /go/main .
```

К опции `FROM` можно добавить псевдоним
для более удобного понимания от куда идёт копирование.

*Пример:*

``` dockerfile
FROM golang:latest as build
COPY . .
RUN go build ./src/main.go

FROM alpine:latest
COPY --from=build /go/main .
CMD ["./main"]
```


## Dockerignore

[Наверх](#содержание)

Файл `.dockerignore` преднзначен для исключения файлов и директорий.
Это помогает избежать ненужной отправки больший и конфиденциальных файлов и каталогов.

| Знак |                                   Описание |
|------|:-------------------------------------------|
|    # |                                Комментарий |
|    * |              Ноль, один или более символов |
|   ** |  Любое количество директорий, включая ноль |
|    ? |                 Подстановка одного символа |
|    ! |                   Исключение из исключений |

*Примеры:*

Комментарий.

    # comment

Игноривание всех файлов заканчивающихся на .log.

    *.log

Игноривание всех файлов заканчивающихся на .log во всех директориях.

    **/*.log

Исключение из исключений.

    # Игноривание всех файлов *.md
    *.md
    # Но включение файла README.md
    !README.md


# Docker Compose

[Наверх](#содержание)

Docker Compose используется для одновременного управления несколькими контейнерами,
входящими в состав приложения.
Этот инструмент предлагает те же возможности,
что и Docker, но позволяет работать с более сложными приложениями.

Описывается всё в файле `docker-compose.yml`.
Это файл, который будет содержать инструкции, необходимые для запуска и настройки сервисов.
Обычно он хранится в корневой директории проекта.


## Установка Docker Compose

[Наверх](#содержание)

Один из вариантов установки docker compose, это установка Docker Desktop.
Он будет содержать в себе и docker и docker-compose.
Ссылка на установку [Docker Desktop](https://www.docker.com/products/docker-desktop/).

Можно установить `docker-compose` используя пакетный менеждер дистрибутива.
Это отдельный файл, который является обёрткой над docker.

Так же существует специальный плагин для docker - `docker-compose-plugin`.
Он позволяет использовать команду не `docker-compose`, а `docker compose`,
команда будет являться не отдельным файлом, а расширением docker.
[Более подробная инструкция по установке](https://docs.docker.com/compose/install/linux/).


# Команды Docker Compose

[Наверх](#содержание)

Docker Compose имеет общие опции для работы со всеми командами.

Опции:
* `-f / --file <docker-compose-specify-file>` -
если требуется использовать другой файл.
docker compose обычно запускает только файл
`docker-compose.yml` или `docker-compose.override.yml`,
* `--env-file` - указать альтернативный env файл.
По умолчанию docker compose использует файл `.env`.
* `--project-name / -p` - указать имя проекта.
* `--parallel <int>` - контролировать максимальное количество параллелизма.
для неограниченного количества `-1`. По умолчанию `-1`.


## Сборка проекта Docker Compose

[Наверх](#содержание)

Сборка проекта.

    docker compose build

Опции:
* `--no-cache` - не использовать кэш при сборке образа.
* `--pull` - стараться всегда получить более новую версию.
* `--quiet / -q` - ничего не выводить на stdout.
* `--build-arg` - установить параметры сборки.


## Запуск проекта Docker Compose

[Наверх](#содержание)

Команда `up` запускает описанные контейнеры.

Опции:
* `--build` - перед запуском, происходит сборка контейнеров.
* `-d` - запуск контейнеров в фоновом режиме.
* `--no-build` - не создавать образ, даже если он отсутствует.
* `--attach` - прикрепиться сервисному выходу.
* `--attach-dependencies` - присойдениться к зависимым контейнерам.
* `--no-recreate` - если контейнеры существуют, не пересоздовать их заново.
* `--pull` - вытянуть образы перед запуском.
* `--abort-on-container-exit` - останавливает все контейнеры,
если какой-то контейнер был остановлен.
* `--force-recreate` - пересоздать контейнер, даже если его конфигурация не изменилась.
* `--remove-orphans` - удалить контейнеры, если они были не определены в compose файле.

*Примеры:*

Запуск проекта.

    docker compose up

Запустить проект и дополнительно собрать его.

    docker compose up --build

Запустить проект в фоновом режиме.

    docker compose up -d

Запустить проект, собрать его и в фоновом режиме.

    docker compose up --build


## Остановка проекта Docker Compose

[Наверх](#содержание)

Остановка проекта.

    docker compose stop

Остановить проект и удалить контейнеры.

    docker compose down


## Просмотр состояния Docker Compose

[Наверх](#содержание)

Посмотр логов сервиса.

    docker compose logs -f <service-name>

Вывод списка контейнеров.

    docker compose ps


## Отображение запущенных процессов Docker Compose

[Наверх](#содержание)

Команда показывает какие процессы запущены в каждом запущенном контейнере.

    docker compose top


## Просмотр образов Docker Compose

[Наверх](#содержание)

Команда для просмотра информации о образах.

    docker compose images


## Выполнение команды внутри контейнера Docker Compose

[Наверх](#содержание)

Выполнение команды в контейнере.

    docker compose exec <service-name> <command>


## Просмотр версии Docker Compose

[Наверх](#содержание)

`version` - предназначен для просмотра версии docker compose.

    docker compose version


## Прочие команды Docker Compose

[Наверх](#содержание)

Справка по всем командам.

    docker compose --help


# Docker Compose file

[Наверх](#содержание)

Полная документация по [docker compose file](https://docs.docker.com/compose/compose-file).

[Документация по 3 версии](https://docs.docker.com/compose/compose-file/compose-file-v3/).

## version

[Наверх](#содержание)

В начале файла указывается версия compose.
В различных версиях добавлены/удалены/изменены службы.
Все различные
[версии и изменения](https://docs.docker.com/compose/compose-file/compose-versioning/).

    version: "3.9"

При указании используемой версии файла compose,
при версиях 2 и 3, нужно указать как старший, так и дополнительный номера.
Если дополнительная версия не указана,
по умолчанию используется 0, а не последняя дополнительная версия.

``` Docker
version: "3"
# эквивалент вот этому
version: "3.0"
```


## services

[Наверх](#содержание)

`services` - в данном блоке описывается список сервисов.

``` Docker
services:
  name_1:
    ...
  name_2:
    ...
  name_3:
    ...
```


## build

[Наверх](#содержание)

`build` - указание пути, где лежит Dockerfile для определенного проекта
и передача определённых аргументов.

*Примеры:*

В текущей директории будет искаться файл `Dockerfile` и запускаться.

    build: .

Будет запущен `Dockerfile.nginx` в директории `nginx`.

    build: ./nginx/Dockerfile.nginx

Так же можно это разбить.

    build:
      context: ./nginx
      dockerfile: Dockerfile.nginx

Для передачи аргументов используется `args`.

    build:
      context: ./nginx
      args:
        ARG1: arg1
        ARG2: arg2

Либо такой вариант.

    build:
      context: ./nginx
      args:
        - ARG1=arg1
        - ARG2=arg2


## image

[Наверх](#содержание)

`image` - образ с которого будет запускаться контейнер.

    image: image:version

*Примеры:*

Запуск от определённого image.

    image: nginx:1.23.2


## ports

[Наверх](#содержание)

`ports` - Открытие контейнерных портов, для работы с хостом.

    ports:
      - <host-port>:<container-port>

*Примеры.*

Открытие порта.

    ports:
      - 80:80

Открытие нескольких портов.

    ports:
      - 8080:80
      - 6060:6060


## expose

[Наверх](#содержание)

`expose` - открывает порты, не окрывая из на хостовой машине.
Будут доступны только связным службам.

    expose:
      - <port>

*Пример.*

    expose:
      - "3000"
      - "8000"


## env_file

[Наверх](#содержание)

`env_file` - Добавление переменные среды из файла.
Может быть одним значением или списком.

    env_file: <env-file>

    env_file:
      - <env-file-1>
      - <env-file-2>
      - <env-file-3>


    VARIABLE=VALUE

*Примеры.*

Указания файлов.

    env_file:
      - .env
      - nginx/.env

Сам env файл.

    MY_VAR=value
    NGINX_VERSION=1.23.1


## environment

[Наверх](#содержание)

`environment` - Добавление переменных для среды.

*Примеры:*

Указания переменных.

    environment:
      SHOW: 'true'
      PASSWORD: "PASSWORD"

Указания переменных в другом формате yml.

    environment:
      - SHOW=true
      - PASSWORD="password"


## volumes

[Наверх](#содержание)

`volumes` - Указание точки монтирования томов внутри образа.

*Примеры:*

Монтирование по абсолютному пути.

    volumes:
      - /opt/data:/var/lib/mysql

Монтирование относительно compose файла.

      - ./cache:/tmp/cache

Так же можно указывать именнованный том.

Сначала требуется создать.
В конце после всех services создаются все нужные volumes.

    volumes:
      name_volume:

И дальше в нужном сервисе вставляется нужный volume.

    volumes:
      - name_volume:/var/lib/mysql

Все созданые volumes хранятся по пути `/var/lib/docker/volumes`.

У volume есть параметры, которые монжо конфигурировать его.

`name` - указание имени volume, с которым он будет создоваться.

`external` - если стоит true, значит он не будет создавать новый volume,
а будет искать готовый.
Если не указывается параметр `name`, тогда требуется указать его в external.

*Примеры:*

Указание имени volume.

    volumes:
      mysql_database:
        name: mysql_database

Использовать готовый volume.

    volumes:
      mysql_database:
        external:
          name: mysql_database

Использование external вместе с name.

    volumes:
      mysql_database:
        external: true
        name: mysql_database


## container_name

[Наверх](#содержание)

`container_name` - Собственное имя контейнера, вместо генерации по умолчанию.

    container_name: <container-name>

*Пример.*

    сontainer_name: nginx_container


## entrypoint

[Наверх](#содержание)

`entrypoint` - Переопределение начального запуска.

    entrypoint: <command>

    # Так же можно задать списком
    entrypoint: ["<command>", "<arg1>", "<arg2>"]

*Примеры:*

    entrypoint: "nginx -g daemon off;"
    entrypoint: ["nginx", "-g", "daemon off;"]

## command

[Наверх](#содержание)

`command` - Переопределение команды для запуска.

    command: "<command>"

    # Так же можно задать списком
    command: ["<command>", "<arg1>", "<arg2>"]

*Примеры:*

    command: "nginx -g daemon off;"
    command: ["nginx", "-g", "daemon off;"]


## depends_on

[Наверх](#содержание)

`depends_on` - зависимости запуска и завершения работы между службами.

*Примеры:*

Служба name_1 зависит от name_2 и name_3,
т.е. будет запускаться после них.

    version "3.9"

    services:
      name_1:
        depends_on:
          - name_2
          - name_3
      name_2:
        ...
      name_3:
        ...


## restart

[Наверх](#содержание)

`restart` - Политика перезапуска контейнера.

Существуют 4 варианта:
* `no` - не перезапускает контейнер (по умолчанию).
* `always` - контейнер будет всегда перезапускается.
* `on-failure` - при ошибке будет перезапускаться контейнер.
* `unless-stopped` - всегда перезапускается контейнер,
если контейнер не остановлен (вручную или другим образом).

*Примеры:*

    restart: on-failure


## healthcheck

[Наверх](#содержание)

`healthcheck` - проверка, которая запускается, чтобы определить,
являются ли контейнеры для этой службы *работоспособными*.

Параметры для healthcheck:
* `interval` - через какой период будет запускаться проверка.
* `timeout` - если проверка длится больше указанного времени,
значит контейнер не работоспособен.
* `retries` - сколько раз запускается проверка,
чтобы понять что контейнер не работает.
* `start_period` - время для запуска контейнера,
перед запуском проверок работоспособности.

interval, timeout и start_period имеют специфичный вариант написания.
Поддерживаемые единицы измерения: us, ms, s, m и h.

Примеры написания:

    2.5s
    10s
    1m30s
    2h32m
    5h34m56s

*Пример:*

    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 1m30s
      timeout: 10s
      retries: 3
      start_period: 40s


## extends

[Наверх](#содержание)

`extends` - позволяет использовать общие конфигурации
для разных файлов или даже для разных проектов.
Расширение служб полезно, если есть несколько служб,
повторно использующих общий набор параметров конфигурации.

Шаблон.

    extends:
      file: /path/to/file
      service: <service-name>

*Примеры:*

Использование сервиса из другого файла.

first-service.yml

    services:
      webapp:
        build: .
        ports:
          - "8000:8000"

second-service.yml

    services:
      web:
        extends:
          file: first-service.yml
          service: webapp


# Полезные инструменты

[Наверх](#содержание)

* `ctop` - удобный просмотр контейнеров, какие запущены,
какие простаивают, сколько памяти тратят и прочее.

Документацию и установку можно найти на [github](https://github.com/bcicen/ctop).

* `hadolint` - инструмент который проверяет корректность написания Dockerfile.

[Ссылка на онлайн использование](https://hadolint.github.io/hadolint/).

[Ссылка на репозиторий и документацию](https://github.com/hadolint/hadolint).