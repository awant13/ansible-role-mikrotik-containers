# 🚀 Ansible Role: Mikrotik Containers  

> ⚠ **WARNING**: To change container parameters, it must be recreated.  
> The `community.routeros.api` module **does not support idempotency**.  

## 🛠 Features  
- 📦  **Multiple containers** support  
- 🔥 Uses **RouterOS API (`community.routeros.api`)** (No idempotency)  
- 🌐 **Networking**: veth interface (`address`, `gateway`)  
- 🌱 **Environment Variables**: key-value pairs  
- 📂 **Storage**: Host-to-container mounts (`src`, `dst`)  
- 🔀 **Port Forwarding**: `src` → `dst` (`protocol: tcp/udp`)  
- 🏷️ **Registry Support**: External registries (`ghcr.io`)  
-  📌 **State Management**: `present` / `absent`  

---

## 🔧 Requirements  

Before using this role, install the required package:  

```bash
sudo apt install python3-librouteros
```

## 📥 Install role
Add in requirements.yml

```yaml
roles:
  - src: https://github.com/awant13/ansible-role-mikrotik-containers.git
    scm: git
    version: latest
    name: containers
``` 

```bash
ansible-galaxy install -f -r requirements.yml
```

## ⚙️ Role Variables

```yaml
# Vars
container_desc: Ansible_Managed
container_net: DockerNet
container_bridge: Docker-Bridge
container_wan: ether1

# Enable Debug
debug: "true"
```

## 🖥️ Example inventory.yml

```yaml
mik:
  hosts:
    mikrotik.local:
      ansible_connection: local
      mik_username: {{ MIKROTIK_USER }}
      mik_password: {{ MIKROTIK_PASS }}

```

## 📜 Example group_vars all/main.yml

```yaml
containers:
  - name: alpine                                  # Unique container name  
    params:                                       # Parameters for container creation  
      remote-image: alpine
      root-dir: ansible_alpine
    interface:                                    # Define veth interface for the container  
      address: "192.168.40.10/24"
      gateway: "192.168.40.1"
    environments:                                 # Define environment variables (optional)  
      - key: test_key_1
        value: test_value_1
      - key: test_key_2
        value: test_value_2
    mounts:                                       # Define mounts (optional)  
      - name: alpine_dir
        src: /alpine/test
        dst: /etc/alpine
      - name: alpine_dir2
        src: /alpine/test2
        dst: /etc/alpine2
    ports:                                        # Define firewall ports (optional)  
      - src: 8080
        dst: 600
        protocol: tcp
      - src: 8081
        dst: 601
        protocol: tcp
    registry:                                     # Define container registry  
      url: https://ghcr.io
    state: present                                # Set state: present or absent  
```
