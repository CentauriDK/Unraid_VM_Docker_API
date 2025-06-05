# unraid_vm_docker_api

A lightweight Flask-based REST API to control and monitor **KVM virtual machines** and **Docker containers** on an Unraid server.



---

## 🔧 Features

- Start / Stop / Restart **KVM Virtual Machines**
- Start / Stop / Check Status of **Docker Containers**
- List available VMs and containers
- Show clean status ("running" or "stopped")
- Include container **uptime**
- ESP32-friendly JSON output

---

## 🐳 Docker Setup (Unraid-Compatible)

Run this container on your Unraid server:

```bash
docker run -d \
  --name unraid_vm_docker_api \
  --net=host \
  --privileged \
  -v /var/run/libvirt:/var/run/libvirt \
  -v /var/run/docker.sock:/var/run/docker.sock \
  centauridk/unraid_vm_docker_api
