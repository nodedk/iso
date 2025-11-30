#!/usr/bin/env bash
set -euo pipefail

APP_DIR="/srv/app"
NEW_DIR="${APP_DIR}-$(date +%s)"
NEW_PORT="8082"
CADDY_API="http://127.0.0.1:2019"

# 1. Sync new build
rsync -az ./build/ "$NEW_DIR/"

# 2. Start new instance
"$NEW_DIR/app" --port "$NEW_PORT" &
NEW_PID=$!

# 3. Wait for readiness
until curl -fs "http://127.0.0.1:$NEW_PORT/health" >/dev/null; do
    sleep 0.2
done

# 4. Update Caddy config
curl -s "$CADDY_API/config/" \
  -X PATCH \
  -H "Content-Type: application/json" \
  --data "{\"apps\":{\"http\":{\"servers\":{\"srv0\":{\"routes\":[{\"handle\":[{\"handler\":\"reverse_proxy\",\"upstreams\":[{\"dial\":\"127.0.0.1:$NEW_PORT\"}]}]}]}}}}}"

# 5. Graceful switch done; after some time, stop old instance
sleep 5
pkill -f "$APP_DIR/app" || true

# 6. Promote new dir
rm -rf "$APP_DIR"
mv "$NEW_DIR" "$APP_DIR"
