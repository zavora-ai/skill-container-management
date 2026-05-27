# Container Examples

## Example 1: "What containers are running?"
```
list_containers(all: false) → [{name: "api", image: "myapp:v2.3.0", status: "Up 3 days", ports: "8080"}, {name: "db", image: "postgres:16", status: "Up 3 days"}]
```
Response: "2 containers running: api (myapp:v2.3.0, 3 days) and db (postgres:16, 3 days)"

## Example 2: "The API container keeps crashing"
```
get_logs(container: "api", tail: 30) → "Error: ECONNREFUSED 127.0.0.1:5432"
get_stats(container: "api") → {memory: "45%", cpu: "2%"}
inspect_container(name: "api") → {network: "bridge", depends_on: "db"}
```
Response: "API can't connect to database (ECONNREFUSED :5432). Memory/CPU fine. Check if db container is on same network."

## Example 3: "Deploy the new version of myapp"
```
pull_image(image: "myapp:v2.3.1") → pulled ✅
stop_container(name: "myapp") → stopped
remove_container(name: "myapp") → removed
run_container(image: "myapp:v2.3.1", name: "myapp", ports: ["8080:8080"], env: ["DB_URL=..."]) → started
get_logs(container: "myapp", tail: 5) → "Server listening on :8080"
```
Response: "✅ Deployed myapp:v2.3.1. Server listening on :8080."
