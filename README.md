# rproxy-ansible

```
ansible-playbook main.yml -i hosts.yml -e @vault.yml --ask-vault-pass
```

If you would like to use existing private keys for wireguard:

put server private key:

/etc/wireguard/privatekey


client private keys

/etc/wireguard/clients/client-name-privatekey
