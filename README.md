# WETTZO Telegram Bot

Официальная документация пользовательских ссылок Telegram-бота WETTZO и Android APK.

> Статус: конфигурация проверена в production. Активный release: `20260723-012654-play-lol`.

## Пользовательская схема

- `PLAY NOW` открывается как обычный Telegram Web App через локализованный HTTPS-маршрут `https://wettzo.lol/<lang>/play`.
- Welcome Bonus, Daily Bonuses, Tournaments и Join VIP используют локализованные clean routes `wettzo.lol`.
- `wettzo.lol` передаёт запрос редиректору `wttz.net`, который динамически выбирает доступный конечный домен.
- `wettzo.ton` является отдельным TON/ADNL-маршрутом и не используется кнопкой `PLAY NOW`. Его origin redirect равен `https://wttz.net/?telegram`.
- Support, X, Telegram communities и VIP Manager используют прямые официальные адреса.

Конечный casino candidate не закреплён навсегда: он выбирается по GEO и доступности из браузера пользователя. В свежем аудите был выбран `wettzo3.com`.

## Android APK

Кнопка `ANDROID APK` в Telegram-боте отправляет APK как Telegram-документ через `sendDocument`; она не открывает внешнюю ссылку.

Для отдельной загрузки используется только стабильный GitHub Release asset:

[Скачать Wettzo.apk](https://github.com/Wettzo/wettzo/releases/download/APK/Wettzo.apk)

- размер: `206051` байт;
- SHA-256: `0BEEA4A3C2AF6C8DEB39638294D2510681DCC307057BE3B0FF05E37E1FB355A5`;
- Android signatures: v1/v2/v3;
- WebView `HOME_URL`: `https://wettzo.lol`.

Файл `Wettzo.apk` в корне ветки GitHub `main` является legacy. Для установки следует использовать только stable release URL выше или документ, отправленный ботом.

## Полный аудит

Полная матрица активных кнопок, первых переходов и конечных целей находится в [`docs/LINKS_AND_APK_AUDIT_RU.md`](docs/LINKS_AND_APK_AUDIT_RU.md).
