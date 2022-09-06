---
title: Быстрый старт
 
contributors: [adeptvin1]
translated: true
---
## Описание
В данном разделе описываются методы тестового развертывания "Feecc Рабочее Место Инженера" для тестирования и оценки системы в производственной среде. В качестве инструмента развертывания системы "Feecc Рабочее Место Инженера" используется инструмент Docker Compose.

### Предварительные настройки для развертывания Feecc Рабочее Место Инженера" в режиме эмуляции оборудования и в полноценном режиме.

#### Установка Docker Engine
1. Обновите систему и установите служебные пакеты для установки Docker:

    Для Debian-based систем:

    ```bash
    sudo apt update -y
    sudo apt upgrade -y
    sudo apt dist-upgrade -y
    sudo apt autoremove -y
    sudo apt-get install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release \
        git  
    ```

2. Удалите старую версию Docker если она была
   
   Для Debian-based систем:

    ```bash
    sudo apt-get remove docker docker-engine docker.io containerd runc
    ```

3. Добавьте официальный GPG ключ Docker и настройте список репозиториев
   
    Для Debian-based систем:

    ```bash
    sudo mkdir -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

4. Установите Docker Engine
   
   Для Debian-based систем:

   ```bash
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
   ```

В данный комплект поставки Docker Engine уже входит Docker Compose, по этому  дополнительных манипуляций с Docker проводить не нужно. 

### Предварительные настройки для развертывания Feecc Рабочее Место Инженера" ТОЛЬКО в полноценном режиме.

#### Подготовка базы данных MongoDB
Для оперативного хранения и доступа к данным в "Feecc Рабочее Место Инженера" используется база данных MongoDB. В ходе подготовки к развертыванию "Feecc Рабочее Место Инженера" вы можете выбрать любой подходящий вам метод развертывания MongoDB, как на своей инфраструктуре так и на облачной, мы приложили несколько ссылок чтобы вам было удобнее ориентироваться:

- [Установка MongoDB на свой сервер](https://www.mongodb.com/try/download/community)
- [Бесплатный MongoDB кластер в Atlas](https://www.mongodb.com/atlas)
- [Аренда MongoDB кластера в Digital Ocean](https://www.digitalocean.com/products/managed-databases-mongodb) 

Мы в свою очередь на первом этапе рекомендуем воспользоваться [бесплатным MongoDB кластером в Atlas](https://www.mongodb.com/atlas), данная платформа является крайне удобной и гибкой в рабочих задачах. Ниже будет инструкция как воспользоваться MongoDB Atlas в связке с "Feecc Рабочее Место Инженера"
1. Вам необходимо зарегистрироваться в [MongoDB Atlas](https://www.mongodb.com/atlas), указать свою почту, логин и пароль.
2. Вам необходимо создать свой первый бесплатный MongoDB кластер нажав на кнопку "Create" в разделе "Database" как на картинке ниже ![mongodb-step1](../images/feecc-system-quickstart/mongodb-step1.png)
3. В разделе "Создание своего MongoDB кластера" вы можете выбрать бесплатный режим "Shared", а также провайдера услуги и месторасположение MongoDB кластера как на рисунке ниже ![mongodb-step2](../images/feecc-system-quickstart/mongodb-step2.png)
4. После того как ваш первый MongoDB кластер был создан вы должны нажать на кнопку "Connect" для уточняния информации по методу подключения к MongoDB кластеру как на рисунке ниже ![mongodb-step3](../images/feecc-system-quickstart/mongodb-step3.png)
5. В разделе методы подключения выберете "Connect your application" как на рисунке ниже ![mongodb-step4](../images/feecc-system-quickstart/mongodb-step4.png)
6. В разделе "Select your driver and version" вам нужно выбрать Python и любую последнию версию от 3.10 как на рисунке ниже ![mongodb-step5](../images/feecc-system-quickstart/mongodb-step5.png)

В результате всех манипуляций у вас будет строка в которой будет храниться информация о методе подключения к вашему MongoDB кластеру , где вместо черного прямоугольника будет ваш логин, а вместо поля `<password>` вам необходимо будет вписать ваш пароль, в формате `mongodb+srv://yourlogin:yourpassword@......` эта строка будет использоваться в качестве значения переменной `MONGODB_URI` в .env файле при развертывании системы. 

#### Создание аккаунта в Pinata.cloud
Для обеспечения быстрой доступности файлов из IPFS в обычной сети связи используется сервис Pinata.cloud. Чтобы завести аккаунт вам необходимо пройти по [ссылке](https://app.pinata.cloud/register) и завести собственный аккаунт, это бесплатно. Вам необходимо сохранить логин и пароль а также дополнительные данные API так как в дальнейшем они будет использоваться в "Feecc Рабочее Место Инженера" в переменных `PINATA_API`, `PINATA_SECRET_API` .

#### Создание аккаунта в YOURLS
Для обеспечения работы сокращателя ссылок вы можете развернуть как свой сервис [YOURLS](https://yourls.org/) так и временно воспользоваться нашим сервисом YOURLS по ссылке [utl.today](https://yourls.org/) 
Вам необходимо сохранить логин и пароль а также ссылку на ваш YOURLS сервис, так как в дальнейшем эти данные будут использоваться в "Feecc Рабочее Место Инженера" в переменных `YOURLS_SERVER`, `YOURLS_USERNAME`, `YOURLS_PASSWORD` 

**ВАЖНО, использование тестового аккаунта [utl.today](https://yourls.org/) в реальном производстве строго запрещено, его можно использовать только для тестов**
**Тестовый аккаунт YOURLS**
`YOURLS_SERVER`:`url.today`
`YOURLS_USERNAME`:`demouser`
`YOURLS_PASSWORD`:`Jhvw*s4ndnEc6ttm`

#### Создание аккаунта Robonomics parachain в экосистеме Polkadot
Для создания аккаунта Robonomics parachain в экосистеме Polkadot воспользуйтесь инструкцией по [ссылке](https://wiki.robonomics.network/docs/en/create-account-in-dapp/#1-using-polkadotjs-browser-extension). Вам необходимо сохранить ключевые фразы аккаунта так как в дальнейшем они будут использованны в переменной `ROBONOMICS_ACCOUNT_SEED`.

### Развертывание Feecc Рабочее Место Инженера 
Для быстрого запуска Feecc Рабочее Место Инженера вы можете воспользоваться репозиторием [feecc-demo](https://github.com/Multi-Agent-io/feecc-demo) в котором мы собрали несколько docker-compose и служебных файлов для развертывания Feecc Рабочее Место Инженера с использованием рекомендованного оборудования а также в режиме эмуляции рекомендованного оборудования.

#### Развертывание Feecc Рабочее Место Инженера с использованием рекомендованного оборудования
Для развертывания Feecc Рабочее Место Инженера с использованием рекомендованного оборудования вам необходимо выполнить следующие условия:

1. Подключите все устройства к компьютеру/серверу на котором будет запущен Feecc Рабочее Место Инженера
   
   - Принтер этикеток к компьютеру/серверу используя USB
   - Сканер штрихкодов и RFID сканер используя USB
   - IP камера через PoE маршрутизатор/коммутатор
   - **Опционально** Сенсорный экран используя USB и HDMI/VGA
  

2. Склонируйте репозиторий [feecc-demo](https://github.com/Multi-Agent-io/feecc-demo)
   
   ```bash
   git clone https://github.com/Multi-Agent-io/feecc-demo.git
   ```

3. Установите mongosh для управления вашей базой данных MongoDB.
   
   ```bash
   sudo apt-get install gnupg
   wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
   echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
   sudo apt-get update
   sudo apt-get install -y mongodb-mongosh
   ```

4. Установите Python3 для генерации идентификатора для пользователя
   ```bash
   sudo apt update
   sudo apt install python3
   ```

5. Перейдите в папку проекта и в папке проекта перейдите в раздел "scripts", сгенерируйте `passport_code` для внесения вашей учетной записи в базу данных.
   
   ```bash
   cd ./feecc-demo/scripts
   python3 ./generate_passport_code.py
   ```
   > Важно! Указывайте данные в генераторе точно также как вы будете вносить их в базу данных.     

6. Используя mongosh импортируйте `002_collections.js` и `003_sampledata` (в `003_sampledata` укажите имя пользователя, `passport_code` и должность. При желании можно импортировать тестовую сборку или по шаблону написать свою)
   ```bash
   ```
   
7. В разделе "feecc-quickstart-supporthardware" откройте файл .env любым файловым редактором и внесите информацию о переменных. Пример заполнения переменных указан в .env файле. 
   
8. Напишите на почту `sales@multi-agent.io` с темой письма `Docker token`, в тело письма текст указывать не обязательно. Автоматически ответным письмом вам придет токен для авторизации в Digital Ocean Container Registry. 
У нас не стоит задачи собрать ваши электронные почты, политика безопасности Digital Ocean не предполагает свободного размещения токенов в github и автоматически их отзывает. Если вы беспокоитесь о сохранности своей электронной почты, то можете воспользоваться любой временной почтой для получения токена.
   
9. Авторизуйтесь в Digital Ocean Contaier Registry который предоставляет компания [Multi-Agent Systems](http://multi-agent.io) для скачивания готовых контейнеров необходимых для развертывание Feecc Рабочее Место Инженера и запустите docker-compose используя ранее запрошенный токен.
   
   ```bash
   docker login -u  -p  registry.digitalocean.com
   sudo docker-compose up -d --build --env-file=./.env
   ```

10. Дождитесь окончания развертывания и проверьте работу системы "Feecc Рабочее Место Инженера" перейдя по ссылке в браузере [http://localhost:3000/](http://localhost:3000/) и попытайтесь авторизоваться используя личную RFID карту которую вы указалии ранее.

#### Развертывание Feecc Рабочее Место Инженера в режиме эмуляции рекомендованного оборудования
Для развертывания Feecc Рабочее Место Инженера в режиме эмуляции рекомендованного оборудования необходимо выполнить следующие условия:

1. Склонируйте репозиторий [feecc-demo](https://github.com/Multi-Agent-io/feecc-demo)
   
   ```bash
   git clone https://github.com/Multi-Agent-io/feecc-demo.git
   ```

2. Перейдите в папку проекта и в папке проекта перейдите в раздел "feecc-quickstart-hardwareemulation"
   
   ```bash
   cd ./feecc-demo/feecc-quickstart-supporthardware
   ```

3. Напишите на почту `sales@multi-agent.io` с темой письма `Docker token`, в тело письма текст указывать не обязательно. Автоматически ответным письмом вам придет токен для авторизации в Digital Ocean Container Registry. 
У нас не стоит задачи собрать ваши электронные почты, политика безопасности Digital Ocean не предполагает свободного размещения токенов в github и автоматически их отзывает. Если вы беспокоитесь о сохранности своей электронной почты, то можете воспользоваться любой временной почтой для получения токена.
   
4. Авторизуйтесь в Digital Ocean Contaier Registry который предоставляет компания [Multi-Agent Systems](http://multi-agent.io) для скачивания готовых контейнеров необходимых для развертывание Feecc Рабочее Место Инженера и запустите docker-compose используя ранее запрошенный токен.
   
   ```bash
   docker login -u  -p  registry.digitalocean.com
   sudo docker-compose up -d --build
   ```

5. Дождитесь окончания развертывания и проверьте работу системы "Feecc Рабочее Место Инженера" перейдя по ссылке в браузере [http://localhost:3000/](http://localhost:3000/) и проверьте работу системы в соответствии с инструкцией [feecc-demo](https://github.com/Multi-Agent-io/feecc-demo)