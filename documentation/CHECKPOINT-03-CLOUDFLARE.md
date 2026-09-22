# Checkpoint 03 — Cloudflare Account & DNS Migration

Date: 2026-09-22
Status: Complete

## Purpose

Move Gradient's Cloudflare ownership from a student/personal account to the permanent Gradient Club email account while keeping the domain registered at GoDaddy.

This establishes a long-term organizational Cloudflare account suitable for annual administrator handover.

## Ownership Model

Primary domain:

gradientclub.in

Domain registrar:

GoDaddy

Cloudflare ownership:

Permanent Gradient Club email

Cloudflare plan:

Free

Payment method:

None required for the current architecture

The domain registration remains at GoDaddy.

Cloudflare is used for DNS, CDN/proxying, security features, and the future Cloudflare Tunnel.

## Previous Configuration

The domain was previously managed through a Cloudflare account associated with a student email address.

The old Cloudflare account was not intended to be the permanent organizational owner.

## New Configuration

A new Cloudflare account was created using the permanent Gradient Club email.

The domain gradientclub.in was added to the new account using the Free plan.

The new Cloudflare account is now the permanent organizational Cloudflare account.

## DNS Migration

The existing DNS zone was exported from the old Cloudflare account.

The exported zone was imported into the new Cloudflare account.

The old zone contained 16 DNS records.

All 16 required DNS records were successfully imported into the new account.

Two apex NS records from the exported zone were rejected during import.

This was expected because Cloudflare controls the authoritative nameservers assigned to each zone and does not allow the imported apex NS records to overwrite the new account's assigned nameservers.

The two rejected NS records were therefore not manually added.

## DNS Records Preserved

The migration preserved the existing DNS configuration including:

- Main gradientclub.in Vercel record
- www Vercel record
- QR subdomain
- SPECATHON subdomain
- Birthday API subdomain
- Birthday Posters subdomain
- _domainconnect record
- Cloudflare Email Routing MX records
- send MX record
- SPF
- DKIM
- DMARC
- Resend DKIM
- Existing proxy/DNS-only states

## Nameservers

Old Cloudflare nameservers:

crystal.ns.cloudflare.com
troy.ns.cloudflare.com

New Gradient Cloudflare nameservers:

shane.ns.cloudflare.com
uma.ns.cloudflare.com

The GoDaddy nameservers were changed from the old pair to the new pair.

Cloudflare subsequently recognized the new nameservers and changed gradientclub.in to Active.

## DNSSEC Check

GoDaddy DS Records were checked before the nameserver migration.

No DS records were present.

Therefore no existing DNSSEC DS record needed to be removed before changing nameservers.

## Validation

After the nameserver migration:

- Cloudflare new zone status: Active
- gradientclub.in: working
- www.gradientclub.in: working
- Existing DNS records: preserved
- Existing Vercel services: working
- Domain remained registered at GoDaddy
- Cloudflare ownership successfully moved to the permanent Gradient Club account

## Old Cloudflare Account

After the new Cloudflare zone became Active and the existing services were verified, gradientclub.in was removed from the old student Cloudflare account.

The old Cloudflare account itself was not deleted.

The permanent Gradient Cloudflare account is now the authoritative Cloudflare account for gradientclub.in.

## Security / Ownership Principle

The permanent Gradient email owns the organizational Cloudflare account.

The student email is no longer required for Cloudflare DNS management.

The domain registration remains at GoDaddy and can be addressed separately as part of future organizational account handover.

No personal payment card is required for the current Cloudflare architecture.

## Free Plan Architecture

The current Gradient architecture is designed to remain within Cloudflare's Free plan.

Current intended Cloudflare services include:

- DNS
- CDN/proxy
- SSL/TLS
- DDoS protection
- Free WAF capabilities
- Cloudflare Tunnel
- Turnstile where required

Paid-only services are not part of the current production architecture.

## Next Layer

Cloudflare Tunnel.

Planned public infrastructure hostname:

api.gradientclub.in

Planned traffic path:

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
Self-hosted Supabase

No public inbound ports will be opened on the home server.

No router port forwarding will be used.

SSH remains restricted to administrative access through Tailscale.

## Recovery Principle

The Cloudflare account is now owned by the permanent Gradient organization rather than an individual student.

The DNS configuration has been exported and preserved.

Infrastructure configuration remains version-controlled in the Gradient infrastructure Git repository.

Future administrators can inherit the organizational Cloudflare account without depending on a graduating student's personal account.
