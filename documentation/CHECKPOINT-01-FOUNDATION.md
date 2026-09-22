# Checkpoint 01 — Foundation + Self-Hosted Supabase

**Date:** 2026-09-22  
**Status:** Complete

## Purpose

Baseline infrastructure checkpoint before deploying Caddy and Cloudflare Tunnel.

## 1. Server

- Hardware: HP Compaq 6200 Pro MT
- CPU: Intel Core i5-2400, 4 cores / 4 threads
- RAM: 12 GB
- Storage: ESSENCORE SATA SSD, approximately 477 GB
- OS: Ubuntu Server 24.04.5 LTS
- Hostname: gradient-server

## 2. Security Baseline

- UFW enabled with default-deny incoming policy.
- SSH uses public-key authentication.
- SSH password authentication disabled.
- Root SSH login disabled.
- Tailscale installed for private administration.
- Supabase network ports are bound to localhost.

## 3. Docker

- Docker Engine installed from the official Docker repository.
- Docker Compose plugin installed.
- Docker access is performed through sudo.
- Administrators are not members of the docker group.

## 4. Self-Hosted Supabase

The official Supabase Docker deployment is running on the Gradient server.

Services currently deployed:

- PostgreSQL
- Supavisor
- Auth / GoTrue
- PostgREST
- Realtime
- Storage
- Edge Functions
- Studio
- Postgres Meta
- Imgproxy
- Envoy API Gateway

All services were healthy at checkpoint creation.

## 5. Network Exposure

Supabase services are intentionally not directly exposed to the LAN or Internet.

- Envoy: 127.0.0.1:8000
- Supavisor: 127.0.0.1:5432
- Supavisor transaction mode: 127.0.0.1:6543

The intended future public path is:

Vercel → Cloudflare → Cloudflare Tunnel → Caddy → Supabase

Caddy and Cloudflare Tunnel have not yet been deployed.

## 6. Resource Baseline

After the Supabase deployment:

- Approximately 1.7 GiB RAM in use
- Approximately 9.9 GiB RAM available
- 4 GiB swap configured and essentially unused
- Approximately 19 GiB used on the 98 GiB root filesystem
- Approximately 75 GiB root filesystem available
- Additional LVM capacity remains available on the physical SSD

## 7. Git / Infrastructure Management

The infrastructure directory is version controlled.

Git tracks infrastructure configuration and documentation.

Git must not contain:

- Secrets
- Passwords
- API keys
- JWT signing keys
- Database credentials
- Runtime database data
- Backups
- Logs
- Caches
- Docker runtime data
- SSH private keys

The official Supabase source checkout is treated as external reference material and is excluded from this repository.

Production Supabase configuration files remain version controlled.

## 8. Next Infrastructure Layers

1. Caddy reverse proxy
2. Local proxy validation
3. Cloudflare Tunnel
4. Public API validation
5. Redis
6. Monitoring
7. Application services
8. Backup system
9. Gradient Control Center

## 9. Recovery Principle

This server is a single production machine and therefore is not true high availability.

Reliability will be achieved through:

- Version-controlled infrastructure
- Reproducible configuration
- External backups
- Monitoring and alerts
- Gradient Edge fallback
- Documented administration procedures
- Individual administrator accounts
