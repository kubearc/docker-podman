# Chapter5 Persisting Data

# Podman Storage: Volumes and Bind Mounts Practice

## 1. Run a Container with a Bind Mount to Share Data with Host
```bash
mkdir ~/mydata
echo "Hello from host" > ~/mydata/host.txt

podman run -it --rm -v ~/mydata:/data registry.access.redhat.com/ubi8/ubi-minimal bash
```
Inside container:
```bash
cat /data/host.txt
```

---

## 2. Verify Changes in Host Directory Reflect Inside Container
On host:
```bash
echo "New file from host" > ~/mydata/new.txt
```
Inside container:
```bash
cat /data/new.txt
```

---

## 3. Create a Named Podman Volume and Mount It to a Container
```bash
podman volume create myvol
podman run -it --rm -v myvol:/appdata registry.access.redhat.com/ubi8/ubi-minimal bash
```
Inside container:
```bash
echo "Persisted data" > /appdata/test.txt
```

---

## 4. Check Details of the Created Volume
```bash
podman volume inspect myvol
```

---

## 5. Run a Database Container with Persistent Volume (PostgreSQL Example)
```bash
podman volume create pgdata

podman run -d --name mydb \
  -e POSTGRES_PASSWORD=secret \
  -v pgdata:/var/lib/postgresql/data \
  -p 5432:5432 \
  docker.io/library/postgres:15
```

---

## 6. Stop and Restart the DB Container to Ensure Data Persists
```bash
podman stop mydb
podman rm mydb
podman run -d --name mydb \
  -e POSTGRES_PASSWORD=secret \
  -v pgdata:/var/lib/postgresql/data \
  -p 5432:5432 \
  docker.io/library/postgres:15
```

---

## 7. Backup Data from a Volume by Copying It to Host
```bash
podman run --rm -v pgdata:/dbdata -v ~/backup:/backup \
  registry.access.redhat.com/ubi8/ubi-minimal \
  tar czf /backup/pg_backup.tar.gz -C /dbdata .
```

---

## 8. Remove Unused Volumes
```bash
podman volume prune
```

---

