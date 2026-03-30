# GalaxyEye Multi-Agent n8n (Simple Setup)

# Preview
![img alt](https://github.com/AkshathaaRk/n8n-agents/blob/main/n8nimg.png)

## Project overview

This project is an n8n workflow that routes user messages to specialized AI agents:

- Chat specialist
- Coding specialist
- Task specialist
- File specialist
- LLM reasoning specialist

We host n8n on a Linux laptop using Docker, then expose localhost with Cloudflare Tunnel so other devices can access it.

Workflow to import:
- `GalaxyEye Multi-Agent System-public.json`

---

## Quick setup (minimal)

### 1) Install Docker (one-time)

```bash
sudo apt update
sudo apt install -y docker.io docker-compose-plugin
sudo systemctl enable --now docker
```

### 2) Start n8n container

```bash
docker volume create n8n_data
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -e N8N_BASIC_AUTH_ACTIVE=true \
  -e N8N_BASIC_AUTH_USER=admin \
  -e N8N_BASIC_AUTH_PASSWORD=change_this_password \
  -v n8n_data:/home/node/.n8n \
  --restart unless-stopped \
  n8nio/n8n:latest
```

### 3) Start Cloudflare tunnel to localhost (no install needed)

```bash
docker run --rm -it --network host cloudflare/cloudflared:latest tunnel --url http://l
```

Cloudflare will print a public URL like:
- `https://xxxx.trycloudflare.com`

Open that URL on your other laptop/phone to access n8n.

---

## Import this project workflow

1. Open n8n UI
2. Go to **Workflows** → **Import from File**
3. Choose `GalaxyEye Multi-Agent System-public.json`
4. Add your Gemini/API credentials in n8n
5. Save and activate

---

## Daily use commands

```bash
# Start n8n
docker start n8n

# Stop n8n
docker stop n8n

# Logs
docker logs -f n8n
```

To expose again, run Cloudflare command again:

```bash
docker run --rm -it --network host cloudflare/cloudflared:latest tunnel --url http://localhost:5678
```

