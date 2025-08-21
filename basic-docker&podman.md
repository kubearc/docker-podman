# Docker & Podman Command Practice Tasks


This repository contains a set of **hands-on tasks** to practice Docker and Podman commands. The tasks are structured from beginner to advanced so you can gradually build your container skills.


---


## 🔹 1. Basic Setup & Info
- Install **Docker** and **Podman** on your system.
- Check versions:
```bash
docker --version
podman --version
```
- Display system info:
```bash
docker info
podman info
```


---


## 🔹 2. Working with Images
- Search for images:
```bash
docker search nginx
podman search nginx
```
- Pull images:
```bash
docker pull nginx
podman pull nginx
```
- List images:
```bash
docker images
podman images
```
- Remove images:
```bash
docker rmi nginx
podman rmi nginx
```


---


## 🔹 3. Container Basics
- Run container in foreground:
```bash
docker run ubuntu echo "Hello from Docker"
podman run ubuntu echo "Hello from Podman"
```
- Run container in background:
```bash
docker run -d --name web nginx
podman run -d --name web nginx
```
- List running containers:
```bash
docker ps
podman ps
```
- Stop & start containers:
```bash
docker stop web && docker start web
podman stop web && podman start web
```


---


## 🔹 4. Exploring Containers
- Access container shell:
```bash
docker exec -it web bash
podman logs web
```
- Inspect container:
```bash
docker inspect web
podman inspect web
```


---


## 🔹 5. Networking
- Run with port mapping:
```bash
docker run -d -p 8080:80 nginx
podman run -d -p 8080:80 nginx
```
- Create custom network & attach containers.


---


## 🔹 6. Volumes & Storage
- Create a volume:
```bash
docker volume create mydata
podman volume create mydata
```
- Mount volume:
```bash
docker run -d -v mydata:/usr/share/nginx/html nginx
podman run -d -v mydata:/usr/share/nginx/html nginx
```
- List volumes:
```bash
docker volume ls
podman volume ls
```


---


## 🔹 7. Building Images
- Create a `Dockerfile`:
```dockerfile
FROM alpine
CMD ["echo", "Hello from custom image"]
```
- Build image:
```bash
docker build -t myapp:v1 .
podman build -t myapp:v1 .
```
- Run image:
```bash
docker run myapp:v1
podman run myapp:v1
```


---
