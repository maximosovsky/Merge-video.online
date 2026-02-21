# Architecture — Merge-video.online

## Overview

Merge-video.online — Telegram-бот для объединения (конкатенации) нескольких YouTube-видео в одно. Скачивает видео через youtube-dl, объединяет через ffmpeg, отдаёт ссылку на скачивание.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Runtime | Node.js (ESM) |
| Bot | Telegraf.js |
| Video download | youtube-dl (child_process spawn) |
| Video merge | ffmpeg (concat filter) |
| IDs | uuid (v4) |
| Deploy | AWS EC2 (судя по ec2metadata) |

## Project Structure

```
├── index.js              # Единственный файл — бот + вся логика
├── package.json          # Зависимости (telegraf, uuid)
├── last_update_id        # Файл-маркер для дедупликации updates
└── LICENSE               # MIT
```

## Data Flow

```
User sends YouTube URL(s) → bot accumulates in user_map (Map<userId, Map<uuid, {url}>>)
  → User нажимает "Старт"
  → youtube-dl --dump-json → получает metadata + formats
  → choose_res() → подбирает общее разрешение для всех видео
  → youtube-dl -f <format> → скачивает каждое видео
  → ffmpeg -filter_complex concat → объединяет в один .mkv
  → Перемещает в /var/www/html/ → отдаёт HTTP-ссылку
```

## Bot Commands

| Command / Button | Action |
|-----------------|--------|
| `/start` | Приветствие |
| Любой YouTube URL | Добавляет в список |
| «Старт» | Запускает merge pipeline |
| «Статус» | Показывает текущий список |
| «Убрать» (inline) | Удаляет видео из списка |

## Key Concepts

- **In-memory storage** — `user_map` и `status_map` (Maps), данные теряются при перезагрузке
- **Update deduplication** — middleware читает/записывает `last_update_id` файл
- **Resolution matching** — `choose_res()` находит общее разрешение для всех видео (предпочтение landscape)
- **spawn_promise** — обёртка child_process.spawn в Promise
- **EC2 deploy** — использует `ec2metadata --public-hostname` для генерации ссылки
