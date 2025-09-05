# Podman Container Debugging & Troubleshooting Guide

## 1. Run a container and check logs
```bash
podman run -d --name mynginx -p 8080:80 nginx
podman logs mynginx
```

**Sample Output:**
```
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
...
nginx entered RUNNING state, process has stayed up for > than 1 seconds
```

---

## 2. Run a failing container and analyze the logs
```bash
podman run -d --name failcontainer wrongimage:latest
podman logs failcontainer
```

**Sample Output:**
```
Error: initializing source docker://wrongimage:latest: reading manifest latest in docker.io/library/wrongimage: errors:
denied: requested access to the resource is denied
```

---

## 3. Debug inside a running container using `podman exec`
```bash
podman exec -it mynginx bash
```

**Inside Container:**
```bash
root@123abc456def:/# ls -l
bin  boot  dev  etc  home  lib  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

---

## 4. Inspect container configuration
```bash
podman inspect mynginx
```

**Sample Snippet:**
```json
[
  {
    "Id": "a1b2c3d4e5f6",
    "Name": "mynginx",
    "Image": "docker.io/library/nginx:latest",
    "State": {
      "Status": "running",
      "Pid": 2345,
      "ExitCode": 0
    },
    "NetworkSettings": {
      "Ports": {
        "80/tcp": [
          {
            "HostIp": "0.0.0.0",
            "HostPort": "8080"
          }
        ]
      }
    }
  }
]
```

---

## 5. Check live container resource usage
```bash
podman stats mynginx
```

**Sample Output:**
```
CONTAINER ID  NAME       CPU %   MEM USAGE / LIMIT  MEM %  NET IO   BLOCK IO  PIDS
a1b2c3d4e5f6  mynginx    0.15%   20.5MiB / 2GiB     1.0%   1.2kB    0B        5
```

---

## 6. List processes inside a container
```bash
podman top mynginx
```

**Sample Output:**
```
USER   PID   PPID  %CPU  ELAPSED  TTY  TIME   COMMAND
root   1     0     0.0   1h       ?    00:00  nginx: master process nginx -g daemon off;
nginx  7     1     0.0   1h       ?    00:00  nginx: worker process
nginx  8     1     0.0   1h       ?    00:00  nginx: worker process
```

---

## 7. Simulate a container crash
```bash
podman run --name crashdemo alpine sh -c "echo 'Running...'; sleep 3; exit 1"
podman logs crashdemo
```

**Sample Output:**
```
Running...
Error: container exited with code 1
```

---

## 8. Remote debugging: connect to a running container
```bash
podman exec -it mynginx bash
apt-get update && apt-get install -y curl
curl localhost
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

## 9. Remove all failed containers
```bash
podman container prune
```

**Sample Output:**
```
WARNING! This will remove all non running containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
a1b2c3d4e5f6
b7c8d9e0f1g2
```

---

## 10. Five Common Troubleshooting Steps

1. **Check Logs** → `podman logs <container>`  
   Example: Shows startup errors, missing files, or application failures.  

2. **Inspect Configuration** → `podman inspect <container>`  
   Example: Verifies environment variables, ports, and mounted volumes.  

3. **Exec into Container** → `podman exec -it <container> sh`  
   Example: Debug interactively using Linux commands.  

4. **Check Resources** → `podman stats <container>` and `podman top <container>`  
   Example: Detects high CPU/memory usage or zombie processes.  

5. **Verify Network** → Use `ping`, `curl`, or check firewall settings.  
   Example: Ensures container can talk to other services.  

---
✅ This guide provides **commands, expected output, and troubleshooting steps** for Podman containers.
