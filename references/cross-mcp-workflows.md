# Container Cross-MCP Workflows

## Containers + Observability: OOM Investigation
```
OBS: list_alerts(service: "api") → "container OOM killed"
CONTAINERS: get_stats(container: "api") → memory: 98%
CONTAINERS: get_logs(container: "api", tail: 20) → "Killed process"
CONTAINERS: update_container(id, memory_limit: "2g")
CONTAINERS: restart_container(id)
SLACK: send_message(channel: "#ops", text: "✅ api OOM fixed — memory increased to 2GB")
```

## Containers + CI/CD: Rolling Deploy
```
CONTAINERS: pull_image(image: "myapp:v2.3.1")
CONTAINERS: stop_container(name: "myapp")
CONTAINERS: remove_container(name: "myapp")
CONTAINERS: run_container(image: "myapp:v2.3.1", name: "myapp", ports: ["8080:8080"])
CONTAINERS: get_logs(container: "myapp", tail: 5) → verify startup
OBS: get_errors(service: "myapp", last: "2min") → no errors ✅
```

## Containers + Database: DB Container Health
```
CONTAINERS: get_stats(container: "postgres") → {cpu: 85%, memory: 70%}
CONTAINERS: exec_container(id: "postgres", command: "pg_isready") → "accepting connections"
DB: get_slow_queries() → check if high CPU correlates with queries
```
