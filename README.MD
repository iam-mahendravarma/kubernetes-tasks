# Day-01 – Kubernetes Learning

## Topics Covered
- Sidecar Containers
- emptyDir Volume

---

## Sidecar Container

### What is a Sidecar Container?

A **sidecar container** is used to extend or enhance the functionality of the main application container without modifying the application code.  
It runs in the **same Pod**, sharing the **same lifecycle and network namespace**.

### Why do we use Sidecar Containers?
- Logging
- Monitoring
- Security
- Proxies
- Configuration reload

This helps in maintaining **separation of concerns** and keeps the main application lightweight.

### Example
- The **main container** runs an **NGINX** server.
- The **sidecar container** reads:
  - NGINX access logs
  - NGINX error logs
- The sidecar container can then forward these logs to systems like:
  - ELK Stack
  - Prometheus
  - Cloud logging services

This is done **without changing the NGINX application code**.

---

## emptyDir Volume

### What is emptyDir?

`emptyDir` is a **temporary volume** that is created when a Pod starts and deleted when the Pod is removed or restarted.

It allows **data sharing between containers inside the same Pod**.

### Why do we use emptyDir?
- Share files between containers
- Temporary storage
- Caching
- Log sharing

### Example
- NGINX writes logs to a directory
- The directory is backed by an `emptyDir` volume
- A sidecar container reads the logs from the same directory

### Disadvantages
- Data is **not persistent**
- If the Pod fails or is deleted, all data in `emptyDir` is lost
- Not suitable for long-term storage

---

## Summary
- **Sidecar containers** are used to extend application functionality without modifying the app.
- **emptyDir** is used for temporary, Pod-level shared storage.
- Both are commonly used together for logging, monitoring, and helper processes.
