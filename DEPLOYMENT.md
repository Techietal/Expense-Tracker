# Deployment Guide

This app uses **Neon PostgreSQL** (Render Free compatible). No persistent disk is required because all data is stored in Neon.

## Render Environment Variables

Set these in your Render service dashboard:

```env
PORT=8080
ASPNETCORE_ENVIRONMENT=Production
ASPNETCORE_URLS=http://0.0.0.0:8080
ASPNETCORE_FORWARDEDHEADERS_ENABLED=true
ConnectionStrings__DefaultConnection=Host=YOUR_NEON_HOST.neon.tech;Database=neondb;Username=YOUR_USER;Password=YOUR_PASSWORD;SSL Mode=Require;Trust Server Certificate=true
```

Replace:
- `YOUR_NEON_HOST` with your Neon endpoint host
- `YOUR_USER` with your Neon database user
- `YOUR_PASSWORD` with your Neon password
- `neondb` with your actual Neon database name if different

## Render Health Check

Use this health check path in Render:

```text
/healthz
```

## Schema Creation

The app runs `EnsureCreated()` on startup, so the required tables are created automatically in Neon on first launch.

## Docker Image

The GitHub Actions workflow builds and pushes the image to GitHub Container Registry on every push to `main` or `master`:

```text
ghcr.io/YOUR_USERNAME/YOUR_REPO_NAME:latest
```

Point your Render service to that image.

## Notes

- Do not set the real Neon password in `appsettings.json` or commit it to git.
- SQLite is no longer used; old `.db` files are ignored.
