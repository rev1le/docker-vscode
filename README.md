# VSCode Server with Traefik

A self-hosted VS Code Server instance with authentication via Traefik reverse proxy. This setup provides a web-based VS Code editor accessible through your browser with basic auth protection.

## Prerequisites

- Docker
- Docker Compose
- Git

## Project Structure

```
vscode-server/
├── docker-compose.yml      # Docker Compose configuration
├── .env.example            # Environment variables template
├── vscode/                 # VS Code Server Dockerfile
│   ├── Dockerfile
│   └── vscode.sources
├── traefik/                # Traefik configuration
│   └── usersfile           # Basic auth credentials
├── data/                   # VS Code Server data volume
└── README.md
```

## Configuration

### 1. Set up environment variables

Copy the example environment file and update it with your desired configuration:

```bash
cp .env.example .env
```

Edit `.env` and set the `PROJECTS_DIR` to point to your local projects directory:

```
PROJECTS_DIR=/path/to/your/projects
```

### 2. Configure authentication (Optional)

To set up or change basic authentication credentials, edit the `traefik/usersfile`. You can generate credentials using:

```bash
htpasswd -c traefik/usersfile username
```

Or use online tools to generate the format: `username:hashed_password`

## Running the Project

### Start the containers

```bash
docker compose up -d
```

This will:
- Build and start the VS Code Server container
- Start the Traefik reverse proxy
- Create a bridge network connecting both services

### Stop the containers

```bash
docker compose down
```

### View logs

```bash
docker compose logs -f
```

## Accessing VS Code Server

Once running, access VS Code Server at:

```
http://localhost:8080
```

You will be prompted for basic authentication credentials (configured in `traefik/usersfile`).

## Volumes

- `/home/vscode/data` - Mounted to `./data/` on the host (VS Code configuration and extensions)
- `/home/vscode/projects` - Mounted to the directory specified in `PROJECTS_DIR` environment variable

## Services

### VS Code Server
- **Container name**: `vscode-server`
- **Network**: `web`
- **Port**: Exposed via Traefik on port 8080

### Traefik
- **Container name**: `vscode-traefik`
- **Image**: `traefik:v2.10`
- **Port**: `8080:8080` (HTTP access)
- **Authentication**: Basic auth using `traefik/usersfile`

## Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `PROJECTS_DIR` | Path to local projects directory | `/home/user/projects` |
