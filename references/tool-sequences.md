# Container Tool Sequences (42 tools)

## Containers (17)
| Tool | Purpose | Risk |
|------|---------|------|
| `list_containers` | List (include stopped with all=true) | read |
| `inspect_container` | Full details: config, mounts, network | read |
| `run_container` | Run new container from image | write |
| `stop_container` | Graceful stop (SIGTERM) | write |
| `kill_container` | Force kill (SIGKILL) | write |
| `remove_container` | Remove stopped container | destructive |
| `restart_container` | Restart | write |
| `pause_container` | Freeze processes | write |
| `unpause_container` | Resume | write |
| `rename_container` | Rename | write |
| `get_logs` | stdout/stderr logs | read |
| `exec_container` | Execute command inside | write |
| `get_stats` | CPU, memory, network I/O | read |
| `get_top` | Running processes | read |
| `get_changes` | Filesystem diff | read |
| `wait_container` | Wait for exit code | read |
| `update_container` | Change CPU/memory limits live | write |

## Images (8)
| Tool | Purpose | Risk |
|------|---------|------|
| `list_images` | Local images with sizes | read |
| `pull_image` | Pull from registry | write |
| `remove_image` | Remove local image | destructive |
| `inspect_image` | Layers, config, OS | read |
| `tag_image` | Tag image | write |
| `image_history` | Layer history | read |
| `save_image` | Export as tar | read |
| `load_image` | Import from tar | write |

## Networks (6)
| Tool | Purpose | Risk |
|------|---------|------|
| `list_networks` | All networks | read |
| `create_network` | Create network | write |
| `remove_network` | Delete network | destructive |
| `inspect_network` | Details + connected containers | read |
| `connect_network` | Attach container | write |
| `disconnect_network` | Detach container | write |

## Volumes (4)
| Tool | Purpose | Risk |
|------|---------|------|
| `list_volumes` | All volumes | read |
| `create_volume` | Create volume | write |
| `remove_volume` | Delete volume | destructive |
| `inspect_volume` | Mountpoint details | read |

## Sequence: Troubleshoot Container (3 calls)
```
1. get_logs(container: "api", tail: 50) → find error
2. get_stats(container: "api") → check resources
3. get_top(container: "api") → check processes
```

## Sequence: Deploy New Version (4 calls)
```
1. pull_image(image: "myapp:v2.3.1")
2. stop_container(name: "myapp")
3. remove_container(name: "myapp")
4. run_container(image: "myapp:v2.3.1", name: "myapp", ports: ["8080:8080"])
```
