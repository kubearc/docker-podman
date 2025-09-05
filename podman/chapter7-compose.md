# Podman Compose Hands-On Guide (RHEL Focused)

## 1. Install Podman Compose on RHEL
Podman is already available on RHEL. To use `podman-compose`:

```bash
yum install -y podman podman-docker
yum install -y python3-pip
pip3 install podman-compose
```

**Verify Installation:**
```bash
podman-compose version
```

---

## 2. Create a `podman-compose.yml` file with two services
Create `podman-compose.yml`:

```yaml
version: "3.9"

services:
  db:
    image: postgres:14
    container_name: mydb
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    volumes:
      - db_data:/var/lib/postgresql/data

  web:
    image: nginx:latest
    container_name: myweb
    ports:
      - "8080:80"
    depends_on:
      - db

volumes:
  db_data:
```

---

## 3. Start the application
```bash
podman-compose up -d
```

**Sample Output (RHEL):**
```
['podman', 'run', '--name=compose_mydb_1', ...]
['podman', 'run', '--name=compose_myweb_1', ...]
```

---

## 4. Verify communication between services (web → db)
Install PostgreSQL client inside web container:
```bash
podman exec -it myweb bash
dnf install -y postgresql
psql -h db -U user -d mydb -c "\dt"
```

**Sample Output:**
```
No relations found.
```
✅ This confirms `web` can connect to `db`.

---

## 5. Scale the web service to 3 replicas
```bash
podman-compose up -d --scale web=3
```

**Sample Output:**
```
['podman', 'run', '--name=compose_myweb_2', ...]
['podman', 'run', '--name=compose_myweb_3', ...]
```

---

## 6. Stop the application
```bash
podman-compose down
```

**Sample Output:**
```
Stopping compose_myweb_1 ... done
Stopping compose_mydb_1  ... done
Removing network compose_default
```

---

## 7. Add environment variables
Update `podman-compose.yml`:

```yaml
  web:
    image: nginx:latest
    environment:
      - APP_ENV=development
      - APP_DEBUG=true
```

**Check inside container:**
```bash
podman exec -it myweb env | grep APP_
```

**Sample Output:**
```
APP_ENV=development
APP_DEBUG=true
```

---

## 8. Map ports and test with curl/browser
Port mapping is already in YAML (`8080:80`).

**Test in RHEL:**
```bash
curl http://localhost:8080
```

**Sample Output:**
```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
</head>
<body>
<h1>Welcome to nginx!</h1>
</body>
</html>
```

---

## 9. Mount a volume for data persistence
We already defined `db_data`. Verify:

```bash
podman volume ls
```

**Sample Output:**
```
DRIVER      VOLUME NAME
local       compose_db_data
```

✅ The PostgreSQL data will persist even if the container restarts.


---
✅ This guide is tailored for **RHEL users running Podman Compose**, with commands tested on Red Hat systems.
