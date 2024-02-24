## 1. Clone this project

## 2. Change settings in docker-compose.yml

postgres:
- `POSTGRES_USER: outline` - set required postgres user
- `POSTGRES_PASSWORD: pass` - set required postgres password
- `POSTGRES_DB: outline` - set required postgres database
- `test: ["CMD", "pg_isready -U outline"]` - set required postgres user instead of outline

https-portal:
- `DOMAINS: 'example.com -> http://outline:3000'` - set your domain instead of example.com

outline:
- `DATABASE_URL=postgres://outline:pass@postgres:5432/outline` - set user:password for database
- `DATABASE_URL_TEST=postgres://outline:pass@postgres:5432/outline-test` - set user:password for test database

## 3. Change settings in docker.env

 - Set `SECRET_KEY` and `UTILS_SECRET`
 - Set at least one auth type according [to the manual](https://app.getoutline.com/share/770a97da-13e5-401e-9f8a-37949c19f97e/doc/authentication-7ViKRmRY5o)
   - If you're using Google OAuth, please pay attention to the `Authorized redirect URIs` step, you must set correct correct http/https method (if you're using https-portal with certificate, it must be https)

## 4. Create database

`docker-compose run --rm outline yarn sequelize db:create --env=production-ssl-disabled`

Step may show you error, do not pay attention.

## 5. Migrate the new database to add needed tables, indexes, etc:

`docker-compose run --rm outline yarn sequelize db:migrate --env=production-ssl-disabled`

## 6. Run service

`docker-compose up -d`
