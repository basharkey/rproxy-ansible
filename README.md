# rproxy-ansible

DOCS OUT OF DATE

## Features

### Nginx Reverse Proxy

- Generates wildcard certificate for domain with automatic renewal

- Full end to end encryption between (host -> reverse proxy -> app)

- Downstream apps can perform Let's Encrypt certificate renewals through reverse proxy

### Dynamic DNS with AWS

### Wireguard VPN

- Easy client management

## Install

1. Update variables within [hosts.yml](https://github.com/basharkey/rproxy-ansible/blob/main/hosts.yml)

2. Create AWS user with programmatic access and the following policy permissions:

    [aws-ddns](https://github.com/basharkey/aws-ddns)
    
    ```
    route53:ChangeResourceRecordSets
    route53:ListResourceRecordSets
    ```
    
    [certbot](https://certbot-dns-route53.readthedocs.io/en/stable/)
    
    ```
    route53:ListHostedZones
    route53:GetChange
    route53:ChangeResourceRecordSets
    ```
3. Add AWS user key pair to [vault.yml](https://github.com/basharkey/rproxy-ansible/blob/main/vault.yml) and encrypt vault

4. Run the playbook:
    ```
    ansible-playbook main.yml -i hosts.yml -e @vault.yml --ask-vault-pass
    ```

### Nginx Client Certificate Authentication

Client certificate authentication can be enabled for a site by including `ssl_verify_client: yes` within a site definition.

If enabled for a site the CA file (ca.crt) must be available in one of the following locations to be copied to the host:

```
./rproxy-ansible/ca.crt
./rproxy-ansible/files/ca.crt
./rproxy-ansible/roles/nginx/files/ca.crt
```

Site definition example in [hosts.yml](https://github.com/basharkey/rproxy-ansible/blob/main/hosts.yml):

```
sites:
  - { sub_domain: nextcloud,
      proxy_pass: https://nextcloud.example.com,
      ssl_verify_client: yes,
      hosts_entry: 192.168.1.2
    }
```

### Existing Wireguard Keys

Existing client/server private keys can be used by putting them in their respective directories.

Public keys are not required as they will be generated using the private keys.

#### Server Private Key Path

```
/etc/wireguard/privatekey
```

#### Client Private Key Path

Each client private key requires its own sub directory within `/etc/wireugard/clients`, where the directory name must match the respective client name in the `wireguard_clients` dictionary:

```
/etc/wireguard/clients/client1/privatekey
/etc/wireguard/clients/client2/privatekey
```

Client definition examples in [hosts.yml](https://github.com/basharkey/rproxy-ansible/blob/main/hosts.yml):

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
