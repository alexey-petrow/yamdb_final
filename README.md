![workflow](https://github.com/p1vosos/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)
# YaMDb

[Ссылка на развернутый на сервере проект](http://51.250.6.88/api/v1/)

## Описание проекта
**YaMDB** – Проект YaMDb собирает отзывы, рейтинг и коментарии пользователей на произведения,  
буть то фильмы, книги, музыка или что-то другое.

## Как запустить проект в docker (dev-режим):

- Клонировать репозиторий и перейти в директорию с docker-compose.yaml:

```
https://github.com/p1vosos/yamdb_final
cd infra_sp2/infra
```

- Развернуть 3 контейнера, nginx, database и web(сам проект + gunicorn):

```
docker-compose up -d
```

- Выполнить миграции и собрать статику:

```
docker-compose exec web python3 manage.py migrate
docker-compose exec web python3 manage.py collectstatic --no-input
```

- Заполнить базу тестовыми данными:

```
docker-compose exec web python3 manage.py loaddata fixtures.json
```

## Проект запущен и доступен по адресу:
- http://localhost/redoc/ - подробная документация
- http://localhost/api/v1/ - корневой url для api запросов
- http://localhost/admin - админ зона

## Шаблон заполнения файла .env

DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql  
DB_NAME=postgres # имя базы данных  
POSTGRES_USER=postgres # логин для подключения к базе данных  
POSTGRES_PASSWORD=postgres # пароль для подключения к БД (установите свой)  
DB_HOST=db # название сервиса (контейнера)  
DB_PORT=5432 # порт для подключения к БД

SECRET_KEY='a&l%11111aaaaaa^##a1)aaa@4aaa=aa&aaaal^##aaa1(aa'

## Примеры запросов к API

### Запрос на добавление нового пользователя
type: `POST`  
url: `/api/v1/auth/signup/`   
request body schema:
- **email**: *string* required Емейл пользователя
- **username**: *string* required Имя пользователя
response:
- **email**: *string* Емейл пользователя
- **username**: *string* Имя пользователя

### Получить токены JWT для пользователя
type: `POST`  
url: `/api/v1/jwt/create/`   
request body schema:
- **username**: *string* required Имя пользователя
- **confirmation_code**: *string* required Код подтвержения, который пришел на email
response:
- **token**: *string* JWT токен

### Список произведений
type: `GET`  
url: `/api/v1/titles/`   
query params: 
- **category**: *string* Фильтрует по полю slug категории
- **genre**: *string* Фильтрует по полю slug жанра
- **name**: *string* Фильтрует по названию произведения
- **year**: *string* Фильтрует по году
response:  
array [
    - **count**: *int* количество публикаций
    - **next**: *string* 
    - **previous**: *string* 
    - **results**: *array*
      - **id**: *int* id произведения
      - **name**: *string* название произведения
      - **year**: *int* год создания произведения
      - **rating**: *int* рейтинг произведения
      - **description**: *string* описание произведения
      - **genre**: *array*
        - **name**: *string* название жанра
        - **slug**: *int* slug жанра
      - **category**: *array*
        - **name**: *string* название жанра
        - **slug**: *int* slug жанра    
]
## Использованные технологии
- [Python](https://www.python.org/)
- [PostgreSQL](https://postgrespro.ru/docs)
- [Django](https://www.djangoproject.com/)
- [Django Rest Framework](https://www.django-rest-framework.org/)
- [JWT](https://jwt.io/)

## Авторы
Ходырев Денис <denis_servmail@mail.ru>  
Петров Алексей <alexeypetrow21@gmail.com>  
Савенко Юрий <rkutdn@ya.ru>