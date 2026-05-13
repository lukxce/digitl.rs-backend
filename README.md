# Digitl

Headless CMS built with [Strapi](https://strapi.io/) v5. This project exposes a REST API for articles, client showcases, pages (about, global settings), and public forms (subscribers, messages).

## Requirements

- **Node.js** 20.x through 24.x (see `engines` in `package.json`)
- **npm** 6 or newer

Strapi uses **SQLite** by default (`DATABASE_CLIENT` unset or `sqlite`); the database file lives under `.tmp/data.db` unless you override `DATABASE_FILENAME`.

## Quick start

1. **Install dependencies**

   ```bash
   npm install
   ```

2. **Environment**

   Copy the example env file and set secrets (Strapi will not start with placeholder values in production-like mode; for local dev, replace the `toBeModified` values with random strings):

   ```bash
   cp .env.example .env
   ```

   Edit `.env` and set at least: `APP_KEYS`, `API_TOKEN_SALT`, `ADMIN_JWT_SECRET`, `TRANSFER_TOKEN_SALT`, `JWT_SECRET`, `ENCRYPTION_KEY`. Optional: `HOST`, `PORT` (default `1337`).

3. **Run in development**

   ```bash
   npm run develop
   ```

   Or:

   ```bash
   npm run dev
   ```

   - **Admin panel:** [http://localhost:1337/admin](http://localhost:1337/admin) (create the first admin user on first launch)
   - **API:** [http://localhost:1337/api](http://localhost:1337/api)

4. **Production build**

   ```bash
   npm run build
   npm run start
   ```

## Scripts

| Command | Description |
| --- | --- |
| `npm run develop` / `npm run dev` | Strapi with auto-reload and admin Vite dev server |
| `npm run build` | Build the admin panel for production |
| `npm run start` | Start Strapi in production mode (run `build` first) |
| `npm run console` | Open Strapi interactive console |
| `npm run seed:example` | Import example seed data (first run only; see `scripts/seed.js`) |
| `npm run strapi` | Strapi CLI passthrough |

## Optional: example data

If the database has never been seeded, you can load template content once:

```bash
npm run seed:example
```

If seeding was already applied, clear the database or remove the Strapi store flag described in `scripts/seed.js` before re-importing.

## Content overview

Typical collection types and APIs (paths under `/api` use pluralized API IDs, e.g. `articles`, `client-showcases`):

- **Articles** — blog-style entries with blocks, cover, categories, **key takeaways**
- **Client showcase** — case studies with slug, media, **success rate** items, **key takeaways**, blocks `content`
- **Categories**, **Authors**
- **About** — single type with blocks
- **Global** — site-wide settings
- **Subscribers** — `POST /api/subscribers` with `{ "data": { "email": "..." } }` (public create when permission is enabled)
- **Messages** — `POST /api/messages` with `{ "data": { "email": "...", "text": "..." } }`

Use `populate=*` or explicit `populate` query params when you need relations and components in API responses.

## Project layout

- `config/` — database, server, admin, middleware, plugins
- `src/api/` — content types (schemas, controllers, routes, services)
- `src/components/` — shared components used by content types
- `scripts/seed.js` — example seed importer
- `types/generated/` — TypeScript definitions (regenerated when schemas change)

## TypeScript types

After changing content-type or component schemas:

```bash
npx strapi ts:generate-types
```

## Learn more

- [Strapi 5 documentation](https://docs.strapi.io/)
