# Checkpoint 02 — Caddy Foundation

Date: 2026-09-22
Status: Complete

## Purpose

Establish Caddy as the local reverse proxy layer for the Gradient production server before introducing Cloudflare Tunnel.

## Server

Hostname:
gradient-server

OS:
Ubuntu Server 24.04.5 LTS

Hardware:
HP Compaq 6200 Pro MT
Intel Core i5-2400
4 cores / 4 threads
12 GB RAM
~477 GB SSD

## Caddy

Caddy was installed from the official Caddy Debian repository.

Version:
Caddy v2.11.4

Service:
caddy.service

Service status:
active

Service enabled:
yes

## Current Configuration

Caddy configuration file:

/etc/caddy/Caddyfile

Current configuration intentionally provides only a local HTTP test endpoint.

Automatic HTTPS is explicitly disabled during the local foundation stage.

Test endpoint:

http://127.0.0.1:8088

Expected response:

Gradient Caddy is working

## Validation

Caddy configuration validation completed successfully.

Command:

sudo caddy validate --config /etc/caddy/Caddyfile

Result:

Valid configuration

Caddy service restart completed successfully.

Command:

sudo systemctl restart caddy

Service verification:

sudo systemctl is-active caddy

Result:

active

Listening-port verification confirmed:

*:8088

Port 80 is not being used by Caddy.

Local HTTP validation:

curl -i http://127.0.0.1:8088

Result:

HTTP/1.1 200 OK
Server: Caddy
Content-Type: text/plain; charset=utf-8

Response:

Gradient Caddy is working

## Security Intent

Caddy is NOT directly exposed to the public Internet.

No router port forwarding is being configured.

No public HTTP/HTTPS ports are opened in UFW.

The intended production traffic path is:

Internet
    ↓
Cloudflare
    ↓
Cloudflare Tunnel
    ↓
Caddy
    ↓
Docker services
    ↓
Supabase / Redis / application services

Cloudflare Tunnel will provide the public ingress layer.

## Domain

Primary domain:

gradientclub.in

Infrastructure subdomains will be created under:

gradientclub.in

Planned examples include:

api.gradientclub.in
studio.gradientclub.in
control.gradientclub.in

Exact public routing will be configured after Cloudflare Tunnel is established.

## Architecture Decision

Caddy remains the internal reverse proxy.

Cloudflare Tunnel handles external ingress.

Caddy will route requests to Docker services using internal service names/ports.

Public frontends remain hosted on Vercel.

Docker will not host the public frontend applications.

## Current Infrastructure State

Completed:

- Ubuntu Server
- UFW firewall
- SSH key-only authentication
- Individual administrator accounts
- Tailscale administrative access
- Docker Engine
- Docker Compose
- Production directory structure
- Self-hosted Supabase
- Git infrastructure repository
- GitHub remote
- Caddy installation
- Caddy local validation

Next layer:

Cloudflare Tunnel

After Tunnel:

- Public API routing
- Supabase API validation
- Private Supabase Studio access
- Redis
- Monitoring
- Application services
- Backups
- Gradient Control Center

## Recovery Principle

The production environment remains a single physical server.

Reliability is provided through:

- Version-controlled infrastructure
- Reproducible configuration
- External backups
- Monitoring
- Gradient Edge fallback
- Cloudflare Tunnel
- Individual administrator accounts
- Documented recovery procedures
