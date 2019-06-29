# Docker - 1

*Выполнено*

* `docker run`
* `docker inspect`
* `docker exec`
* `docker kill`

# Docker 3

## Цели
* [ ] Научиться описывать и собирать Docker-образы для сервисного приложения
* [ ] Научиться оптимизировать работу с Docker-образами
* [ ] Запуск и работа приложения на основе Docker-образов, оценка удобства запуска контейнеров при помощи docker run


docker network create reddit
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db mongo:latest
docker run -d --network=reddit --network-alias=post akozhin/post:1.0
docker run -d --network=reddit --network-alias=comment akozhin/comment:1.0
docker run -d --network=reddit -p 9292:9292 akozhin/ui:1.0

# Docker 4

## Сеть в docker

Рассмотрены сетевые драйверы bridge host none

Созданы сети для приложений фрона и бекэнда (ui только во фронте, сервисы во фронте и бэкенде, БД только в бэкенде):

```bash
docker network create back_net  --subnet=10.0.2.0/24
docker network create front_net --subnet=10.0.1.0/24
```
Созданы контейнеры в разных сетях
```bash
docker run -d --network=back_net --network-alias=post_db --network-alias=comment_db  --name mongo_db mongo:latest
docker run -d --network=back_net --network-alias=post --name post mnsoldotus/post:1.0
docker run -d --network=back_net --network-alias=comment --name comment mnsoldotus/comment:1.0
docker run -d --network=front_net -p 9292:9292 --name ui mnsoldotus/ui:1.0
```
Выполнено онлайн подключение контейнеров к сети
```bash
docker network connect front_net post 
docker network connect front_net comment 
```

Рассмотрены сетевые интерфейсы со стороны хостовой машины
```bash
docker network ls
brctl show <interface>
```
Рассмотрены правила iptables

## Docker-compose

Создан docker-compose.yml для проекта из 3 приложений

Выполнен запуск через `docker-compose up`

Просмотр списка контейнеров `docker-compose ps`

Выполнена параметризация docker-compose.yml черех переменные окружения и .env файл

Наименованием проекта по умолчанию является basename директории в которой находится docker-compose.yml
Задать имя можно:

* Переменной COMPOSE_PROJECT_NAME в файле .env. Пример COMPOSE_PROJECT_NAME=docker4
* Опцией docker-compose -p, --project-name NAME. Пример docker-compose -p docker4 up -d

https://docs.docker.com/compose/reference/envvars/