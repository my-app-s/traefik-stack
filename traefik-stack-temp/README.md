# 🚦 Traefik Edge Router Stack

> Scalable, production-ready infrastructure template for deploying **Traefik v3** as an edge router with automated Let's Encrypt SSL/TLS termination and dynamic Docker provider integration.

This repository provides a modular configuration designed to support both **horizontal scaling** (shared proxy instances with unified state) and **vertical scaling** (isolated, independent proxy instances via custom project names).

---

## 🚀 Quick Start

1. Clone the repository and copy the example environment file:
```bash
cp .env.example .env

```


2. Adjust the variables in `.env` (make sure to set your real email for SSL certificates).
3. Start the stack in detached mode:
```bash
docker compose up -d

```



---

## ⚙️ Configuration (.env)

| Variable | Description | Default |
| --- | --- | --- |
| `VERSION_T` | Traefik image version | `v3.3` |
| `PROJECT_NAME` | Prefix used for container, network, and volume naming (enables isolation) | `traefik` |
| `FORWARD_PORT_HTTP` | Host port mapped to container HTTP (Port 80) | `80` |
| `FORWARD_PORT_HTTPS` | Host port mapped to container HTTPS (Port 443) | `443` |
| `RESOLVER_NAME` | ACME certificate resolver label name | `myresolver` |
| `ACME_EMAIL` | Email address registered with Let's Encrypt for renewal notices | `admin@localhost.local` |

---

## 🏗️ Architectural Modes

* **Horizontal Scaling (Default):** Leaving `PROJECT_NAME=traefik` uses a shared external network (`traefik_public_network`) and a shared certificate storage volume (`traefik_certificates`), allowing multiple proxy replicas or dependent stacks to link seamlessly.
* **Vertical Scaling (Isolated):** Changing `PROJECT_NAME` (e.g., `traefik-staging` or `proxy-client2`) spins up a completely segregated router with its own dedicated network and storage, ideal for multi-tenant or multi-environment host setups.

---

## 🏷️ Trademarks

This project is an independent work and is not affiliated with or endorsed by Docker or Traefik.

All product names, logos, and brands are the property of their respective owners.

---

## Disclaimer & License

* **Short Disclaimer (EN)**: Materials are provided ***as is*** under the LICENSE file. No warranties, no rights granted unless explicitly stated. Authors are not liable for damages. No partnership or obligations created.
* **Short Disclaimer (RU)**: Материалы предоставляются ***как есть*** и регулируются LICENSE. Гарантий нет, права не передаются без явного указания. Автор(ы) не несут ответственности. Партнёрство или обязательства не создаются.
* **Full Disclaimer**: Read the full text in the [DISCLAIMER.md](https://github.com/my-app-s/my-app-s/blob/main/DISCLAIMER.md) (Available in EN/RU).
* **License**: This project is dual-licensed:
- ​Open Source: [GNU AGPLv3](https://github.com/my-app-s/go-custom-router/blob/main/LICENSE)
- Commercial: A separate proprietary commercial license is available for proprietary and closed-source use. Contact the copyright holder for commercial licensing terms.

## Author & Contacts

* **GitHub**: [@my-app-s](https://github.com/my-app-s)
* **LinkedIn**: [In/my-app-s](https://www.linkedin.com/in/my-app-s)
* **Mail**: [myapps.mre.dev@gmail.com](mailto:myapps.mre.dev@gmail.com)
