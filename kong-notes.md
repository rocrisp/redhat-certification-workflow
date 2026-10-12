# Kong Gateway Operator - Certification Notes

> **Findings captured:** 2026-10-08, in preparation for Wednesday 2026-10-14 meeting with Kong.

## Current State

- **Certified Operators Repo:** https://github.com/redhat-openshift-ecosystem/certified-operators/tree/main/operators/kong-gateway-operator
- **Latest certified version:** v0.3.0
- **Latest upstream Kong release:** v2.3.3 (stable), v2.4.0-rapid (pre-release)
- **FBC enabled:** No

### Current ci.yaml
```yaml
cert_project_id: 62fe3916c0ec0946279ffadb
```

---

## What Needs to Be Done

### 1. Enable FBC in ci.yaml

Update `ci.yaml` to:
```yaml
cert_project_id: 62fe3916c0ec0946279ffadb
fbc:
  enabled: true
```

### 2. Add New Bundle for Latest Version

Add a new bundle directory for the latest version (e.g., `v2.3.3`) under:
```
operators/kong-gateway-operator/v2.3.3/
```

### 3. Add release-config.yaml

Inside the new bundle version directory, add a `release-config.yaml` to define catalog channels and the upgrade graph:
```yaml
---
catalog_templates:
  - template_name: basic.yaml
    channels: [stable]
    replaces: kong-gateway-operator.v0.3.0
```

### 4. Submit GitHub PR

Submit a PR to the `certified-operators` repository. The CI pipeline will validate the bundle and FBC catalog automatically.

---

## Key Risk: Upgrade Graph

The jump from **v0.3.0 → v2.3.3** is a large version gap. Kong needs to decide:

- **Option A:** Include intermediate bundles to form a complete upgrade chain
- **Option B:** Use `skipRange` to allow direct upgrades, e.g.:
  ```yaml
  olm.skipRange: ">=0.0.1 <2.3.3"
  ```

This needs to be discussed with Kong before submitting the PR.
