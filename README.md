Hi, ![](https://user-images.githubusercontent.com/18350557/176309783-0785949b-9127-417c-8b55-ab5a4333674e.gif)my name is Serhii Shypylov
=========================================================================================================================================

-------------------------------

### Socials
<p align="left">
  <a href="https://t.me/oneitpro">
    <img src="https://img.icons8.com/ios-glyphs/30/ffffff/telegram-app.png" alt="Telegram" width="30" height="30" />
  </a>
  <a href="https://www.linkedin.com/in/sergey-shipilov-7262a31b4/">
    <img src="https://img.icons8.com/ios-glyphs/30/ffffff/linkedin.png" alt="LinkedIn" width="30" height="30" />
  </a>
  <a href="https://www.instagram.com/shipssvpl/">
    <img src="https://img.icons8.com/ios-glyphs/30/ffffff/instagram-new.png" alt="Instagram" width="30" height="30" />
  </a>
  <a href="https://www.facebook.com/profile.php?id=100083345006373">
    <img src="https://img.icons8.com/ios-glyphs/30/ffffff/facebook.png" alt="Facebook" width="30" height="30" />
  </a>
  <a href="https://discord.com/invite/6z5EyagDyW?ref=1it.pro">
    <img src="https://img.icons8.com/ios-glyphs/30/ffffff/discord.png" alt="Discord" width="30" height="30" />
  </a>
  <a href="mailto:admin@1it.pro">
    <img src="https://img.icons8.com/ios-glyphs/30/ffffff/new-post.png" alt="Mail" width="30" height="30" />
  </a>
  <a href="https://1it.pro/">
    <img src="https://img.icons8.com/ios-glyphs/30/ffffff/domain.png" alt="Website" width="30" height="30" />
  </a>
</p>

---
# Installation Guide for OpenWebUI and Ollama using Docker Compose

## Prerequisites
Before proceeding with the installation, ensure that you have the following dependencies installed on your system:

- **Docker** (https://docs.docker.com/get-docker/)
- **Docker Compose** (https://docs.docker.com/compose/install/)

## Step 1: Create the Environment File
Create a `.env` file in the same directory where you will place the `docker-compose.yml` file. This file contains the environment variables for your services.

```sh
# OpenWebUI settings
OPENWEBUI_PORT=8080
OLLAMA_BASE_URL=http://ollama:11434

# Ollama settings
OLLAMA_PORT=11434
OLLAMA_VOLUME=/root/.ollama
WEBUI_VOLUME=/app/backend/data
```

## Step 2: Create the Docker Compose Configuration
Create a `docker-compose.yml` file in your working directory and paste the following content:

```yaml
version: '3.8'

services:
  openwebui:
    image: ghcr.io/open-webui/open-webui:main
    container_name: openwebui
    hostname: openwebui
    restart: unless-stopped
    ports:
      - "${OPENWEBUI_PORT}:8080"
    environment:
      OLLAMA_BASE_URLS: ${OLLAMA_BASE_URL}
    extra_hosts:
      - "host.docker.internal:host-gateway"
    volumes:
      - open-webui-local:${WEBUI_VOLUME}
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:${OPENWEBUI_PORT}"]
      interval: 10s
      retries: 5
      start_period: 15s

  ollama:
    image: ollama/ollama:latest
    container_name: ollama
    hostname: ollama
    restart: unless-stopped
    ports:
      - "${OLLAMA_PORT}:11434"
    volumes:
      - ollama-local:${OLLAMA_VOLUME}

volumes:
  ollama-local:
  open-webui-local:
```

## Step 3: Start the Services
Run the following command in the directory where your `docker-compose.yml` file is located:

```sh
docker-compose up -d
```

This command will:
- Pull the required images
- Create the necessary containers
- Run them in detached mode (`-d` flag)

## Step 4: Verify the Services
Check if the services are running using:

```sh
docker ps
```

You should see `openwebui` and `ollama` running in the list of containers.

To check logs for troubleshooting, use:

```sh
docker logs openwebui
```
```sh
docker logs ollama
```

## Step 5: Access OpenWebUI
Once the containers are up, you can access OpenWebUI in your browser:

```
http://localhost:8080
```

(replace `8080` with the value of `$OPENWEBUI_PORT` if modified in `.env`).

## Managing the Services
### Restarting Services
```sh
docker-compose restart
```

### Stopping Services
```sh
docker-compose down
```

### Updating Containers
To update the containers, pull the latest images and restart the services:
```sh
docker-compose pull
```
```sh
docker-compose up -d --force-recreate
```

## Conclusion
You have successfully set up OpenWebUI and Ollama using Docker Compose. Enjoy using the services!

