# START: что куда вставлять

Сайт берёт имя, ID и фото из переменных окружения. Код трогать не нужно.

## 1. Переменные

| Переменная | Что это | Пример | Если не задать |
|---|---|---|---|
| `OWNER_NAME` | Имя в карточке сверху | `Нурдаулет Жумалиев` | Карточки не будет |
| `OWNER_ID` | ID или ник под именем | `@nurdaulet` или `ID 12345` | Строки под именем не будет |
| `OWNER_PHOTO` | Фото (аватар) | `https://github.com/intellightkz.png` | Серая иконка-силуэт |

- `OWNER_ID` начинается с `@` → станет ссылкой на Telegram (`https://t.me/<ник>`). Любой другой текст показывается как есть.
- `OWNER_PHOTO` — прямая ссылка на картинку (открывается в браузере как картинка, а не страница). Подходит аватар GitHub (`https://github.com/<ник>.png`), Imgur, любой CDN.
- Фото из файла: положи `photo.jpg` в папку `site/`, запушь и поставь `OWNER_PHOTO=photo.jpg`.
- Лучше квадратное фото от 200×200. Обрежется в круг само.

## 2. Куда вставлять на Railway

1. Открой проект на [railway.com](https://railway.com) → сервис `apple-motion-findings`.
2. Вкладка **Variables** → **New Variable**.
3. Добавь `OWNER_NAME`, `OWNER_ID`, `OWNER_PHOTO` (имя слева, значение справа, без кавычек).
4. Нажми **Deploy** (Railway сам предложит применить изменения). Через ~30 с сайт обновится.

Можно вставить всё разом: **Variables** → **Raw Editor** →

```
OWNER_NAME=Нурдаулет Жумалиев
OWNER_ID=@nurdaulet
OWNER_PHOTO=https://github.com/intellightkz.png
```

## 3. Первый деплой (если проекта на Railway ещё нет)

1. [railway.com/new](https://railway.com/new) → **Deploy from GitHub repo** → `intellightkz/apple-motion-findings`.
   Репо не видно → **Configure GitHub App** → дай доступ к репо.
2. Railway сам соберёт `Dockerfile` (настройки в `railway.json`). Порт выставляется сам.
3. Сервис → **Settings** → **Networking** → **Generate Domain** → получишь ссылку `*.up.railway.app`.
4. Шаг 2 выше: добавь переменные.

Дальше каждый `git push` в `main` обновляет сайт автоматически.

## 4. Локальный запуск (по желанию)

```bash
cp .env.example .env
```

Впиши свои значения в `.env`, затем:

```bash
docker build -t motion-findings . && docker run --rm -p 8080:8080 --env-file .env motion-findings
```

Открой http://localhost:8080.

## 5. Где что лежит

| Файл | Что внутри |
|---|---|
| `site/index.html` | Весь текст сайта. Карточка владельца — в начале `<header>` |
| `site/glass-pay-demo.mp4` | Демо-видео сверху. Заменить: положи свой файл с тем же именем |
| `site/glass-pay-poster.jpg` | Кадр, который виден до загрузки видео |
| `Caddyfile` | Веб-сервер: отдаёт файлы и подставляет переменные в HTML |
| `Dockerfile`, `railway.json` | Сборка и проверка здоровья на Railway |
| `.env.example` | Шаблон переменных для локального запуска |

## Если что-то не так

- **Карточки нет** → не задан `OWNER_NAME`, или после добавления переменных не нажат Deploy.
- **Вместо фото пустой круг** → ссылка `OWNER_PHOTO` ведёт на страницу, а не на картинку. Открой её в браузере: должна открыться только картинка.
- **Деплой упал** → Railway → **Deployments** → лог сборки. Сайт статический, внешних зависимостей нет, кроме образа `caddy:2-alpine`.
