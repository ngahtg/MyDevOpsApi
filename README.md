# MyDevOpsApi — CI/CD & Containerization for ASP.NET Core

A hands-on DevOps project: an **ASP.NET Core 8 Web API** packaged with **Docker**, run together with **SQL Server** through **Docker Compose**, and built/published automatically by a **GitHub Actions** pipeline.

> The API itself is the default .NET 8 Web API template (`WeatherForecast`); the focus of this repository is the delivery pipeline around it.

## Tech stack

| Area | Tools |
|---|---|
| Backend | C#, ASP.NET Core 8 Web API, Swagger (Swashbuckle) |
| Container | Docker (multi-stage build), Docker Compose |
| Database | SQL Server 2022 (container) |
| CI/CD | GitHub Actions, Docker Hub |
| API testing | Postman |

## What's inside

- **Multi-stage `Dockerfile`** — builds with the .NET SDK image, then copies only the published output into the lightweight ASP.NET runtime image. The `.csproj` is copied and restored first to take advantage of Docker layer caching.
- **`docker-compose.yml`** — starts the API and a SQL Server container together; the API receives its connection string through environment variables.
- **GitHub Actions pipeline (`.github/workflows/ci.yml`)**
  - On every push / pull request to `main`: restore → build (Release).
  - On push to `main`: log in to Docker Hub using **GitHub Secrets**, then build and push the image `mydevopsapi:latest`.
- **Feature-branch workflow** — changes go through feature branches and pull requests into `main`, with CI validating each PR.

## Run locally

```bash
# Requires Docker Desktop
docker compose up --build
```

- API: http://localhost:5000/WeatherForecast
- SQL Server: `localhost,14333`

Run without Docker:

```bash
cd MyDevOpsApi
dotnet run
```

## CI/CD setup (for forks)

Add these repository secrets under **Settings → Secrets and variables → Actions**:

| Secret | Value |
|---|---|
| `DOCKERHUB_USERNAME` | Your Docker Hub username |
| `DOCKERHUB_TOKEN` | A Docker Hub access token |
