# ansible-role-mikrotik-containers

## Requirements

python3-librouteros

```bash
sudo apt install python3-librouteros
```

## Role Variables

```yaml
desc: AnsibleControl
container_net: DockerNet
container_bridge: Docker-Bridge
```

## Example inventory.yml

```yaml
mik:
  hosts:
    mikrotik.local:
      ansible_connection: local
      mik_username: {{ MIKROTIK_USER }}
      mik_password: {{ MIKROTIK_PASS }}

```

## Example group_vars all/main.yml

```yaml
containers:
  - name: alpine
    params:
      remote-image: alpine
      root-dir: ansible_alpine
    interface:
      address: "192.168.40.10/24"
      gateway: "192.168.40.1"
    enviroments:
      - key: test_value_1
        value: test_key_1
      - key: test_value_2
        value: test_key_2
    mounts:
      - name: alpine_dir
        src: /alpine/test
        dst: /etc/aplpine
      - name: alpine_dir2
        src: /alpine/test2
        dst: /etc/aplpine2
    ports:
      - src: 8080
        dst: 600
        protocol: tcp
      - src: 8081
        dst: 600
        protocol: tcp
    state: present # present or absent
