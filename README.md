# rProxy Ansible

## Features
* Nginx Reverse Proxy
  * Generates wildcard certificate for domain with automatic renewal
  * Full end to end encryption between (host -> reverse proxy -> app)
  * Downstream apps can perform Let's Encrypt certificate renewals through reverse proxy
* Dynamic DNS with AWS
* Wireguard VPN
  * Easy client management

## Install
1. Create AWS user with programmatic access and the following policy permissions:

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
2. Update variables in [hosts.yml](hosts.yml)
3. Update variables in [vault.yml](vault.yml)
4. `make vault` to encrypt [vault.yml](vault.yml)
5. `make install` to run playbook
