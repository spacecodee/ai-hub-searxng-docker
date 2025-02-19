# AI Hub with SearXNG, Open WebUI, ComfyUI, and Ollama

This repository contains Docker Compose configurations for setting up an AI hub that includes:

- SearXNG (Privacy-focused metasearch engine)
- Open WebUI (Web interface for Ollama)
- ComfyUI (Advanced Generate Image Interface)
- Ollama (Local LLM runner)

## Components

| Name                                                   | Description                       | Port             |
|--------------------------------------------------------|-----------------------------------|------------------|
| [SearXNG](https://github.com/searxng/searxng)          | Privacy-focused metasearch engine | 8080             |
| [Open WebUI](https://github.com/open-webui/open-webui) | Web interface for Ollama          | 3000             |
| [ComfyUI](https://github.com/comfyanonymous/ComfyUI)   | Stable Diffusion interface        | 8188             |
| [Ollama](https://github.com/ollama/ollama)             | Local LLM runner                  | 11434 (internal) |
| [Caddy](https://github.com/caddyserver/caddy)          | Reverse proxy                     | 80, 443          |
| [Valkey](https://github.com/valkey-io/valkey)          | In-memory database                | internal         |

## Prerequisites

1. [Install Docker](https://docs.docker.com/install/)
2. [Install Docker Compose](https://docs.docker.com/compose/install/)
3. For ComfyUI: NVIDIA GPU with appropriate drivers installed
4. Git
5. **Mandatory** | Go to the open webui docs before you run any docker compose, you need to follow the steps to create
   the necessary
   files to run searxng with open
   webui. [Open WebUI Docs - SearXNG](https://docs.openwebui.com/tutorials/web-search/searxng)

## Docker Compose Files

There are three docker compose, those are:

1. `docker-compose-w-ollama-comfyui.yaml` - This docker compose file includes all the services, SearXNG, Open WebUI,
   ComfyUI, and Ollama.
2. `docker-compose-w-ollama.yaml` - This docker compose file includes SearXNG, Open WebUI, and Ollama.
3. `docker-compose.yaml` - This docker compose file includes only SearXNG with SearXNG. (This option is better if you
   run ollama in a server like runpod, and it exposes the port 11434 through a URL, so, you can use this docker in your local machine keeping the searxng and open webui configurations in your local machine)

## Quick Start (Local Development)

1. Clone the repository:

```bash
git clone https://github.com/spacecodee/ai-hub-searxng-docker.git
cd ai-hub-searxng-docker
```

2. Create a `.env` file in the root directory:

```env
SEARXNG_HOSTNAME=localhost

# Adjust these values based on your system's capabilities
SEARXNG_UWSGI_WORKERS=6
SEARXNG_UWSGI_THREADS=6
```

3. Start the services:

```bash
docker compose -f docker-compose-w-ollama-comfyui.yaml up -d
```

## Accessing the Services

After starting the services, you can access them at:

- SearXNG: http://localhost:8080
- Open WebUI: http://localhost:3000
- ComfyUI: http://localhost:8188

**If you want to run this docker-compose in your local machine that is connected to any server, you need to forward the
ports using ssh:**

```bash
ssh -L 8080:localhost:8080 -L 3000:localhost:3000 -L 8188:localhost:8188 user@server-ip
```

## Configuring Open WebUI with SearXNG

Open WebUI can be integrated with SearXNG to provide web search capabilities. To set this up:

1. Access Open WebUI at http://localhost:3000
2. Go to Settings > Extensions
3. Enable the Web Search extension
4. Configure the SearXNG settings:
    - SearXNG URL: `http://your-ip-or-container-name:8080/search?q=<query>` (Must be like that)
    - Search Path: `/search`

For more detailed information about the Open WebUI and SearXNG integration, visit
the [official documentation](https://docs.openwebui.com/tutorials/web-search/searxng).

## ComfyUI Information

The ComfyUI installation includes:

- Base ComfyUI
- ComfyUI-Manager (automatically installed on startup)
- Support for custom nodes

### ComfyUI Directory Structure:

```
comfyui/
├── models/      # Place your models here
├── output/      # Generated images appear here
├── input/       # Place input images here
└── custom_nodes/ # Custom nodes directory
```

## Updating

To update the services:

```bash
# Pull the latest changes
git pull

# Rebuild and restart the containers
docker compose -f docker-compose-w-ollama-comfyui.yaml down
docker compose -f docker-compose-w-ollama-comfyui.yaml up -d --build
```

## Troubleshooting

### Viewing Logs

To view logs for all services:

```bash
docker compose -f docker-compose-w-ollama-comfyui.yaml logs -f
```

For specific services:

```bash
# SearXNG logs
docker compose -f docker-compose-w-ollama-comfyui.yaml logs -f searxng

# ComfyUI logs
docker compose -f docker-compose-w-ollama-comfyui.yaml logs -f comfyui

# Open WebUI logs
docker compose -f docker-compose-w-ollama-comfyui.yaml logs -f openwebui

# Ollama logs
docker compose -f docker-compose-w-ollama-comfyui.yaml logs -f ollama
```

### Common Issues

1. **ComfyUI Manager not installing**: Check the ComfyUI container logs for any error messages during startup.
2. **GPU not detected**: Ensure NVIDIA drivers are properly installed and the NVIDIA Container Toolkit is configured.
3. **Services not starting**: Check if the required ports are available and not used by other applications.

## Important Notes

- This configuration is intended for local development and testing purposes.
- For production deployments, additional security measures should be implemented:
    - Change default passwords
    - Configure proper SSL/TLS certificates
    - Implement authentication
    - Adjust worker and thread counts based on server capacity
    - Configure proper firewalls and access controls

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.