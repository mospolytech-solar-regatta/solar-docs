# Настройка solar-boat-connector

solar-boat-connector - центральный модуль системы лодки, он запускается на RaspberryPi в окружении
Linux на ARM. Ниже приведена универсальная инструкция по первичной настройке.

Для работы модуля solar-boat-connector нужны настроенные службы Redis и PostgreSQL,
Если эти службы не настроены, пройдите [инструкцию](../environment.md) по настройке окружения.

### Склонировать репозиторий

```shell
git clone https://github.com/mospolytech-solar-regatta/solar-boat-connector
```

### Перейти в директорию проекта

```shell
cd solar-boat-connector.md
```

### Создать виртуальное окружение

Если вы используете PyCharm, лучше создавать виртуальное
окружение [встроенными средствами PyCharm](https://www.jetbrains.com/help/pycharm/creating-virtual-environment.html))

=== "Windows"

    ``` shell
    python -m venv venv
    ```

=== "Linux"

    ``` shell
    python3 -m venv venv
    ```

=== "MacOS"

    ``` shell
    python3 -m venv venv
    ```

### Активировать виртуальное окружение

Если используется PyCharm, просто откройте вкладку `Terminal`

=== "Windows"

    ``` cmd
    # CMD
    venv\Scripts\activate.bat

    # PowerShell
    .\venv\Scripts\Activate.ps1
    ```

=== "Linux"
    ``` shell 
    source venv/bin/activate
    ```

=== "MacOS"

    ``` shell
    source venv/bin/activate
    ```
### Установить зависимости

=== "Windows"

    ``` shell
    pip install -r requirements.txt
    ```

=== "Linux"

    ``` shell
    sudo apt install python3-dev libpq-dev
    pip install -r requirements.txt
    ```

=== "MacOS"

    ``` shell
    brew install postgresql
    pip install -r requirements.txt
    ```

### Создать конфигурацию запуска

Скопировать example.env в .env и установить следующие поля в соответствии с настройками Redis и PostgreSQL

     - POSTGRES_DB=название базы данных
     - POSTGRES_PASSWORD=пароль базы данных
     - POSTGRES_SERVER=адрес сервера PostgreSQL
     - POSTGRES_USER=созданный пользователь PostgreSQL
     - POSTGRES_PORT=порт сервера PostgreSQL
     - REDIS_DSN=url подключения к redis (стандартный - redis://localhost/)

### Запустить миграции базы данных

=== "Windows"

    ``` shell
    alembic upgrade head
    ```

=== "Linux"

    ``` shell
    alembic upgrade head
    ```

=== "MacOS"

    ``` shell
    alembic upgrade head
    ```

### Запустить проект
Solar-connector будет запущен как веб сервис по адресу http://localhost:8000/.
Для доступа к автодокументации перейти на http://localhost:8000/docs. Если нужно 
запустить проект на другом порте, изменить номер после `--port` в команде запуска.

=== "Windows"

    ``` shell
    uvicorn app.main:app --port 8000
    ```

=== "Linux"

    ``` shell
    uvicorn app.main:app --port 8000
    ```

=== "MacOS"

    ``` shell
    uvicorn app.main:app --port 8000
    ```