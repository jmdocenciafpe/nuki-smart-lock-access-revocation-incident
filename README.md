# nuki-smart-lock-access-revocation-incident
Technical analysis and documentation of a real-world access revocation failure in a Nuki Smart Lock system, focusing on IoT security architecture and access control design.

# Access Revocation Failure in Nuki Smart Lock
## Technical analysis of a real-world IoT access control incident

This repository documents and analyzes a real incident involving a Nuki Smart Lock system,
where it was not possible to reliably revoke and delete an access authorization,
despite the system being fully operational.

The purpose of this repository is technical documentation and security analysis.
It is not accusatory and does not speculate about intent.

---

## Summary

- An access authorization could not be deleted from a Nuki Smart Lock.
- The issue was reproducible across all available interfaces:
  - Web UI
  - Android application
  - Official REST API
- The backend accepted revocation requests but did not guarantee execution on the physical lock.
- As a result, the authorization and associated personal data persisted in the system.

This behavior represents a structural design issue for physical access control systems.

---

## Scope

- Device: Nuki Smart Lock
- Connectivity: Nuki Bridge (operational)
- Interfaces tested: Web, Android app, REST API
- Timeframe: January 2026

---

## Why this matters

In physical access control systems:
- Revocation must be deterministic
- Authorization deletion must be guaranteed
- Data associated with access must be removable

An asynchronous, best-effort revocation model is unsuitable for security-critical environments.

---

## Repository structure

- `INCIDENT.md` – detailed technical description
- `TIMELINE.md` – chronological sequence of events
- `ES/INFORME.md` – original Spanish technical report
- `EVIDENCE/` – screenshots and reproducible evidence
- `CORRESPONDENCE/` – vendor communication (sanitized)

---

## Disclosure

This incident was:
- Communicated to the vendor
- Documented with evidence
- Reported to the Spanish national cybersecurity institute (INCIBE)

This repository exists to contribute to informed discussion on IoT security design.
