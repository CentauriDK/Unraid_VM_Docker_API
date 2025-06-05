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
  -p 5888:5888 \
  --net=host \
  --privileged \
  -v /var/run/libvirt:/var/run/libvirt \
  -v /var/run/docker.sock:/var/run/docker.sock \
  centauridk/unraid_vm_docker_api


| Method | Endpoint                   | Description               |
| ------ | -------------------------- | ------------------------- |
| GET    | `/vm/list`                 | List all virtual machines |
| GET    | `/vm/status/<vm_name>`     | Get VM status             |
| POST   | `/vm/start/<vm_name>`      | Start VM                  |
| POST   | `/vm/stop/<vm_name>`       | Graceful stop VM          |
| POST   | `/vm/force-stop/<vm_name>` | Force stop VM             |
| POST   | `/vm/restart/<vm_name>`    | Restart VM                |


| Method | Endpoint                          | Description          |
| ------ | --------------------------------- | -------------------- |
| GET    | `/docker/list`                    | List all containers  |
| GET    | `/docker/status/<container_name>` | Get container status |
| POST   | `/docker/start/<container_name>`  | Start container      |
| POST   | `/docker/stop/<container_name>`   | Stop container       |

Example Usage

Compleate list of created VM's
GET /vm/list
Response:
[   
    {
        "id": "1",
        "name": "Home Assistant",
        "state": "running"
    },    
    {
        "id": "2",
        "name": "Ubuntu",
        "state": "stopped"
    },   
    {
        "id": "3",
        "name": "Windows 11 Work",
        "state": "stopped"
    } 
]

Status of specific VM
GET /vm/status/Home Assistant
Response:
{
  "vm": "Home Assistant",
  "status": "running"
}

Start of specific VM
POST /vm/control
Body:
{
  "vm_name": "Home Assistant",
  "action": "start"
}

Stop of specific VM
POST /vm/control
Body:
{
  "vm_name": "Home Assistant",
  "action": "stop"
}

Restart of specific VM
POST /vm/control
Body:
{
  "vm_name": "Home Assistant",
  "action": "restart"
}

Forced Stop of specific VM
POST /vm/control
Body:
{
  "vm_name": "Home Assistant",
  "action": "force-stop"
}


Compleate list of installed Docker Images
GET /docker/list
Response:
[     
    {
        "name": "zigbee2mqtt",
        "status": "running",
        "uptime": "3 days"
    },
    {
        "name": "nextcloud",
        "status": "running",
        "uptime": "3 days"
    },    
    {
        "name": "MQTT",
        "status": "running",
        "uptime": "3 days"
    },
	{
        "name": "phpmyadmin",
        "status": "stopped",
        "uptime": ""
    }  
]

Status of specific Docker
GET /docker/status/nextcloud
Response:
{
  "container": "nextcloud",
  "status": "running",
  "uptime": "Up 3 days"
}

Start of specific Docker
POST /docker/control
Body:
{
  "container_name": "nextcloud",
  "action": "start"
}

Stop of specific Docker
POST /docker/control
Body:
{
  "container_name": "nextcloud",
  "action": "stop"
}
