# Upgrade Guide

This document provides instructions for upgrading between versions when there are breaking changes.

---

## 0.1.x to 0.2.0

### Environment Variables Renamed

The `UID` and `GID` variables have been renamed to `USER_ID` and `GROUP_ID` to avoid conflicts with reserved shell variables.

**Update your `.env` file:**

```diff
- UID=1000
- GID=1000
+ USER_ID=1000
+ GROUP_ID=1000
```

### Service Network Now External

The `service-net` network is now declared as **external** instead of being created automatically by Docker Compose.

**Action required:** Create the network manually before starting the service:

```bash
docker network create --internal ntfy # or your SERVICE_NET value
```

---

## About

For a complete list of changes, see [CHANGELOG.md](CHANGELOG.md).
