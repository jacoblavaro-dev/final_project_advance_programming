# Final Project — Advance Programming

A Laravel-based student records manager with full CRUD (create, read, update, delete) functionality and live, real-time updates powered by Laravel Reverb.

## Features

- **Student management** — add, edit, and delete student records (first/last name, email, student number, year level, course).
- **Real-time updates** — when one user adds a student, the list updates live for everyone else via WebSockets (Laravel Reverb + Echo), no page refresh needed.
- **Validation** — unique email/student number enforcement, with proper handling on edit so a student doesn't collide with their own existing record.
- **Authentication** — built on Laravel Breeze/Fortify (login, registration, profile management, two-factor auth support).

## Tech stack

- Laravel 13 (PHP 8.4)
- Blade + Tailwind CSS
- Laravel Reverb (WebSockets) for real-time broadcasting
- SQLite by default locally, MySQL/Postgres supported for deployment
- Docker for containerized deployment (Render-ready via `render.yaml`)

## Local setup

\`\`\`bash
composer install
npm install

cp .env.example .env
php artisan key:generate

# Uses SQLite by default — create the file if it doesn't exist:
touch database/database.sqlite

php artisan migrate

npm run build   # or: npm run dev
php artisan serve
\`\`\`

Visit `http://localhost:8000`, register an account, then go to **Students** to add, edit, or delete records.

### Real-time updates locally

To see the live "new student" broadcast in action, run Reverb alongside the app:

\`\`\`bash
php artisan reverb:start
\`\`\`

## Deployment

This repo is configured for [Render](https://render.com) via `render.yaml`, which deploys two services from the same Dockerfile:

- **`lanto`** — the main Laravel + Apache web app.
- **`lanto-reverb`** — a separate public WebSocket service for real-time broadcasting.

To deploy:

1. Push this repo to GitHub (already done).
2. In Render, create a new **Blueprint** and point it at this repo — it will read `render.yaml` automatically.
3. Set the required env vars Render will prompt for: `APP_URL`, `APP_KEY` (generate with `php artisan key:generate --show`), `REVERB_APP_ID`, `REVERB_APP_KEY`, `REVERB_APP_SECRET`, `DB_CONNECTION`, and `DATABASE_URL`.
4. Deploy — migrations run automatically on startup via `docker/start.sh`.

## Team

Built as a final project for Advance Programming.