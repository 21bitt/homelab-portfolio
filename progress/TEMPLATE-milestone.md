# Milestone: <short title>

**Date:** YYYY-MM-DD  
**Status:** draft | verified | rolled back

## Objective

What capability you are adding (one paragraph).

## Threat / operations assumption

Why this matters for security or day-2 operations.

## Design

- VLANs / subnets affected:
- Firewall rule philosophy (least privilege):
- Dependencies (other services):

## Implementation summary

High-level steps only. **Do not** put real IPs, VLAN IDs, interface names, switch port numbers, hostnames, or config exports in this public repo—see [`docs/portfolio-scope.md`](../docs/portfolio-scope.md). Sanitized configs stay **outside** Git or in a **private** store.

## Verification

- Tests run (ping, DNS, blocked path, expected alert):
- Evidence (screenshot references or log excerpts — no secrets):

## Rollback

How to undo safely (config restore, snapshot, port mode).

## Lessons learned

What broke, what you would do differently (valuable for interviews).
