# START: что на что заменить

Всего 3 вещи: **имя**, **ID** и **фото**. Имя и ID задаются переменными на Railway, фото — файлом в проекте.

## Что на что заменить

| Что | Где | Сейчас | Замени на |
|---|---|---|---|
| Имя | Railway → **Variables** → `OWNER_NAME` | не задано (карточки нет) | своё имя, например `Нурдаулет Жумалиев` |
| ID | Railway → **Variables** → `OWNER_ID` | не задано | `@ник_в_telegram` или любой текст, например `ID 12345` |
| Фото | файл `site/photo.jpg` | серая заглушка-силуэт | своё фото **с тем же именем** `photo.jpg` |

- `OWNER_ID` с `@` в начале → становится ссылкой на Telegram (`https://t.me/<ник>`). Без `@` показывается просто текстом.
- Фото: квадратное, от 400×400, JPG. Обрежется в круг само. Имя файла строго `photo.jpg`, папка строго `site/`.
- Карточка (фото + имя + ID) появляется, только когда задан `OWNER_NAME`.

## Шаг 1. Фото (один раз, через GitHub)

Вариант А — на сайте GitHub, без терминала:
1. Открой https://github.com/intellightkz/apple-motion-findings/tree/main/site
2. **Add file** → **Upload files** → перетащи своё фото, предварительно переименовав его в `photo.jpg`.
3. **Commit changes**. Старая заглушка заменится, Railway сам пересоберёт сайт.

Вариант Б — локально:
1. Замени файл `site/photo.jpg` своим (то же имя).
2. В папке проекта:

```bash
git add site/photo.jpg && git commit -m "Update photo" && git push
```

## Шаг 2. Имя и ID на Railway

1. [railway.com](https://railway.com) → проект → сервис `apple-motion-findings`.
2. Вкладка **Variables** → **Raw Editor** → вставь (свои значения, без кавычек):

```
OWNER_NAME=Нурдаулет Жумалиев
OWNER_ID=@nurdaulet
```

3. **Update Variables** → **Deploy**. Через ~30 с сайт обновится.

## Первый деплой (если проекта на Railway ещё нет)

1. [railway.com/new](https://railway.com/new) → **Deploy from GitHub repo** → `intellightkz/apple-motion-findings`.
   Репо не видно → **Configure GitHub App** → дай доступ к репо.
2. Railway сам соберёт `Dockerfile` (настройки в `railway.json`). Порт выставляется сам.
3. Сервис → **Settings** → **Networking** → **Generate Domain** → получишь ссылку `*.up.railway.app`.
4. Сделай шаг 2 (имя и ID).

Дальше каждый `git push` в `main` обновляет сайт автоматически.

## Локальный запуск (по желанию)

```bash
cp .env.example .env
```

Впиши имя и ID в `.env`, затем:

```bash
docker build -t motion-findings . && docker run --rm -p 8080:8080 --env-file .env motion-findings
```

Открой http://localhost:8080.

## Другие файлы, которые можно заменить

| Файл | Что это | Как заменить |
|---|---|---|
| `site/photo.jpg` | Фото в карточке | Свой JPG с тем же именем |
| `site/glass-pay-demo.mp4` | Видео сверху страницы | Свой MP4 (H.264) с тем же именем |
| `site/glass-pay-poster.jpg` | Кадр до загрузки видео | Свой JPG с тем же именем |
| `site/index.html` | Весь текст сайта | Правь текст между тегами; блок `{{ ... }}` в начале `<header>` не трогай |

Не трогать: `Caddyfile`, `Dockerfile`, `railway.json` — это сборка и сервер, всё уже настроено.

## Если что-то не так

- **Карточки нет** → не задан `OWNER_NAME`, или после добавления переменных не нажат **Deploy**.
- **Старое фото** → обнови страницу с очисткой кэша (Cmd+Shift+R). Проверь, что файл называется ровно `photo.jpg` и лежит в `site/`.
- **Деплой упал** → Railway → **Deployments** → лог сборки. Сайт статический, внешних зависимостей нет, кроме образа `caddy:2-alpine`.
