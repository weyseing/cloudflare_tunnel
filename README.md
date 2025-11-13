# Setup Guide
- **Install cloudflared**
```bash
wget -O cloudflared https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64

chmod +x cloudflared

sudo mv cloudflared /usr/local/bin/

cloudflared --version
```

- **Start dummy server**
```bash
python3 -m http.server 8000
```

# Quick Cloudflare Tunnel
- **Startup quick tunnel**
```bash
cloudflared tunnel --url http://localhost:<PORT>
```

# WAF-Protected Tunnel
- **Access to Cloudflare via https://dash.cloudflare.com/login**
- **Add domain** by `Free Plan`

![image](./assets/1.PNG)

- **Create tunnel**
```bash
cloudflared tunnel login
cloudflared tunnel create my-tunnel
```

- **Output Response**
```bash
Tunnel credentials written to /home/jeremy/.cloudflared/85b27403-cb6e-4440-956b-4964010c2fae.json. cloudflared chose this file based on where your origin certificate was found. Keep this file secret. To revoke these credentials, delete the tunnel.
```

- **Configure tunnel via `~/.cloudflared/config.yml`** 
    - MUST update `credentials-file` below
``` bash
tunnel: my-tunnel
credentials-file: /home/jeremy/.cloudflared/<UUID>.json

ingress:
  - service: http://localhost:8001
  - service: http_status:404
```
