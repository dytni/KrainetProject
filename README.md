# Запуск приложения

---
# Docker Вариант
## 1. Подготовка
Убедитесь, что установлены:
- Docker
- Docker Compose
- Java 21
- Maven
и порты 8000 8080 8081 9092 свободны

Структура:

KrainetProject/
- ├── AUTH/
- ├── Notification/
- ├── docker-compose.yml
- └── .env



## 2. Переменные окружения
Создайте `.env` с вашими данными или возьмите их из моего письма:
```env
SPRING_MAIL_USERNAME=ваша-почта@gmail.com
SPRING_MAIL_PASSWORD=ваш-app-пароль
```
## 3. Запуск
Выполните команды из терминала находясь в корневой папке ```KrainetProject```
```bash
    docker-compose down --volumes
    docker-compose build
    docker-compose up --build
```
---

# Локальный Вариант

## 1. Подготовка
Убедитесь, что установлены:
- Kafka 4.0.0
- Java 21
- Maven
- PostgreSQL

Структура:

AUTH service можно скачать из этого репозитория
https://github.com/dytni/Krainet
Notification service можно скачать из этого репозитория
https://github.com/dytni/notifications


## 2. Переменные окружения
Создайте `.env` с вашими данными или возьмите их из моего письма:
```env
SPRING_MAIL_USERNAME=ваша-почта@gmail.com
SPRING_MAIL_PASSWORD=ваш-app-пароль
```


Поднимите Kafka сервер и при если нужно измените
```application.properties``` файлы

Создайте базу данных PostgreSQL и измените оба ```application.properties``` файла для подключения к вашей базе данных укажите ваш URL, password и username

## 3 Запуск
Запустите сначала AUTH сервис и дождитесь пока пройдут все не обходимые миграции
затем запустите Notification сервис

---
# Данные для входа

## Admin
  В систему уже загружен Admin 
  ```json 
  {
  "username": "admin_user",
  "password": "password"
 }
``` 
 После входа сервер вам вышлет JWT для аутентифицированного пользователя

## User

Также в систему уже загружен User
  ```json 
  {
  "username": "john_doe",
  "password": "password"
 }
```
После входа сервер вам вышлет JWT для аутентифицированного пользователя

---

# Адреса


## Общие (доступны после аутентификации)я

### Регистрация нового пользователя
- **Метод:** `POST`
- **Путь:** `http://localhost:8080/auth/signup`
- **Тело запроса:**
  ```json
  {
    "username": "string",
    "email": "string",
    "password": "string",
    "firstName": "string",
    "lastName": "string"
  }


### Вход в систему
- **Метод:** `POST`
- **Путь:** `http://localhost:8080/auth/signin`
- **Тело запроса:**
  ```json
  {
    "username": "string",
    "password": "string"
  }
  ```



### Получить текущего пользователя
- **Метод:** `GET`
- **Путь:** `http://localhost:8080/user/me`
- **Требуется JWT токен**
- **Ответ:**
  ```json
  {
    "id": 1,
    "username": "string",
    "email": "string",
    "firstName": "string",
    "lastName": "string",
    "roles": ["USER"]
  }
  ```

### Редактировать свой профиль
- **Метод:** `PUT`
- **Путь:** `http://localhost:8080/user/editMe`
- **Тело запроса:**
  ```json
  {
    "username": "string",
    "email": "string",
    "password": "string",
    "firstName": "string",
    "lastName": "string"
  }
  ```


### Удалить свой аккаунт
- **Метод:** `DELETE`
- **Путь:** `http://localhost:8080/user/deleteMe`
- **Требуется JWT токен**

---

## Только для администратора (роль `ADMIN`)

### Добавить пользователя
- **Метод:** `POST`
- **Путь:** `http://localhost:8080/users/add`
- **Тело запроса:**
  ```json
  {
    "username": "string",
    "email": "string",
    "password": "string",
    "firstName": "string",
    "lastName": "string",
    "roles": ["USER"]
  }
  ```

### Редактировать пользователя
- **Метод:** `PUT`
- **Путь:** `http://localhost:8080/users/edit/{id}`
- **Тело запроса:** (тот же формат, что и для `/users/add`)


### Удалить пользователя
- **Метод:** `DELETE`
- **Путь:** `http://localhost:8080/users/delete/{id}`


### Просмотреть всех пользователей
- **Метод:** `GET`
- **Путь:** `http://localhost:8080/users/show`


---