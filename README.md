## MS SQL in DOCKER
- Проект с наработками по переносу в контейнер СУБД MSSQL

## Запуск

* ``ACCEPT_EULA`` - принимает лицензионное соглашение конечного пользователя.
* ``MSSQL_PID`` - указывает бесплатно лицензированный выпуск Developer Edition SQL Server для непроизводственных приложений.
* `MSSQL_SA_PASSWORD` - задает надежный пароль (не мение 8 символов).
* `MSSQL_TCP_PORT` - задает TCP-порт, прослушивающий SQL Server до 1234. Это означает, что вместо сопоставления порта 1433 (по умолчанию) с портом узла в этом примере необходимо выполнить сопоставление пользовательского порта TCP с помощью команды -p 1234:1234.

```bash
# Если вы работаете с Docker в Linux, используйте следующий синтаксис с одинарными кавычками
docker run -e ACCEPT_EULA=Y -e MSSQL_PID='Developer' -e MSSQL_SA_PASSWORD='P@ssw0rd47154' -e MSSQL_TCP_PORT=1433 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-CU29-GDR1-ubuntu-16.04
```

```bash
# Если вы работаете с Docker в Windows, используйте следующий синтаксис с двойными кавычками
docker run -e ACCEPT_EULA=Y -e MSSQL_PID="Developer" -e MSSQL_SA_PASSWORD="P@ssw0rd47154" -e MSSQL_TCP_PORT=1433 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-CU29-GDR1-ubuntu-16.04
```

```bash
docker-compose up -d
```

## Дополнительная информация

- При экспорте базы с windows версии MSSQL в получившемся sql бекапе необходимо отредактировать пути для mdf и ldf на linux расположение /var/opt/mssql/data/*.{mdf, ldf}
```bash
CREATE DATABASE [DATABASE_NAME]
 CONTAINMENT = PARTIAL
 ON  PRIMARY 
( NAME = N'my_mremoteng', FILENAME = N'/var/opt/mssql/data/DATABASE_NAME.mdf' , SIZE = 8192KB , MAXSIZE = UNLIMITED, FILEGROWTH = 65536KB )
 LOG ON 
( NAME = N'my_mremoteng_log', FILENAME = N'/var/opt/mssql/data/DATA\DATABASE_NAME.ldf' , SIZE = 270336KB , MAXSIZE = 2048GB , FILEGROWTH = 65536KB )
 COLLATE Cyrillic_General_100_CI_AS
GO
```

- Перед импортом создайте ползователя USER_NAME для базы данных и отредактируйте sql скрипт бекапа добавив соответсттвующие права на базу
```bash
CREATE USER USER_NAME FOR LOGIN USER_NAME;
GO
GRANT CONNECT TO USER_NAME;
GO
GRANT SELECT, INSERT, UPDATE, DELETE TO USER_NAME;
GO
```

## Полезные ссылки
- [Microsoft SQL Server - образы на базе Ubuntu](https://hub.docker.com/_/microsoft-mssql-server)\
- [Настройка параметров SQL Server с помощью переменных окружения в Linux](https://learn.microsoft.com/en-us/sql/linux/sql-server-linux-configure-environment-variables?view=sql-server-2017)