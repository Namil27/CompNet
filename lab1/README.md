# ЛР1. HA Postgres Cluster (Patroni + Zookeeper + HAProxy)

## Цель
Развернуть кластер PostgreSQL из двух нод с Patroni + Zookeeper и настроить единый вход через HAProxy.

## Подготовительный этап
- Мы подготовили файлы:
  - `Dockerfile`
  - `docker-compose.yml`
  - `postgres0.yml`
  - `postgres1.yml`
  - `haproxy.cfg`
- И запустили кластер при помощи команды:
```bash
docker compose up -d --build

### Список запущенных контейнеров после запуска docker compose.
![docker ps](assets/01-docker-ps.png)

### Проверим роли нод Leader и Replica
![patronictl list](assets/02-patroni-list.png)
По выводу Leader - pg-slave, Replica - pg-master

### На лидере создана таблица и добавлена запись:
![insert leader](assets/03-insert-leader.png)