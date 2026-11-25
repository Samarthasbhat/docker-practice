# docker-practice

A small FastAPI app containerized with Docker. I built it to learn Docker: writing a Dockerfile, building images, running containers, and pushing to a registry.

## Endpoints

| Route | Description |
|---|---|
| `GET /` | Returns a hello message |
| `GET /health` | Health check, returns `{"status": "ok"}` |
| `GET /docs` | Auto-generated API docs (Swagger UI) |

## Tech stack

- Python 3.12
- FastAPI + Uvicorn
- Docker

## Run with Docker

Build and run locally:

```bash
docker build -t myapp .
docker run -d -p 8000:8000 --name myapp myapp
```

Then open http://localhost:8000.

Or pull the prebuilt image from Docker Hub:

```bash
docker run -d -p 8000:8000 samarthasbhat/myapp:v1
```

Stop and remove the container:

```bash
docker stop myapp && docker rm myapp
```

## Run without Docker

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn main:app --reload
```

## Development with live reload

Mount the code into the container so edits show up without rebuilding:

```bash
docker run -d -p 8000:8000 -v $(pwd):/app --name myapp myapp \
  uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

## Project structure

```
.
├── main.py             # FastAPI app
├── requirements.txt    # Python dependencies
├── Dockerfile          # Image build instructions
├── .dockerignore       # Files excluded from the image
└── README.md
```

## Docker notes

- The container runs as a non-root user (`ram`).
- Dependencies are installed before copying the code, so rebuilds use the layer cache.

## Roadmap

- [ ] Add a `HEALTHCHECK` to the Dockerfile
- [ ] Add Docker Compose with Postgres and Redis
- [ ] Build and push the image automatically with GitHub Actions
