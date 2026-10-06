# nim23 portfolio (archived)

This repository holds an older version of my personal portfolio and blog, built with Next.js and Django. I keep it for reference. It is not the code behind [nim23.com](https://nim23.com), which runs on a newer, separate codebase.

[![Next.js](https://img.shields.io/badge/Next.js-15-black?logo=next.js)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=white)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Django](https://img.shields.io/badge/Django-5.1-092E20?logo=django&logoColor=white)](https://www.djangoproject.com/)
[![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE.txt)

## Pages

| Page | Route | What it shows |
| --- | --- | --- |
| Home | `/` | Short intro, resume download, skills, work experience and recent blog posts |
| About | `/about` | Work experience, skills, education, certifications and interests |
| Projects | `/projects` | Projects with a description, screenshots, the tech used, and preview and GitHub links |
| Blogs | `/blogs` | Blog posts with categories, view counts, likes and comments |
| Snippets | `/snippets` | Code snippets with syntax highlighting, view counts, likes and comments |
| Entertainment | `/entertainment` | Recent videos from my YouTube channel and movies I've watched |
| Stats | `/stats` | Numbers from my GitHub profile |
| Contact | `/contact` | Contact form that sends email through EmailJS |
| Privacy | `/privacy` | Privacy policy |

The footer also has a newsletter signup form.

## How it works

All content is managed in the Django admin. Rich text is edited with TinyMCE, and images and files are stored on Cloudinary.

The Next.js frontend reads that content straight from the same PostgreSQL database. The route handlers under `frontend/src/app/api/` query it with Prisma, and the pages fetch their data from those routes. Views, likes, comments and newsletter signups are written back the same way.

The Django side also has a REST API with Knox token auth. Logged-in users can browse its docs at `/swagger/` and `/redoc/`. In Docker, Django is served by Gunicorn under Supervisor.

## Tech stack

- Frontend: Next.js 15 (App Router), React 19, TypeScript, Tailwind CSS, Prisma, Framer Motion, SWR, next-mdx-remote, Shiki, next-pwa
- Backend: Django 5.1, Django REST Framework, django-rest-knox, drf-yasg, django-tinymce, django-admin-interface, WhiteNoise, Gunicorn, Supervisor
- Database: PostgreSQL
- Services: Cloudinary, EmailJS, Google Analytics, GitHub API, YouTube Data API
- Tooling: Docker Compose, GitHub Actions (ESLint and flake8)

## Repository layout

```
backend/                Django project
  project/              settings, URLs, API router
  portfolios/           experience, skills, education, certifications, projects, interests, movies
  blogs/                blog posts, categories, views and comments
  code_snippets/        snippets, views and comments
  others/               newsletter signups and uploaded files
  users/                custom user model and Knox login
  utils/                shared helpers and mixins
frontend/               Next.js app
  prisma/               Prisma schema for the shared database
  src/app/              pages and /api route handlers
  src/components/       UI components
  src/lib/              data access, page metadata, constants, types
docker-compose.yml      backend and frontend containers
UTILS.md                misc notes
TODO.md                 old backlog
```

## Running it locally

You need Python 3.12, Node.js 20 or newer with Yarn, PostgreSQL, and a Cloudinary account (the backend won't start without the Cloudinary settings). On Debian or Ubuntu you also need `build-essential libffi-dev libjpeg-dev libpq-dev`.

The backend reads `.env` from the repository root and the frontend reads `frontend/.env`. `.env.sample` only covers part of what the code reads, so use the tables below as the full list.

Backend:

```bash
cp .env.sample .env           # then fill it in
cd backend
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver    # http://localhost:8000
```

Frontend:

```bash
cd frontend
yarn install                  # also runs prisma generate
yarn dev                      # http://localhost:3000
```

With Docker, `docker compose up --build` starts the backend on port 8000 and the frontend on port 3000. The backend container runs migrations, creates the superuser and collects static files on startup.

## Environment variables

### Backend (`.env` in the repository root)

| Variable | Purpose |
| --- | --- |
| `MODE` | `DEVELOPMENT`, `STAGING` or `PRODUCTION` |
| `SECRET_KEY` | Django secret key |
| `LOG_LEVEL`, `DJANGO_LOG_LEVEL` | Logging level |
| `DATABASE_NAME`, `DATABASE_USER`, `DATABASE_PASSWORD`, `DATABASE_HOST`, `DATABASE_PORT` | PostgreSQL connection |
| `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET` | Media storage |
| `BACKEND_BASE_URL` | Base URL of the backend |
| `DJANGO_SUPERUSER_USERNAME`, `DJANGO_SUPERUSER_EMAIL`, `DJANGO_SUPERUSER_PASSWORD` | Superuser that `entrypoint.py` creates when the container starts |

### Frontend (`frontend/.env`)

| Variable | Purpose |
| --- | --- |
| `MODE` | `DEVELOPMENT`, `STAGING` or `PRODUCTION` |
| `NIM23_DATABASE_URL` | PostgreSQL connection string for Prisma, pointing at the backend's database |
| `BACKEND_BASE_URL`, `BACKEND_DOMAIN` | Backend URL and domain |
| `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_DEFAULT_VERSION` | Used to build image URLs |
| `EMAIL_JS_SERVICE_ID`, `EMAIL_JS_TEMPLATE_ID`, `EMAIL_JS_COMMENT_TEMPLATE_ID`, `EMAIL_JS_PUBLIC_KEY` | Contact form and comment notification emails |
| `NEXT_PUBLIC_GA_MEASUREMENT_ID`, `GA_PROPERTY_ID`, `GA_PROJECT_ID`, `GA_CLIENT_EMAIL`, `GA_PRIVATE_KEY` | Google Analytics tracking and visitor numbers |
| `DARKSTAR_GOOGLE_API_KEY` | YouTube Data API key for the Entertainment page |
| `GITHUB_ACCESS_TOKEN` | GitHub API token for the Stats page |
| `NEXT_PUBLIC_SITE_URL` | Base URL of the site |
| `NIM23_APPS_SITE_URL` | Target of the Apps link in the navbar and footer |
| `NEXT_PUBLIC_BLOGS_GRID_ITEMS_PER_PAGE`, `NEXT_PUBLIC_BLOGS_LIST_ITEMS_PER_PAGE`, `NEXT_PUBLIC_PROJECTS_ITEMS_PER_PAGE`, `NEXT_PUBLIC_SNIPPETS_ITEMS_PER_PAGE` | Page sizes |
| `NEXT_PUBLIC_UPI` | UPI ID for the support QR code |

## License

MIT. See [LICENSE.txt](LICENSE.txt).

## Contact

- Current portfolio: [nim23.com](https://nim23.com)
- GitHub: [@NumanIbnMazid](https://github.com/NumanIbnMazid)
- LinkedIn: [Numan Ibn Mazid](https://www.linkedin.com/in/numanibnmazid/)
- Email: [numanibnmazid@gmail.com](mailto:numanibnmazid@gmail.com)
