# node1 heartbeat

External **dead-man's switch** for the `node1` homelab box.

`node1` runs a systemd timer (`node1-heartbeat.timer`, every 15 min) that writes a UTC
timestamp into `heartbeat.txt` on the `heartbeat` branch. The workflow in
`.github/workflows/node1-heartbeat.yml` runs in the cloud every 15 min and **fails if that
timestamp is older than 2400 s (40 min)** — meaning node1 is down or offline.

The check lives in GitHub Actions, not on node1 and not on the desktop, because a watchdog
running on either machine cannot report that machine being gone. GitHub emails the workflow
cron author on failure.

The `heartbeat` branch holds a single amended commit that is force-pushed each beat, so the
repo never accumulates history. It contains only a UTC timestamp.
