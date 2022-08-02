# Yamdb_final

![yamdb_workflow](https://github.com/Dimanitto/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

## Технологии:
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![GitHub%20Actions](https://img.shields.io/badge/-GitHub%20Actions-464646??style=flat-square&logo=GitHub%20actions) ![GitHub](https://img.shields.io/badge/-GitHub-464646??style=flat-square&logo=GitHub) ![docker](https://img.shields.io/badge/-Docker-464646??style=flat-square&logo=docker) ![NGINX](https://img.shields.io/badge/-NGINX-464646??style=flat-square&logo=NGINX) ![Python](https://img.shields.io/badge/-Python-464646??style=flat-square&logo=Python) ![Django](https://img.shields.io/badge/-Django-464646??style=flat-square&logo=Django) ![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-464646??style=flat-square&logo=PostgreSQL)

## О проекте:
Проект Yamdb_final создан для демонстрации методики DevOps (Development Operations) и идеи Continuous Integration (CI) и Continuous Deployment (CD),
суть которых заключается в интеграции и автоматизации следующих процессов:
* синхронизация изменений в коде
* автоматический запуск тестов
* сборка, запуск приложения в среде, аналогичной среде боевого сервера
* деплой на сервер после успешного прохождения всех тестов
* уведомление об успешном прохождении всех этапов

### Подготовка удаленного сервера для развертывания приложения
Установка docker:
```
 sudo apt install -y docker-ce
```
Установка docker-compose:
```
apt install docker-compose
```
Создайте папку проекта на удаленном сервере и скопируйте туда файлы docker-compose.yaml, default.conf (настройки nginx):
```
scp ./<FILENAME> <USER>@<HOST>:/home/<USER>/yamdb_final/
```

### Подготовка репозитория на GitHub

Для использования Continuous Integration и Continuous Deployment необходимо в репозитории на GitHub прописать Secrets - переменные доступа к вашим сервисам.
Переменые прописаны в workflows/yamdb_workflow.yaml

* DOCKER_PASSWORD, DOCKER_USERNAME - для загрузки и скачивания образа с DockerHub 
* USER, HOST, PASSPHRASE, SSH_KEY - для подключения к удаленному серверу 
* TELEGRAM_TO, TELEGRAM_TOKEN - для отправки сообщений в Telegram

### Развертывание приложения

1. При пуше в ветку main приложение пройдет тесты, обновит образ на DockerHub и сделает деплой на сервер. Дальше необходимо подлкючиться к серверу.
```
ssh <USER>@<HOST>
```
2. Перейдите в запущенный контейнер приложения командой:
```
docker container exec -it <CONTAINER ID> bash
```
3. Внутри контейнера необходимо выполнить миграции и собрать статику приложения:
```
python manage.py collectstatic --no-input
python manage.py migrate
```

4. Также можно наполнить базу данных начальными тестовыми данными:
```
python manage.py loaddata fixtures.json
```

## Развернутый проект
http://51.250.106.88/admin/

## Для обращения к API проекта:
* http://51.250.106.88/api/v1/auth/token/
* http://51.250.106.88/api/v1/users/
* http://51.250.106.88/api/v1/categories/
* http://51.250.106.88/api/v1/genres/
* http://51.250.106.88/api/v1/titles/
* http://51.250.106.88/api/v1/titles/{title_id}/reviews/
* http://51.250.106.88/api/v1/titles/{title_id}/reviews/{review_id}/
* http://51.250.106.88/api/v1/titles/{title_id}/reviews/{review_id}/comments/

### Автор
Selivanov Dmitry
