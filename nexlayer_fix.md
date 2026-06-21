# Nexlayer working build fix

This file is the authoritative, pinned build solution for this repo. Nexlayer uses it verbatim on every run and will not override it. If a future build with this fix fails, Nexlayer appends/updates it rather than regenerating.

## CRITICAL BUILD CONSTRAINTS

1. **Use the pre-built GHCR image — do NOT build from source** — Building from source takes 20+ minutes. Use `ghcr.io/open-webui/open-webui:main`.

2. **Port is 8080** — open-webui serves on port 8080.

3. **Data volume at /app/backend/data** — persists models and chat history.

4. **WEBUI_SECRET_KEY required** — set a secret key for session security.

## Fixed Dockerfile

```dockerfile
FROM ghcr.io/open-webui/open-webui:main
```

## Fixed nexlayer.yaml

```yaml
application:
  name: open-webui
  pods:
    - name: app
      image: "ghcr.io/open-webui/open-webui:main"
      path: /
      servicePorts:
        - 8080
      vars:
        WEBUI_SECRET_KEY: "nexlayer-secret-2024"
      volumes:
        - name: data
          mountPath: /app/backend/data
          size: 5Gi
```
