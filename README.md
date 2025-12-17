# infra-erpnext

Infrastructure setup for running **ERPNext v16** using Docker, based on the official [frappe_docker](https://github.com/frappe/frappe_docker) repository.  
This project provides environment configuration (`.env`), base compose files, and overrides for local development and production deployment.

---

## 📦 Project Structure

```
infra-erpnext/
├── .env                  # Environment variables (DB, Redis, ERPNext settings)
├── docker-compose.yml    # Base services (MariaDB, Redis, ERPNext)
├── overrides/            # Override compose files for dev/prod
│   ├── docker-compose.override.yml
│   └── docker-compose.prod.yml
└── README.md             # Documentation
```

---

## ⚙️ Prerequisites

- Docker ≥ 20.x  
- Docker Compose ≥ v2.x  
- At least 4 GB RAM available  
- Ports `80` and `443` free (or mapped differently in overrides)

---

## 🔑 Environment Variables

All sensitive and configurable values are stored in `.env`. Example:

```env
MYSQL_ROOT_PASSWORD=root
MYSQL_USER=frappe
MYSQL_PASSWORD=frappe
MYSQL_DATABASE=erpnext
DB_HOST=mariadb
REDIS_CACHE=redis-cache:6379
REDIS_QUEUE=redis-queue:6379
REDIS_SOCKETIO=redis-socketio:6379
```

You can adjust these values depending on your deployment.

---

## 🚀 Usage

### 1. Clone Repository
```bash
git clone https://github.com/your-org/infra-erpnext.git
cd infra-erpnext
```

### 2. Configure `.env`
Edit `.env` with your database credentials, Redis settings, and ERPNext options.

### 3. Start Services
```bash
docker compose up -d
```

This will start MariaDB, Redis, and ERPNext containers.

### 4. Access ERPNext
Open your browser at:
```
http://localhost:8080
```
Run the setup wizard to configure your company, currency, and admin user.

---

## 🛠️ Overrides

- **Development (`docker-compose.override.yml`)**  
  - Maps local volumes for live code editing.  
  - Exposes ports for debugging.  

- **Production (`docker-compose.prod.yml`)**  
  - Adds Nginx reverse proxy.  
  - Configures SSL certificates.  
  - Optimized resource limits.  

Run with:
```bash
docker compose -f docker-compose.yml -f overrides/docker-compose.prod.yml up -d
```

---

## 📑 Maintenance

- **Logs:**  
  ```bash
  docker compose logs -f erpnext
  ```
- **Database Backup:**  
  ```bash
  docker exec -it infra-erpnext-mariadb mysqldump -u root -p erpnext > backup.sql
  ```
- **Update Images:**  
  ```bash
  docker compose pull
  docker compose up -d
  ```

---

## ✅ Best Practices

- Keep `.env` out of version control (use `.env.example` for defaults).  
- Use **volumes** for persistent data (`sites`, `mariadb_data`).  
- For production, always run behind **Nginx + HTTPS**.  
- Regularly back up database and site files.  

---
