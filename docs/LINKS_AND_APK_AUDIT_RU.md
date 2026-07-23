# WETTZO — аудит ссылок и Android APK

## Статус

- active production release: `20260723-012654-play-lol`;
- deploy status: **verified**;
- `wettzo-bot.service`: `active/running`, `NRestarts=0`;
- webhook пустой, `pending_updates = 0`;
- live PLAY, BONUS/VIP и независимый TON-origin проверены после deploy.

## Как читать конечную цель

`wttz.net` выбирает candidate динамически по GEO и browser-probe. Поэтому `wettzo1.com`, `wettzo2.com` или `wettzo3.com` нельзя навсегда закреплять в документации как единственную конечную цель.

В свежем аудите выбранным host был `wettzo3.com`. В таблицах ниже `<candidate>` означает домен, который реально выберет редиректор для конкретного пользователя.

## Главное меню

| Кнопка | Тип Telegram-действия | Результат |
|---|---|---|
| BONUS | callback | открывает локализованный экран с Welcome Bonus, Daily Bonuses и Tournaments |
| VIP CLUB | callback | открывает локализованный экран Join VIP и VIP Manager |
| ANDROID APK | callback + `sendDocument` | бот отправляет встроенный `Wettzo.apk`; URL не открывается |
| WETTZO X | обычный URL | `https://x.com/wettzo?s=11` |
| WETTZO TELEGRAM | обычный URL | локализованная Telegram community |
| CONTACT SUPPORT | Web App | `https://direct.lc.chat/19480896/` |
| PLAY NOW | Web App | локализованный `https://wettzo.lol/<lang>/play` |

Команды `/start`, `/menu`, `/language`, выбор языка и кнопки Back работают внутри бота и не открывают внешние URL.

## PLAY NOW

| Локаль | URL кнопки | Первый HTTP 302 | Конечная цель |
|---|---|---|---|
| EN | `https://wettzo.lol/en/play` | `https://wttz.net/?_wttz_path=%2Fen` | `https://<candidate>/en` |
| FR | `https://wettzo.lol/fr/play` | `https://wttz.net/?_wttz_path=%2Ffr` | `https://<candidate>/fr` |
| DE | `https://wettzo.lol/de/play` | `https://wttz.net/?_wttz_path=%2Fde` | `https://<candidate>/de` |

PLAY использует обычный HTTPS Web App. `wettzo.ton` не является целью этой кнопки.

## BONUS и VIP CLUB

| Действие | Локаль | URL кнопки | Первый HTTP 302 | Конечный pathname |
|---|---|---|---|---|
| Welcome Bonus | EN | `https://wettzo.lol/en/welcome-bonus` | `https://wttz.net/?_wttz_path=%2Fen%2Fpromotions` | `https://<candidate>/en/promotions` |
| Welcome Bonus | FR | `https://wettzo.lol/fr/welcome-bonus` | `https://wttz.net/?_wttz_path=%2Ffr%2Fpromotions` | `https://<candidate>/fr/promotions` |
| Welcome Bonus | DE | `https://wettzo.lol/de/welcome-bonus` | `https://wttz.net/?_wttz_path=%2Fde%2Fpromotions` | `https://<candidate>/de/promotions` |
| Daily Bonuses | EN | `https://wettzo.lol/en/daily-bonuses` | `https://wttz.net/?_wttz_path=%2Fen%2Fdaily-bonuses` | `https://<candidate>/en/daily-bonuses` |
| Daily Bonuses | FR | `https://wettzo.lol/fr/daily-bonuses` | `https://wttz.net/?_wttz_path=%2Ffr%2Fdaily-bonuses` | `https://<candidate>/fr/daily-bonuses` |
| Daily Bonuses | DE | `https://wettzo.lol/de/daily-bonuses` | `https://wttz.net/?_wttz_path=%2Fde%2Fdaily-bonuses` | `https://<candidate>/de/daily-bonuses` |
| Tournaments | EN | `https://wettzo.lol/en/tournaments` | `https://wttz.net/?_wttz_path=%2Fen%2Ftournaments%2Fall` | `https://<candidate>/en/tournaments/all` |
| Tournaments | FR | `https://wettzo.lol/fr/tournaments` | `https://wttz.net/?_wttz_path=%2Ffr%2Ftournaments%2Fall` | `https://<candidate>/fr/tournaments/all` |
| Tournaments | DE | `https://wettzo.lol/de/tournaments` | `https://wttz.net/?_wttz_path=%2Fde%2Ftournaments%2Fall` | `https://<candidate>/de/tournaments/all` |
| Join VIP | EN | `https://wettzo.lol/en/vip-club` | `https://wttz.net/?_wttz_path=%2Fen%2FvipClub` | `https://<candidate>/en/vipClub` |
| Join VIP | FR | `https://wettzo.lol/fr/vip-club` | `https://wttz.net/?_wttz_path=%2Ffr%2FvipClub` | `https://<candidate>/fr/vipClub` |
| Join VIP | DE | `https://wettzo.lol/de/vip-club` | `https://wttz.net/?_wttz_path=%2Fde%2FvipClub` | `https://<candidate>/de/vipClub` |

`_wttz_path` удаляется из query перед конечным переходом. Если точный deep-link недоступен, редиректор может открыть корень уже выбранного candidate.

## Прямые ссылки

| Действие | EN | FR | DE |
|---|---|---|---|
| WETTZO TELEGRAM | `https://t.me/WettzoEn` | `https://t.me/WettzoFR` | `https://t.me/WettzoDE` |
| VIP Manager | `https://t.me/WettzoVIP` | `https://t.me/WettzoVIP` | `https://t.me/WettzoVIP` |
| CONTACT SUPPORT | `https://direct.lc.chat/19480896/` | `https://direct.lc.chat/19480896/` | `https://direct.lc.chat/19480896/` |
| WETTZO X | `https://x.com/wettzo?s=11` | `https://x.com/wettzo?s=11` | `https://x.com/wettzo?s=11` |

X, Telegram communities и VIP Manager являются обычными URL-кнопками. CONTACT SUPPORT является Web App.

## Независимый TON-маршрут

`wettzo.ton` обслуживается через TON/ADNL отдельно от Telegram Web App-кнопок. Его origin redirect должен быть ровно:

```text
https://wttz.net/?telegram
```

Состояние Magic/TON не определяет доступность `PLAY NOW`, потому что PLAY использует `wettzo.lol/<lang>/play`.

## Android APK

### Кнопка в Telegram-боте

Кнопка `ANDROID APK` вызывает callback, после чего бот отправляет встроенный файл через Telegram `sendDocument`. В этом сценарии нет перехода на GitHub или другой сайт.

### Стабильная внешняя загрузка

```text
https://github.com/Wettzo/wettzo/releases/download/APK/Wettzo.apk
```

Эта ссылка должна сохраняться без изменений при замене содержимого release asset.

### Проверенные свойства

| Проверка | Результат |
|---|---|
| Размер | `206051` байт |
| SHA-256 | `0BEEA4A3C2AF6C8DEB39638294D2510681DCC307057BE3B0FF05E37E1FB355A5` |
| ZIP / AndroidManifest | присутствуют и читаются |
| Android signatures | v1/v2/v3 |
| WebView `HOME_URL` | `https://wettzo.lol` |
| Корень `wettzo.lol` | `302` на `https://wttz.net/?apk` |
| Конечный APK candidate | динамический; в свежем аудите `wettzo3.com` |

APK, отправляемый ботом, и stable GitHub release asset должны совпадать по размеру и SHA-256.

`Wettzo.apk` в корне ветки GitHub `main` — legacy-файл. Он не является авторитетным download channel и не должен использоваться вместо stable release URL.

## Инцидент и исправление

Предыдущая конфигурация указывала `PLAY NOW` на `wettzo.ton`. Для Telegram Web App это ненадёжно: `.ton` не является обычным DNS HTTPS-host, а доступность Magic-шлюза может отличаться от состояния raw ADNL.

Текущая production-конфигурация переносит PLAY на обычные локализованные HTTPS routes `wettzo.lol/<lang>/play`. TON-маршрут сохранён независимо для сценария `?telegram` и больше не является зависимостью кнопки PLAY.
