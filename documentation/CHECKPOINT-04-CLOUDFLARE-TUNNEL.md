# Checkpoint 04 — Cloudflare Tunnel + Public Connectivity

**Date:** 2026-09-22  
**Status:** Complete

## Purpose

Establish and verify the production Cloudflare Tunnel layer between the
public internet and the Gradient production server before connecting the
public API hostname to self-hosted Supabase.

This checkpoint records the state after successful external connectivity
through Cloudflare Tunnel and Caddy.

---

## Server

- Hostname: `gradient-server`
- Hardware: HP Compaq 6200 Pro MT
- CPU: Intel Core i5-2400, 4 cores / 4 threads
- RAM: 12 GB
- Storage: approximately 477 GB SSD
- OS: Ubuntu Server 24.04.5 LTS

---

## Cloudflare Account

- Domain: `gradientclub.in`
- Cloudflare account ownership: permanent Gradient club account
- Plan: Cloudflare Free
- Registrar: GoDaddy
- No Cloudflare payment method required for the current architecture

The domain's authoritative DNS is managed by the permanent Gradient
Cloudflare account.

---

## Cloudflare Tunnel

- Tunnel type: Named, remotely managed tunnel
- Tunnel name: `gradient-production`
- Cloudflare Tunnel connector: `cloudflared`
- Deployment method: Docker
- Docker image: `cloudflare/cloudflared:latest`
- Restart policy: `unless-stopped`
- Network mode: Docker host networking

The tunnel token is stored only on the production server and is excluded
from Git. It must never be committed to the repository or included in
documentation.

The tunnel provides outbound connectivity from the production server to
Cloudflare and does not require inbound router port forwarding.

---

## Public Hostname

Current public hostname:

`api.gradientclub.in`

Cloudflare route:

`api.gradientclub.in` → `http://127.0.0.1:8088`

The route is managed through the Cloudflare dashboard.

---

## Caddy

- Caddy version: `v2.11.4`
- Installed from the official Caddy repository
- Service: systemd
- Enabled at boot
- Current listener: port `8088`

Temporary validation response:

`Gradient Caddy is working`

This response is intentionally temporary and will be replaced when Caddy
is configured to reverse proxy requests to self-hosted Supabase.

---

## Connectivity Validation

### Local Caddy

Verified:

`http://127.0.0.1:8088`

Result:

`HTTP/1.1 200 OK`

Response:

`Gradient Caddy is working`

### Cloudflare Tunnel

Cloudflare tunnel connectivity pre-checks passed:

- DNS resolution: PASS
- UDP/QUIC connectivity: PASS
- TCP/HTTP/2 connectivity: PASS
- Cloudflare API connectivity: PASS
- Tunnel registered successfully

### Public hostname

Verified:

`https://api.gradientclub.in`

Result:

`HTTP/2 200`

This confirms the public request path through Cloudflare Tunnel to the
Caddy service is operational.

---

## Network Security

UFW remains configured with default-deny incoming traffic.

Only SSH is intentionally exposed through the host firewall.

No public inbound ports are required for Cloudflare Tunnel.

There is no router port forwarding for HTTP, HTTPS, PostgreSQL, Redis,
Supabase, or Caddy.

The temporary firewall rule used during Docker networking diagnostics was
removed after switching `cloudflared` to host networking.

---

## Docker

`cloudflared` is managed using:

`/srv/gradient/infra/cloudflared/compose.yml`

Secret environment data is stored in:

`/srv/gradient/infra/cloudflared/.env`

The `.env` file is protected with restrictive permissions and is excluded
from Git.

The tunnel token must never appear in Git history, documentation, terminal
logs intended for sharing, or screenshots.

---

## Current Public Architecture

Internet

↓

Cloudflare DNS / Proxy

↓

Cloudflare Tunnel

↓

`gradient-cloudflared` Docker container

↓

Host network

↓

Caddy on `127.0.0.1:8088`

↓

Temporary test response

The next layer will replace the temporary Caddy response with:

Caddy → Supabase Envoy → self-hosted Supabase

---

## What This Checkpoint Does Not Cover

The following layers remain to be configured and validated:

- Caddy → Supabase routing
- Public Supabase API validation
- Private Supabase Studio access
- Redis
- Monitoring
- Backup system
- Application services
- CI/CD
- Gradient Control Center
- Gradient Edge fallback
- Disaster recovery validation

---

## Recovery Principle

The production environment remains based on a single physical production
server.

Reliability is provided through:

- version-controlled infrastructure
- external backups
- monitoring
- documented recovery procedures
- Gradient Edge fallback
- individual administrator accounts
- reproducible Docker configuration

This checkpoint provides a known-good recovery point before changing the
Caddy routing layer.

---

## Next Step

Configure Caddy to reverse proxy the public API hostname to the local
Supabase Envoy service and validate the complete path:

Internet → Cloudflare → Tunnel → Caddy → Supabase
