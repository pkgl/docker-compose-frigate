# Frigate with Nginx Reverse Proxy and Automatic Let's Encrypt Certificates

This project provides a **Docker Compose setup** to run [Frigate](https://docs.frigate.video/) behind an **Nginx reverse proxy** using the [docker-nginx-certbot](https://github.com/JonasAlfredsson/docker-nginx-certbot) container.  
It automatically generates and renews **Let's Encrypt TLS certificates** via HTTP challenge.

---

## Requirements

- A valid **domain name** with an **A record** pointing to the public IPv4 address of your server
- Public IPv4 address (reachable from the internet)
- Port **80** must be open for the HTTP challenge

More details: [good_to_know.md](https://github.com/JonasAlfredsson/docker-nginx-certbot/blob/master/docs/good_to_know.md)

---

## Configuration

Before starting the stack, adjust the following files:

### 1. `nginx-certbot.env`

```env
CERTBOT_EMAIL=your@email.com
```

- Replace with your valid email address (used by Let's Encrypt for certificate notifications).

---

### 2. `nginx/nginx.conf`

```nginx
server_name example.domain.com;

ssl_certificate         /etc/letsencrypt/live/example.domain.com/fullchain.pem;
ssl_certificate_key     /etc/letsencrypt/live/example.domain.com/privkey.pem;
ssl_trusted_certificate /etc/letsencrypt/live/example.domain.com/chain.pem;
```

- Replace `example.domain.com` with your own **DNS A record**.

---

## Usage

After adjusting the configuration files, start the containers with:

```bash
docker compose up
```

- The reverse proxy and Frigate will start.  
- A valid Let's Encrypt certificate will be generated automatically.  
- You can then access Frigate securely via your configured domain.

---

## Good to Know

- TLS is **disabled inside Frigate itself** because TLS termination happens at the reverse proxy.  
  More details: [Frigate reverse proxy guide](https://docs.frigate.video/guides/reverse_proxy/).

---
