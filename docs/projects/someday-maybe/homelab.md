# Homelab

A collection of services ready to be deploy to __private cloud__ of your choice or at home.

## Getting Started

## Prerequisites

* Docker

## Installing

A reverse proxy / load balancer that's easy, dynamic, automatic, fast, full-featured, open source, production proven, provides metrics, and integrates with Docker swarm.  

```
set -a && . ./.env
docker stack deploy --orchestrator=swarm --prune --compose-file=./traefik.yml traefik
```

```
docker service ps traefik_traefik
```

## Status

| Service     | Description                  | Status             |
| ----------- | ---------------------------- | ------------------ |
| Traefik     | Routing                      | :heavy_check_mark: |
| Calibre-web | <https://books.home.local>      | :heavy_check_mark: |
| Gitea       | <https://agentc.myddns.me/git> | :heavy_check_mark: |
| Wekan       |                              | :construction:     |

## Tools

* [Traefik](https://traefik.io/)
