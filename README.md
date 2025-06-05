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
        "state": "shut off"
    },   
    {
        "id": "3",
        "name": "Windows 11 Work",
        "state": "shut off"
    } 
]

GET /vm/status/Home Assistant
Response:
{
  "vm": "Home Assistant",
  "status": "running"
}

POST /vm/control
Body:
{
  "vm_name": "Home Assistant",
  "action": "start"
}

POST /vm/control
Body:
{
  "vm_name": "Home Assistant",
  "action": "stop"
}

POST /vm/control
Body:
{
  "vm_name": "Home Assistant",
  "action": "restart"
}

POST /vm/control
Body:
{
  "vm_name": "Home Assistant",
  "action": "force-stop"
}


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


GET /docker/status/nextcloud
Response:
{
  "container": "nextcloud",
  "status": "running",
  "uptime": "Up 3 days"
}

POST /docker/control
Body:
{
  "container_name": "nextcloud",
  "action": "start"
}

POST /docker/control
Body:
{
  "container_name": "nextcloud",
  "action": "stop"
}
