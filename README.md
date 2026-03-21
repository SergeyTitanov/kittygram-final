#  Как работать с репозиторием финального задания

## Структура репозитория (корень `kittygram-final`)

```
kittygram-final
├── .env.example
├── README.md
├── .github
│   └── workflows
│       └── main.yml
├── backend
│   ├── README.md
│   ├── cats
│   ├── kittygram_backend
│   ├── manage.py
│   └── requirements.txt
├── docker-compose.production.yml
├── frontend
│   ├── Dockerfile
│   ├── README.md
│   ├── package-lock.json
│   ├── package.json
│   ├── public
│   └── src
├── kittygram_workflow.yml
├── nginx
│   ├── Dockerfile
│   └── nginx.conf
├── pytest.ini
├── tests.yml
└── tests
```

Дополнительно в этом проекте: `docker-compose.yml` (локальная разработка), `.gitignore`, `.flake8`.

## Django-админка

**Встроенного логина/пароля в репозитории нет** — пользователя с правами администратора нужно создать на сервере (или локально в Docker):

```bash
docker compose -f docker-compose.production.yml exec backend python manage.py createsuperuser
```

Дальше вход: `http://<ваш_IP>:<порт>/admin/` с указанными при создании логином и паролем.

Если статика админки (CSS) не подгружалась, проверьте, что после деплоя выполнялись `collectstatic` и перезапуск gateway (см. workflow `deploy`).

## Что нужно сделать

Настроить запуск проекта Kittygram в контейнерах и CI/CD с помощью GitHub Actions

## Как проверить работу с помощью автотестов

В корне репозитория создайте файл tests.yml со следующим содержимым:
```yaml
repo_owner: ваш_логин_на_гитхабе
kittygram_domain: полная ссылка (http://<ip-адрес вашей ВМ>:<порт gateway>) на ваш проект Kittygram
dockerhub_username: ваш_логин_на_докерхабе
```

Скопируйте содержимое файла `.github/workflows/main.yml` в файл `kittygram_workflow.yml` в корневой директории проекта.

Для локального запуска тестов создайте виртуальное окружение, установите в него зависимости из backend/requirements.txt и запустите в корневой директории проекта `pytest`.

## Чек-лист для проверки перед отправкой задания

- Проект Kittygram доступен по ссылке, указанной в `tests.yml`.
- Пуш в ветку main запускает тестирование и деплой Kittygram, а после успешного деплоя вам приходит сообщение в телеграм.
- В корне проекта есть файл `kittygram_workflow.yml`.
