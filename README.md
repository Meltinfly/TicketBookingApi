# 📌 TicketBookingApi

[![.NET](https://img.shields.io/badge/.NET_9.0-purple?style=for-the-badge&logo=.net)](https://dotnet.microsoft.com/en-us/download/dotnet/9.0)
[![MS_SQL Server](https://img.shields.io/badge/MS_SQL_Server-2022-orange?style=for-the-badge&)](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
[![Docker](https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/get-started)
[![Docker Compose](https://img.shields.io/badge/Docker%20Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docs.docker.com/compose/install/)

RESTful API для системы бронирования билетов на основе ASP.NET Core 9. Предоставляет функционал для управления поездками, бронирования билетов и пользовательскими аккаунтами с поддержкой современных методов аутентификации.

---

## 🌟 Основные возможности
- Аутентификация и авторизация 
    - Регистрация и вход пользователей
    - Обновление токенов (JWT Access/Refresh Tokens)
    - OAuth 2.0 (авторизация через Google, Yandex)

- Управление билетами 
    - Покупка билета
    - Отмена бронирования
    - Просмотр своих билетов
    - Просмотр билетов пользователя (только для администраторов)

- Управление поездками 
    - Создание поездки (администратор)
    - Удаление поездки (администратор)
    - Обновление поездки (администратор)
    - Просмотр поездки по ID
    - Просмотр всех поездок с фильтрацией

- Управление пользователями 
    - Удаление пользователя (администратор)
    - Поиск пользователя по имени (администратор)
    - Просмотр всех пользователей (администратор)
    - Обновление своего профиля

## 🛠 Технологии  
| Категория                 | Технология                                                                          |
|---------------------------|-------------------------------------------------------------------------------------|
| **Backend**               | ASP.NET Core 9                                                                      |
| **База данных**           | MS SQL Server                                                                       |
| **ORM**                   | EF Core (Code First)                                                                |
| **Безопасность**          | ASP.NET Identity, JWT (Access Tokens + Refresh Tokens), OAuth 2.0 (Google, Yandex)  |
| **Паттерны и библиотеки** | CQRS (MediatR), AutoMapper, FluentValidation                                        |
| **API Docs**              | OpenAPI / Swagger, Scalar                                                           |

## ▶️ Запуск проекта
### 🖥️ Базовый запуск:
Предварительные требования:
- .NET 9 SDK
- MS SQL Server (локальный или облачный)

#### 1. Клонировать репозиторий
``` bash
git clone https://github.com/Meltinfly/TicketBookingApi.git
cd TicketBookingApi
```

#### 2. Настройте подключение к БД:
- Откройте файл `appsettings.json`
- Измените строку подключения:
``` json
"ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=TicketBooking;Trusted_Connection=True;TrustServerCertificate=True;"
  }
```

#### 3. Примените миграции:
``` bash
dotnet ef database update
```

#### 4. Настройте OAuth (опционально):
- Для Google OAuth:
``` bash
dotnet user-secrets set "Authentication:Google:ClientId" "<CLIENT_ID>"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<CLIENT_SECRET>"
```
- Для Yandex OAuth:
``` bash
dotnet user-secrets set "Authentication:Yandex:ClientId" "<CLIENT_ID>"
dotnet user-secrets set "Authentication:Yandex:ClientSecret" "<CLIENT_SECRET>"
```

#### 5. Запустить приложение
``` bash
dotnet run
```
---
### 🐋 Развертывание через Docker
Для удобного запуска приложения в изолированной среде используется Docker. Проект включает в себя `Dockerfile` и `docker-compose.yml` для оркестрации сервисов.

Убедитесь, что на вашем компьютере установлены:
- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

#### 1. Клонировать репозиторий
``` bash
git clone https://github.com/Meltinfly/TicketBookingApi.git
cd TicketBookingApi
```

#### 2. Настройте подключение к БД:
- Откройте файл `Program.cs`
- Измените строку подключения c `DefaultConnection` на `DockerConnection`:
``` c#
const string CONNECTION_STRING = "DockerConnection";
```

#### 3. Настройте OAuth (опционально):
- Создате .env файл в корне проекта (рядом с `docker-compose.yml`)
- Добавьте следующие значения:
```
GOOGLE_CLIENT_ID=<CLIENT_ID>
GOOGLE_CLIENT_SECRET=<CLIENT_SECRET>
YANDEX_CLIENT_ID=<CLIENT_ID>
YANDEX_CLIENT_SECRET=<CLIENT_SECRET>
```

#### 4. Запустите приложение с помощью Docker Compose:
``` bash
docker compose up --build
```
---
По умолчанию API будет доступно по адресу:
👉 http://localhost:5287 или https://localhost:7298

### 👤 Тестовые пользователи 
При первом запуске в системе создаются два тестовых пользователя: 
- Администратор: 
    - Логин: log1
    - Пароль: admin123
            
- Обычный пользователь:
    - Логин: log2
    - Пароль: user123

### 📖 Документация API:
- Swagger UI: http://localhost:5287/swagger
- Scalar UI: http://localhost:5287/scalar

## 🔐 Аутентификация
### Получение токенов
#### 1. Регистрация:
``` http
POST /api/auth/register
Content-Type: application/json

{
    "username": "log3",
    "password": "user123",
    "name": "Александр",
    "lastName": "Васильев",
    "patronymic": "Олегович", // необязательно
    "email": "brawn@gmail.com", // необязательно
    "phoneNumber": "+78800353535" // необязательно
}
```

#### 2. Вход:
``` http
POST /api/auth/login
Content-Type: application/json

{
    "username": "log3",
    "password": "user123"
}
```

#### 3. Вход через сторонние провайдеры:
- Вход через Google:
`http://localhost:5287/api/auth/signin/Google`
- Вход через Yandex:
`http://localhost:5287/api/auth/signin/Yandex`

#### 4. Обновление токена:
``` http
POST /api/auth/refresh-token
Content-Type: application/json

{
  "token": "<refresh_token>"
}
```

## 📡 Пример использования API
### Покупка билета (требуется авторизация):
``` http
POST /api/tickets
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "tripId": 2,
  "seatNumber": 1
}
```

## 📬 Связь
Если у вас есть вопросы или предложения, напишите мне:
- Email: mihaylov.slava@outlook.com
- Telegram: @Fl1cker_0
