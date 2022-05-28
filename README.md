# rproxy-ansible

## Install

```
ansible-playbook main.yml -i hosts.yml -e @vault.yml --ask-vault-pass
```

## Existing Wireguard Keys

Existing client/server private keys can be used by putting them in their respective directories.

Public keys are not required as they will be generated using the private keys.

### Server Private Key Path
```
/etc/wireguard/privatekey
```

### Client Private Key Path

Each client private key requires its own sub directory within `/etc/wireugard/clients`, where the directory name must match the respective client name in the `wireguard_clients` dictionary:
```
/etc/wireguard/clients/client1/privatekey
/etc/wireguard/clients/client2/privatekey
```

Respective client entries in [hosts.yml](https://github.com/basharkey/rproxy-ansible/blob/main/hosts.yml):

```
wireguard_clients:
- { name: client1,
      address: 192.168.2.2,
      allowed_ips: "{{ wireguard_server_address }}/32, {{ wireguard_internal_network  }}"
  }

- { name: client2,
    address: 192.168.2.3,
    allowed_ips: "{{ wireguard_server_address }}/32, {{ wireguard_internal_network  }}"
  }
```
