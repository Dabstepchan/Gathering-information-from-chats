# Gathering information from chats
Бот для сбора отчетов из клиентских чатов. Проверяет наличие сообщений с заданными хештегами за указанный период. Отправляет отчеты в личные сообщения, сохраняет в Google-таблицу.
## Требования
- **OpenServer 5.4.1** — локальный сервер;
- **PHP 8.1** — язык программирования;
- **Apache 2.4** — веб-сервер;
- **MySQL 8.0** — база данных;
- **ngrok 3.19.1** — публичный доступ к локальному серверу;
## Создание Telegram-бота:
1. Перейдите в Telegram и найдите бота [@BotFather](https://t.me/BotFather);
2. Используя команды BotFather, создайте нового бота:
   - `/newbot` — укажите имя для бота;
   - Сохраните API-ключ, который выдает BotFather;
   - Созданного бота добавьте во все необходимые чаты.
## Настройка Google Sheets API:
1. Перейдите на [Google Cloud Console](https://console.cloud.google.com/);
2. Создайте новый проект;
3. Активируйте API Google Sheets;
4. Создайте **Service Account**:
   - Перейдите в раздел **Credentials**.
   - Нажмите **Create Credentials → Service Account**.
   - Укажите имя, создайте, и на вкладке ключей добавьте новый ключ в формате JSON.
   - Скачайте файл ключа и переименуйте его в `credentials.json`.
5. В проекте:
   - Загрузите файл `credentials.json` в директорию: `\tg-bot.su\storage\app\google`.
## Настройка проекта:
1. Переименуйте файл `.env.example` в `.env`;
2. Сгенерируйте ключ приложения командой: `php artisan key:generate`;
3. Введите свои данные в env:
- **GOOGLE_APPLICATION_NAME** — название таблицы;
- **GOOGLE_SHEET_ID** — ID листа;
- **GOOGLE_CLIENT_ID** — ID клиента (с Google Sheets API);
- **GOOGLE_CLIENT_SECRET**;
- **ADMIN_USER_ID** — ID пользователя Telegram, которому будут приходить отчёты;
- **APP_URL** - при использовании ngrok заменить на созданный сервисом адрес;
## Настройка Google таблицы:
1. Создайте таблицу Google Sheets;
2. Добавьте в качестве редактора email сервисного аккаунта.
## Запуск сервера:
1. Запустите OpenServer;
2. Поднимите локальный сервер командой: `php artisan serve`.
## Настройка ngrok (при необходимости):
1. Зарегистрируйте бесплатный аккаунт на [ngrok](https://ngrok.com/);
2. Запустите ngrok для перенаправления локального сервера командой: `ngrok http 8000` (или другой используемый порт).
## Настройка БД:
1. Создайте базу данных, по необходимости измените `.env`;
2. Выполните миграции для создания таблиц командой: `php artisan migrate`;
3. Согласно инструкциям Telegraph, внесите бота в таблицу командой: `php artisan telegraph:new-bot`;
4. Введите ключ из [@BotFather](https://t.me/BotFather), затем имя бота, добавляем чат к боту (первым чатом добавьте пользователя, который будет получать отчеты, для это введите свой Telegram ID), сделайте вебхук;
5. Добавьте все необходимые чаты через следующую команду: `php artisan telegraph:new-chat`;
6. Введите ID чата (можно получить через `/chatid`), затем имя бота.
После проведенных действий, регестрируем вебхук командой: `php artisan telegraph:set-webhook`.
## Добавление задачи в планировщик:
1. Используйте Windows Task Scheduler или Cron, в зависимости от системы;
2. В качестве задачи добавьте запуск PHP, указав аргумент schedule:check-reports;
3. Триггер - ежедневно, выполнять задачу раз в минуту.
