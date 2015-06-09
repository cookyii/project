# Каркас Cookie CMF

## Подготовка окружения

Для начала необходимо установить `nodejs` и `npm`. Установка описана на [GitHub](https://github.com/joyent/node/wiki/Installation).

Для обработки bower пакетов, в системе требуется глобально установить пакет `fxp/composer-asset-plugin`. Делается это командой
```
php composer.phar global require "fxp/composer-asset-plugin:1.0.0"
```
Для полуавтоматического деплоя используется `cookyii/build`. Установить `cookyii/build` не составляет труда.
Для этого необходимо выполнить команду
```bash
composer global require cookyii/build:dev-master
```

Незабывайте периодически обновлять `cookyii/build` выполняя команду
```bash
composer global update cookyii/build
```

## Деплой

Document root:
* Frontend - `./frontend-app/web`
* Backend - `./backend-app/web`

1. Клонировать проект через git.
2. Скопировать файл `.env.dist` в `.env`, заполнить необходимые данные.
3. Скопировать файл `.credentials.env.dist` в `.credentials.env`, заполнить необходимые данные.
4. Установить зависимости через композер `./composer.phar install --prefer-dist` (если нет композера, установить через `./getcomposer`).
5. Развернуть миграции `./frontend migrate`, `./backend migrate`.
6. Установить frontend зависимости через npm `npm install`.

Система готова к эксплуатации.

## Обновление

1. Обновить кодовую базу через git
2. Обновить композер `./composer.phar selfupdate`
3. Обновить зависимости через композер `./composer.phar install --prefer-dist`
4. Развернуть новые миграции `./web migrate`, `./backend migrate`, `./crm migrate`.
5. Оновить frontend зависимости через npm `npm update`

Система готова к эксплуатации.

## Полуавтоматический деплой и обновление

Сначала потребуется скачать обновление из гита командой `git pull`, затем выполнить одну из команд:
* `./build set/production` - собрать проект для продакшена.
* `./build set/demo` - собрать проект для demo площадки.
* `./build set/dev` - собрать проект для dev площадки.

Дополнительно доступны следующие команды (они выполняются в рамках `build/*` команд, и сюда добавлены только для справки):
* `./build map` - показать список всех команд.
* `./build clear` - удалить все временные файлы и логи во всех приложениях.
* `./build clear/*` - удалить все временные файлы и логи в конкретном приложении.
* `./build migrate` - выполнить все новые миграции для всех приложений.
* `./build migrate/*` - выполнить все новые миграции для конкретного приложения.
* `./build less` - скомпилировать less для всех приложений.
* `./build less/*` - скомпилировать less для конкретного приложения.
* `./build composer` - установить `composer` зависимости из `composer.lock`.
* `./build composer/update` - скачать новые версии `composer` зависимостей и обновить `composer.lock`.
* `./build composer/selfupdate` - обновить `composer`.
* `./build rbac` - обновить правила `rbac` для всех приложений.
* `./build rbac/*` - обновить правила `rbac` для конкретного приложения.

## Основные моменты

За основу каркаса взят `yii2-advance-application`.
Он позволяет создавать комплекс из разных приложений с разными подходами
(обычное `web` приложение, `cli` приложение, `rest` приложение).

На данный момент, в проекте созданны следующие приложения:
* `frontend-app` - основное web приложение.
* `backend-app` - приложение для панели управления.

## Структура директорий

```
backend-app/           общий код приложения backend
backend-modules/       модули приложения backend
common/                общие компоненты для всех приложений
frontend-app/          общий код приложения frontend
frontend-modules/      модули приложения frontend
messages/              переводы приложений для всех приложений
resources/             общие ресурсы (модели) для всех приложений
runtime/               общие временны данные для всех приложений
vendor/                пакеты сторонних разработчиков
```
