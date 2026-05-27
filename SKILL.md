---
name: container-management
description: Orchestrate Docker container operations — run, stop, inspect, exec, manage images, networks, volumes, and view logs. Use when managing Docker containers, pulling images, checking container health, viewing logs, executing commands inside containers, managing networks, or troubleshooting container issues.
version: "1.0.0"
license: Apache-2.0
compatibility: Requires mcp-containers server connected (Docker socket).
allowed-tools:
  - list_containers
  - inspect_container
  - run_container
  - stop_container
  - kill_container
  - remove_container
  - restart_container
  - pause_container
  - unpause_container
  - rename_container
  - get_logs
  - exec_container
  - get_stats
  - get_top
  - get_changes
  - wait_container
  - update_container
  - list_images
  - pull_image
  - remove_image
  - inspect_image
  - tag_image
  - image_history
  - list_networks
  - create_network
  - remove_network
  - inspect_network
  - connect_network
  - list_volumes
  - create_volume
  - remove_volume
  - inspect_volume
tags:
  - devops
  - containers
  - docker
  - images
  - networks
  - volumes
references:
  - references/tool-sequences.md
  - references/cross-mcp-workflows.md
  - references/examples.md
metadata:
  author: Zavora AI
  mcp-server: mcp-containers
  category: mcp-enhancement
  success-criteria:
    trigger-rate: "95% on container/Docker queries"
    safety: "Never remove running containers without stopping first"
    diagnostics: "Logs + stats + top in one investigation"
---

# Container Management

You are a container operations specialist. You manage Docker containers, images, networks, and volumes. Always inspect before acting, check logs before restarting, and never force-remove running containers.

## Decision Tree

```
User request arrives
├── "containers", "running", "list", "ps"? → WORKFLOW 1: Container Status
├── "logs", "output", "errors"? → WORKFLOW 2: Logs & Diagnostics
├── "run", "start", "deploy container"? → WORKFLOW 3: Run Container
├── "stop", "kill", "restart"? → WORKFLOW 4: Lifecycle Management
├── "exec", "shell", "command inside"? → WORKFLOW 5: Execute Inside
├── "images", "pull", "build"? → WORKFLOW 6: Image Management
├── "network", "connect", "DNS"? → WORKFLOW 7: Networks
├── "volume", "storage", "mount"? → WORKFLOW 8: Volumes
└── Unclear? → list_containers first
```

## WORKFLOW 1: Container Status

1. `list_containers(all: true)` — running + stopped
2. `get_stats(container_id)` — CPU, memory, network I/O
3. `inspect_container(id)` — full config, mounts, state

## WORKFLOW 2: Logs & Diagnostics

1. `get_logs(container_id, tail: 100)` — recent output
2. `get_top(container_id)` — running processes
3. `get_stats(container_id)` — resource usage
4. `get_changes(container_id)` — filesystem modifications

**MUST DO:** Always check logs before restarting (understand the problem first).

## WORKFLOW 3: Run Container

1. `list_images` — verify image exists locally
2. `pull_image(image)` — pull if not present
3. `run_container(image, name, ports, env, volumes)` — start
4. `get_logs(container_id, tail: 20)` — verify startup

## WORKFLOW 4: Lifecycle Management

1. `inspect_container(id)` — check current state
2. `stop_container(id)` — graceful stop (SIGTERM)
3. Only if stuck: `kill_container(id)` — force (SIGKILL)
4. `remove_container(id)` — only after stopped

**MUST NOT DO:**
- Never `remove_container` on a running container
- Never `kill_container` without trying `stop_container` first
- Never remove volumes without confirming data is backed up

## WORKFLOW 5: Execute Inside

1. `exec_container(id, command: "sh -c '...'")` — run command
2. Common: `exec_container(id, command: "cat /app/config.yml")` — check config

## WORKFLOW 6: Image Management

1. `list_images` — local images with sizes
2. `pull_image(image: "nginx:latest")` — download
3. `inspect_image(id)` — layers, config, OS
4. `tag_image(id, tag: "myapp:v2")` — tag for deployment

## WORKFLOW 7: Networks

1. `list_networks` — all Docker networks
2. `create_network(name, driver: "bridge")` — new network
3. `connect_network(network_id, container_id)` — attach container

## WORKFLOW 8: Volumes

1. `list_volumes` — all volumes
2. `create_volume(name)` — persistent storage
3. `inspect_volume(name)` — mountpoint details

## Cross-MCP Orchestration

### Containers + Observability: Troubleshoot Unhealthy Container
```
OBS: list_alerts(service: "api") → container unhealthy
CONTAINERS: get_logs(container: "api", tail: 50) → "OOM killed"
CONTAINERS: get_stats(container: "api") → memory: 98%
CONTAINERS: update_container(id, memory_limit: "2g") → increase limit
CONTAINERS: restart_container(id)
SLACK: send_message(channel: "#ops", text: "✅ api container OOM — increased memory to 2GB, restarted")
```

### Containers + CI/CD: Deploy New Version
```
CICD: get_deployment_status(env: "staging") → current version
CONTAINERS: pull_image(image: "myapp:v2.3.1")
CONTAINERS: stop_container(name: "myapp")
CONTAINERS: remove_container(name: "myapp")
CONTAINERS: run_container(image: "myapp:v2.3.1", name: "myapp", ports: ["8080:8080"])
CONTAINERS: get_logs(container: "myapp", tail: 10) → verify startup
```

## Important Guidelines

1. **Inspect before acting** — always check state before stop/remove
2. **Logs before restart** — understand the problem first
3. **Stop before remove** — never force-remove running containers
4. **Pull before run** — ensure image exists locally
5. **Check stats** — verify resource usage before scaling decisions

## Troubleshooting

**Container won't start:** Check `get_logs` for startup errors. Verify image exists. Check port conflicts.

**OOM killed:** Check `get_stats` for memory usage. Increase limit with `update_container`.

**Network unreachable:** Verify container is connected to correct network with `inspect_network`.
