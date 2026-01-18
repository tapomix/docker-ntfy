# Tapomix / Docker - ntfy

Self-hosted push notification service running with [Docker Compose](https://docs.docker.com/compose/) behind [Traefik](https://traefik.io/traefik).

[ntfy](https://ntfy.sh/) allows you to send notifications to your phone or desktop via simple HTTP PUT/POST requests.

## Installation

Clone the repository:

```bash
git clone https://github.com/tapomix/docker-ntfy.git ntfy
```

## Configuration

### Environment file

Copy the `.env.dist` file to `.env` and customize it:

```bash
cp .env.dist .env
```

### Environment Variables

| Variable | Description |
| -------- | ----------- |
| `CONTAINER_NAME` | Name of the Docker container |
| `SERVICE_NET` | Internal network name (default: `${CONTAINER_NAME}`) |
| `SERVICE_VERSION` | Docker image version (default: `latest`) |
| `TRAEFIK_HOST` | Public hostname for Traefik routing |
| `TRAEFIK_NET` | Traefik network name |
| `TRAEFIK_PORT` | Internal port exposed to Traefik (default: `80`) |
| `USER_ID` | User ID for file permissions (default: `1000`) |
| `GROUP_ID` | Group ID for file permissions (default: `1000`) |
| `TZ` | Timezone (default: `Etc/UTC`) |

### ntfy Configuration

The server configuration is defined in `.docker/config.yaml`:

| Setting | Value | Description |
| ------- | ----- | ----------- |
| `auth-default-access` | `deny-all` | Requires authentication for all topics |
| `enable-login` | `true` | Enables web UI login |
| `enable-signup` | `false` | Disables public registration |
| `attachment-total-size-limit` | `1M` | Max total size for attachments |
| `message-size-limit` | `1k` | Max size per message |
| `behind-proxy` | `true` | Optimized for Traefik reverse proxy |

## Usage

### Start the container

```bash
docker compose up -d
```

### Create an admin user

```bash
docker compose exec ntfy ntfy user add --role=admin <username>
```

### Create an API token

Generate a token for use in client services (scripts, automation, etc.):

```bash
# Token that never expires
docker compose exec ntfy ntfy token add <username>

# Token with expiration and label
docker compose exec ntfy ntfy token add --expires=90d --label="my-service" <username>

# List existing tokens
docker compose exec ntfy ntfy token list
```

### Subscribe to a topic

1. Install the [ntfy app](https://ntfy.sh/docs/subscribe/phone/) on your phone
2. Add your server URL (`https://<your-host>`)
3. Subscribe to a topic (e.g. `alerts`)
4. Authenticate with your username/password or access token

## Resources

- [ntfy documentation](https://docs.ntfy.sh/)
