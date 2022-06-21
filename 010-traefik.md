# 100 Days of Homelab: Day #10 - Traefik
I've changed my plan !

## Traefik
Initially, I wanted to use 2 reverse-proxies, one for handling everything in my docker swarm (traefik), and one for handling my other stuff, as well as the requests from outside of my local network (npm, or bunkerweb). The issue with this setup is that I've noticed that depending on if I'm accessing my stuff from inside or outside my LAN, it's not the same reverse-proxy that'll handle the certificate. If I reach my service from inside, traefik will handle it, but if I reach it from outsid, through a reverse-proxy "waterfall", it'll be handled by the top one, aka NPM (for now). I could try to bypass that by setting up a TCP streams with filters, instead of a proper reverse-proxy for the top level RP, but I figured I would probbly be better off if I could use a single RP instead. So the new plan for now is to try and setup only traefik as my RP, with both docker and file providers so that I can reverse-proxy stuff ouside of my swarm. This way, I'll also be able to use a single middleware for authentication, through the authelia one I setup on my swarm cluster.

## Consul ???
In addition, I think this setup might push me to look into consul, which could help me with service discovery on servers that are not in my cluster, instead of using file provider, but that's a story for another day !

PS: here is my current traefik stack if anyone's interested.

```yaml
---
version: '3.8'

services:
  #! services
  traefik:
    image: traefik:latest
    command:
      # classic cloudflare setup
      - "--ping=true"
      - "--api.dashboard=true"
      - "--entrypoints.http.address=:80"
      - "--entrypoints.https.address=:443"
      - "--providers.docker=true"
      - "--providers.docker.swarmMode=true"
      - "--providers.docker.watch=true"
      - "--providers.docker.exposedbydefault=false"
      - "--providers.docker.network=reverse-proxy"
      - "--certificatesresolvers.cloudflare.acme.email=email@domain.tld"
      - "--certificatesresolvers.cloudflare.acme.storage=/certificates/acme.json"
      - "--certificatesresolvers.cloudflare.acme.dnschallenge.provider=cloudflare"
      - "--certificatesresolvers.cloudflare.acme.dnschallenge.resolvers=1.1.1.1:53,1.0.0.1:53"
      - "--entrypoints.http.http.redirections.entryPoint.to=https"
      - "--entrypoints.http.http.redirections.entryPoint.scheme=https"
      # file provider setup
      - "--providers.file.directory=/config"
      - "--providers.file.watch=true"
      # plugins setup
      - "--experimental.plugins.rewrite-body.modulename=github.com/packruler/rewrite-body"
      - "--experimental.plugins.rewrite-body.version=v1.0.1"
    secrets:
      - cf_email
      - cf_traefik_key
    deploy:
      mode: replicated
      replicas: 1
      placement:
        constraints:
          - node.role == manager
      update_config:
        parallelism: 1
        delay: 3s
      labels:
        - 'traefik.enable=true'
        # dashboard exposition
        - 'traefik.http.routers.dashboard.rule=Host(`cloud.domain.tld`) && (PathPrefix(`/api`) || PathPrefix(`/dashboard/`))'
        - 'traefik.http.routers.dashboard.service=api@internal'
        - 'traefik.http.routers.dashboard.tls=true'
        - 'traefik.http.routers.dashboard.tls.certresolver=cloudflare'
        - 'traefik.http.routers.dashboard.middlewares=authelia@docker'
        # healthcheck exposition
        - 'traefik.http.routers.ping.rule=Host(`cloud.domain.tld`) && PathPrefix(`/ping`)'
        - 'traefik.http.routers.ping.service=ping@internal'
        - 'traefik.http.routers.ping.tls=true'
        - 'traefik.http.routers.ping.tls.certresolver=cloudflare'
        # dummy port exposition - can be any integer value
        - 'traefik.http.services.dummy-svc.loadbalancer.server.port=9999'
    environment:
      CF_API_EMAIL_FILE: "/run/secrets/cf_email"
      CF_DNS_API_TOKEN_FILE: "/run/secrets/cf_traefik_key"
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - data:/config
      - certs:/certificates
    ports:
      - target: 80
        published: 80
        mode: host
      - target: 443
        published: 443
        mode: host
    networks:
      - reverse-proxy

#! networks
networks:
  reverse-proxy:
    external: true

#! volumes
volumes:
  certs:
    driver: glusterfs
    name: "brick0/reverse-proxy/certificates"
  data:
    driver: glusterfs
    name: "brick0/reverse-proxy/data"

#! secrets
secrets:
  cf_traefik_key:
    external: true
  cf_email:
    external: true
```

And here is my provider domain.tld.yml (I do 1 file per service, because I find it clearer)

```yaml
# domain.tld, www.domain.tld
http:
  routers:
    homer:
      service: homer
      entryPoints: "https"
      tls: 
        certResolver: "cloudflare"
      middlewares: "authelia@docker"
      rule: "Host(`domain.tld`) || Host(`www.domain.tld`)"

  services:
    homer:
      loadBalancer:
        servers:
          - url: "http://10.1.20.10:8070"

```


See you tomorrow !