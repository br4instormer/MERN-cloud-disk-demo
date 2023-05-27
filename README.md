# Инструкция по развёртыванию учебного приложения

## Подготовка

### Установка окружения

* Установить [Node.js](https://nodejs.org/en/download). Тестировалось на v18.16

### Скачивание проекта

* Скачать с [github-страницы](https://github.com/br4instormer/MERN-cloud-disk-demo) проект в zip-архиве
* Распаковать архив

### Изменение путей каталогов хранения файлов

* Изменить пути в файле `server/config/default.json` на собственные

  **Не забывайте про слэши. В Windows необходимо использовать два слэша подряд в качестве разделителя**

  ```json
  {
    "filePath": "C:\\Users\\user\\Desktop\\mern-cloud-disk-demo\\server\\files",
    "staticPath": "C:\\Users\\user\\Desktop\\mern-cloud-disk-demo\\server\\static"
  }
  ```


### Создание каталогов хранения файлов

В сервисе `server` создать каталоги `files` и `static`. Либо иные, которые указаны в параметрах конфига `filePath` и `staticPath` соответственно.

### Установка Docker

* [Скачать](https://www.docker.com/products/docker-desktop/) и установить Docker Desktop

## Развёртывание

### Развёртывание базы данных

План:

1. [Создать volume для хранения базы данных MongoDB](#deploy-db-image-1)
2. [Запустить инстанс MongoDB в Docker-контейнере](#deploy-db-image-2)
3. [Удостовериться, что инстанс запущен](#deploy-db-image-3)

---

1. <a id="deploy-db-image-1"></a>В консоли выполнить команду `docker volume create dbdata`
2. <a id="deploy-db-image-2"></a>Выполнить команду `docker run --rm -d --name mongodb -e MONGO_INITDB_ROOT_USERNAME=user -e MONGO_INITDB_ROOT_PASSWORD=dbpassword -v dbdata:/data/db -p 27017:27017 mongo`

    Параметры окружения используемые для инициализации базы данных:

    * `MONGO_INITDB_ROOT_USERNAME` имя пользователя для доступа к базе
    * `MONGO_INITDB_ROOT_PASSWORD` пароль для доступа к базе

3. <a id="deploy-db-image-3"></a>Выполнить команду `docker container ls`

    Контейнер `mongodb` должен присутствовать с списке

    ```bash
    CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                           NAMES
    7147cd6e7dc7   mongo     "docker-entrypoint.s…"   41 minutes ago   Up 41 minutes   0.0.0.0:27017->27017/tcp, :::27017->27017/tcp   mongodb
    ```

### Установка зависимостей

1. Зайти в каталог `server`, в консоли выполнить команду `npm i`
2. Зайти в каталог `client`, в консоли выполнить команду `npm i`

## Эксплуатация

### Запуск сервисов

1. Зайти в каталог `server`, в консоли выполнить команду `npm run start`
2. Зайти в каталог `client`, в консоли выполнить команду `npm run start`

### Остановка сервисов

* Нажмите Ctrl+C в консоли

### Оснановка контейнера базы данных

* Выполните в консоли команду `docker container stop mongodb`
