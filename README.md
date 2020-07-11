# Reverse Proxy

This role sets up a reverse proxy, facilitated by [Traefik](https://containo.us/traefik/), to route HTTP traffic to the containers. 

> Current `master` targets Traefik 2.x, for Traefik 1.x support use [`traefik-1.x` branch](https://github.com/OneOffTech/ansible-role-reverseproxy/tree/traefik-1.x)

## Requirements

A working Docker installation with Docker Compose.

For automatic certificate generation via LetsEncrypt a domain name has to point toward your host address.

## Configuration Variables

```yaml
reverseproxy:
  # Email used for notify certificate expiration
  letsencrypt_email: "root@example.com" 
  path: "/home/user/reverseproxy" # Where the proxy service will reside
  data: "/data/reverseproxy/cert" # Where the certificates will be persisted
  conf: "/data/reverseproxy/conf" # Where the configuration will be persisted
  log_level: "ERROR"
  api: "traefik.domain.com" # domain where the dashboard will be reachable
  api_user: "user:passwd" # username and password to protect the access to the dashboard (Use htpasswd to generate the passwords) https://docs.traefik.io/middlewares/basicauth/
```

## Upgrading installations

To upgrade the installation it is sufficient to simply run this playbook again after increasing
the version number.

### Upgrade from Traefik 1 to Traefik 2

Traefik version 2 is a major release and introduce some breaking changes in the configuration files
and labels. 

- Create a backup of all files and configuration;
- The network name to attach containers has been renamed to `traefik_web`. 
  The full network name depends on the deployment folder, so if the reverse proxy is deployed under `/home/user/reverseproxy`,
  the complete network name will be `reverseproxy_traefik_web`;
- Change `cert` folder as the acme format changed in Traefik version 2 and is not backward compatible, e.g. `data: "/data/reverseproxy/cert-t2"`;
