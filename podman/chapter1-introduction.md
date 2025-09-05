# Podman and Container Basics (RHEL/OpenShift Focus)

## 1. Compare Containers with Virtual Machines (3+ differences)
- **Resource Usage**: Containers share the host OS kernel, VMs require a full guest OS.  
- **Startup Time**: Containers start in seconds, VMs take minutes.  
- **Portability**: Containers are lightweight and portable across environments, VMs are heavier and tied to hypervisors.  

---

## 2. Run Your First Podman Container (UBI8 Minimal)
```bash
podman run -it registry.access.redhat.com/ubi8/ubi-minimal bash
```
✅ Launches an interactive shell inside the UBI8 minimal container.

---

## 3. Difference Between Image and Container
- **Image**: A static, read-only template (like an application blueprint).  
- **Container**: A running (or stopped) instance of that image.  
Example: `ubi8:latest` is the image; `container123` running from that image is a container instance.

---

## 4. List Running Containers and Explain Status
```bash
podman ps -a
```
**Sample Output:**
```
CONTAINER ID  IMAGE                                  STATUS         NAMES
a1b2c3d4e5f6  registry.access.redhat.com/ubi8/ubi    Up 5 minutes   myubi
```
- **Up** → container is running.  
- **Exited** → container stopped but exists.  
- **Created** → defined but never started.  

---

## 5. Advantages of Using Containers in Development
1. **Consistency** – Works the same on dev, test, and prod.  
2. **Efficiency** – Lightweight, faster to start/stop compared to VMs.  
3. **Scalability** – Easy to scale services up and down.  

---

## 6. Role of OCI (Open Container Initiative)
- Defines open standards for **container image formats** and **runtimes**.  
- Ensures containers work across Podman, Docker, CRI-O, etc.  
- Promotes vendor neutrality and interoperability.  

---

## 7. Namespaces and cgroups in Linux
- **Namespaces**: Provide isolation (PID, network, mount, IPC, UTS). Each container sees its own "world".  
- **cgroups (control groups)**: Limit and monitor resources like CPU, memory, and I/O for containers.  

---

## 8. Run a Container and Observe Process Isolation
```bash
podman run -dit --name testubi registry.access.redhat.com/ubi8/ubi-minimal bash
podman exec -it testubi bash
ps aux
```
**Inside container sample output:**
```
USER   PID  %CPU %MEM COMMAND
root     1  0.0  0.0 bash
root    15  0.0  0.0 ps aux
```
✅ Only processes inside the container are visible.

---

## 9. Key OpenShift Features on Top of Kubernetes
- **Developer Experience**: Source-to-Image (S2I) builds, templates.  
- **Integrated Security**: RBAC, SCCs, compliance features.  
- **Enterprise Tools**: Monitoring, logging, and CI/CD integration.  
- **Multi-tenancy**: Projects and quotas for teams.  
- **Operator Framework**: Extend Kubernetes with operators.  

---

## 10. Quiz (Answers in Own Words)
**Q1: What makes containers different from VMs?**  
👉 Containers are lightweight, share the host kernel, and are faster to start compared to VMs that run a full OS.  

**Q2: Why are namespaces important?**  
👉 They give each container its own isolated environment (processes, network, mounts, etc.).  

**Q3: What’s the role of cgroups?**  
👉 They ensure containers don’t overuse system resources by setting CPU and memory limits.  

**Q4: Why use OpenShift instead of plain Kubernetes?**  
👉 OpenShift adds enterprise-ready features like developer tools, integrated security, CI/CD, and multi-tenancy.  

**Q5: What is the difference between an image and a container?**  
👉 An image is a blueprint, a container is a live instance created from it.  

---
✅ This guide covers **basics of containers, Podman usage, Linux internals, and OpenShift concepts**.
