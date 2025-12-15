
## 1️⃣ What the volumes contain

| Volume             | What it stores                                                   |
| ------------------ | ---------------------------------------------------------------- |
| `n8n_storage`      | Workflows, credentials, execution data (`.n8n`)                  |
| `postgres_storage` | Postgres database (all n8n DB data)                              |
| `ollama_storage`   | Ollama models you pulled (`llama3.2`, `mxbai-embed-large`, etc.) |
| `qdrant_storage`   | Qdrant vector database storage                                   |

---

## 2️⃣ Steps to transfer volumes to a new system


1. On the old system, export the volume:

```bash
docker run --rm -v n8n_storage:/volume -v $(pwd):/backup alpine tar czf /backup/n8n_storage.tar.gz -C /volume ./
docker run --rm -v postgres_storage:/volume -v $(pwd):/backup alpine tar czf /backup/postgres_storage.tar.gz -C /volume ./
docker run --rm -v ollama_storage:/volume -v $(pwd):/backup alpine tar czf /backup/ollama_storage.tar.gz -C /volume ./
docker run --rm -v qdrant_storage:/volume -v $(pwd):/backup alpine tar czf /backup/qdrant_storage.tar.gz -C /volume ./
```

2. Copy the `.tar.gz` files to the new system.  

3. On the new system, create the volumes:

```bash
docker volume create n8n_storage
docker volume create postgres_storage
docker volume create ollama_storage
docker volume create qdrant_storage
```

4. Restore the data:

```bash
docker run --rm -v n8n_storage:/volume -v $(pwd):/backup alpine sh -c "cd /volume && tar xzf /backup/n8n_storage.tar.gz"
docker run --rm -v postgres_storage:/volume -v $(pwd):/backup alpine sh -c "cd /volume && tar xzf /backup/postgres_storage.tar.gz"
docker run --rm -v ollama_storage:/volume -v $(pwd):/backup alpine sh -c "cd /volume && tar xzf /backup/ollama_storage.tar.gz"
docker run --rm -v qdrant_storage:/volume -v $(pwd):/backup alpine sh -c "cd /volume && tar xzf /backup/qdrant_storage.tar.gz"
```

5. Start your stack on the new system with `docker compose up -d`.  
