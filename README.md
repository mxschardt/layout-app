# Структура проекта

1. Клиентское приложение на React.js

    Для сборки и деплоя используется Vite.

    Компоненты:

    - Layout описывает макет веб-страницы (шапка, подвал, левая и правая панель).
    - Workspace осуществляет логику рабочего пространства. При появлении свободного
      места (расширения экрана) запрашивает столько карточек, сколько поместится на экран.
      При уменьшении свободного места карточки скрываются, но не удаляются.
    - Card представляет плашку.

    Хуки:

    - useDimensions позволяет следить за размерами компонента. Возвращает ссылку,
      которую нужно установить на компонент, и текущие размеры этого компонента.
      Размеры обновляются при изменении размера окна. Для избежания многочисленных вычислений
      используется useCallback.

2. Серверное приложение на .NET Core (API)

    Реализует Model-View-Controller с использованием DTO. 
    Для связи модели и DTO используется AutoMapper.

    Model:

    - User описывает некоторого пользователя, который владеет постами.
    - Post представляет собой пост пользователя. Содержит заголовок, запись (тело поста) и
      дату создания в UTC

    DTO:

    - Create/Read/UpdateUserDto - объекты для передачи данных пользователя.
    - Create/Read/UpdatePostDto - объекты для передачи данных поста.

    Controller:

    - UserController базовый CRUD пользователя.
    - PostController базовый CRUD поста.

    Оба контроллера позволяют выбрать начальную точку и лимит получаемых записей.
    С помощью PostController возможно получить все посты выбранного пользователя. 

    Database:

    - ApplicationContext представляет контекст базы данных. Включает в себя таблицы
      пользователя и постов.
    - Post/UserConfiguration осуществляют настройку сущностей для базы данных.
    - SeedData отвечает за миграции базы данных, а также её заполнение
      тестовыми данными в режиме разработки.
    
    В режиме разработки используется InMemoryDB, в режиме Production - PostgreSQL.

3. Docker-Compose и NGINX

    Все решение собирается одним docker-compose файлом; в директориях клиента и сервера
    содержатся Dockerfile для корректной сборки. Для создания базы данных
    используется образ postgres:15.3-alpine, а для NGINX - nginx:mainline.

    NGINX работает на порту 8080, и прокидывает следующие пути:

    - /     контейнер с клиентом
    - /api  контейнер с API сервером

# Быстрый старт

1. Склонируйте репозиторий

```
git clone --recursive https://github.com/mxschardt/layout-app && cd layout-app
```

2. Запустите docker-compose

```
docker-compose up
```

Будет создано клиентское приложение, серверное API, база данных и NGINX.
В режиме разработке база данных будет заполнена тестовыми данными при инициализации.
Приложение будет доступно по адресу http://localhost:8080
