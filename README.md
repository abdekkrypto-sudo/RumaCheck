# RumaCheck

House tasks, reminders, package tracking, and deep cleaning for a group of housemates.

## Local development

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
# edit .env with your values
python app.py
```

The app creates a default admin user only when you configure the admin environment variables or when running in local development with the provided defaults.

## Environment variables

Copy `.env.example` to `.env` and configure these values:

- `RUMACHECK_SESSION_SECRET`: secret used for Flask sessions
- `SESSION_COOKIE_SECURE`: set to `false` locally, `true` in production HTTPS
- `DATABASE_URL`: database URL, defaults to a local SQLite file
- `BASE_URL`: public URL used for password reset links
- `GROUP_CHAT_LINK`: WhatsApp group link
- `RUMACHECK_ADMIN_USERNAME`: admin username
- `RUMACHECK_ADMIN_PASSWORD`: admin password
- `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASSWORD`, `MAIL_FROM`: email delivery settings

## Deployment

### Render

1. Create a new Web Service from this repository.
2. Set the build command to `pip install -r requirements.txt`.
3. Set the start command to `gunicorn app:app --bind 0.0.0.0:$PORT`.
4. Add the env vars from `.env.example`.
5. Enable auto deploys on the default branch.

A ready-to-use `render.yaml` is included in the repo.

### Railway

1. Import the repo into Railway.
2. Set the start command to `gunicorn app:app --bind 0.0.0.0:$PORT`.
3. Add the same environment variables listed above.
4. Redeploy.

### Heroku

```bash
heroku create
heroku config:set RUMACHECK_SESSION_SECRET=your_secret RUMACHECK_ADMIN_USERNAME=admin RUMACHECK_ADMIN_PASSWORD=change_me
heroku config:set SESSION_COOKIE_SECURE=true
heroku git:remote -a your-app-name
git push heroku main
```

The repo also includes a `Procfile` for Heroku-compatible deployment.

## Security notes

- Do not commit real secrets or production credentials.
- Set `SESSION_COOKIE_SECURE=true` behind HTTPS.
- Use strong admin credentials through environment variables.
- Treat email credentials and reset links as sensitive data.
