# Incident Description

## Description

An access authorization could not be deleted from a Nuki Smart Lock system.

Attempts were made through:
- Nuki Web interface
- Android mobile application
- Official REST API

In all cases, the revocation request was accepted,but the authorization persisted on the physical lock.

---

## Partial mitigation

A partial mitigation was applied by setting the authorization validity date to a past timestamp, preventing active use of the access.

However:
- The authorization record was not deleted
- Associated personal data remained stored
- The system entered an inconsistent state

This does not constitute proper revocation or data deletion.

---

## Root cause analysis

The issue is not related to:
- Bluetooth
- Wi-Fi
- Connectivity
- User error

The root cause is architectural:
- Asynchronous backend-driven revocation
- Lack of forced deletion mechanisms
- No emergency or override procedure

---

## Security and data protection impact

- Persistent unauthorized access risk
- Inability to guarantee access removal
- Retention of personal data without effective deletion

---

## Status

As of publication, the incident remains unresolved.
