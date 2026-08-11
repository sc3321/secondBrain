

## 0. One‑Page Cheat‑Sheet (for frantic moments)

- **Container** = running process isolated by Linux namespaces + cgroups; shares _host_ kernel.
    
- **Image** = read‑only, layered filesystem snapshot + JSON config.  
    _Immutable_, addressed by _tag_ or _digest_ (`sha256:…`).
    
- **Multi‑stage Dockerfile** = pop‑up factory (**builder**) → shrink‑wrapped gadget (**runtime**).  
    Builder layers never leave your machine/CI; runtime layers are what you push & deploy.
    
- **Registry** = warehouse of layer blobs + manifests (Docker Hub, GHCR, ECR, etc.).
    
- **Pipeline** = `docker build` ➜ `docker push` ➜ YAML (`compose.yml`/K8s) ➜ runtime host `docker pull` ➜ container starts.
    

---

## 1. The Story: From First Line of Code to Production

### 1.1 The Pop‑Up Factory (Builder Stage)

You write a **multi‑stage Dockerfile**:

```Dockerfile
FROM node:22-alpine AS builder            # full tool‑chain
WORKDIR /app
COPY package*.json ./
RUN npm ci                                 # heavy step, cached
COPY . .
RUN npm run build                          # produce static assets → dist/
```

_Inside Docker_, a container boots with Node, npm, and Alpine. It compiles, tests, lints—whatever you ask. **Everything here is disposable**: source code, package caches, random `.o` files. Layers are cached locally for speed but _never_ sent to production.

### 1.2 The Shrink‑Wrapped Gadget (Runtime Stage)

```Dockerfile
FROM nginx:1.27-alpine AS runtime         # lean, battle‑tested
COPY --from=builder /app/dist /usr/share/nginx/html
ENTRYPOINT ["nginx","-g","daemon off;"]
```

Only the compiled `dist/` directory is copied forward. The final image is ~20 MB, contains zero compilers, and runs as non‑root. This is the artifact you **tag** (`myorg/web:1.0`) and **push**.

### 1.3 The Warehouse (Registry)

`docker push myorg/web:1.0` uploads:

1. **Manifest JSON** – which layers compose the image.
    
2. **Layer blobs** – tarballs addressed by their SHA‑256 checksums.  
    If another image re‑uses a layer the registry already stores, it’s a no‑op—deduplication by content.
    

### 1.4 The Travel Agent (YAML)

A `docker‑compose.yml` or Kubernetes Deployment is simply the travel itinerary:

```yaml
services:
  web:
    image: myorg/web:1.0         # or immutable digest
    ports: ["80:80"]
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
```

At `docker compose up` the runtime host pulls missing layers, verifies checksums, grafts read‑only layers with a tiny write layer, and launches the container.

### 1.5 The Destination (Runtime Host)

On **Linux**, containers share the host’s kernel—no VM needed.  
On **macOS/Windows**, Docker Desktop silently runs a lightweight Linux VM to provide that kernel. Either way, the kernel is _outside_ the image.

---

## 2. Core Concepts & Definitions

|Term|What it really means|Common gotchas|
|---|---|---|
|**Layer**|Compressed tarball produced by each `RUN`, `COPY`, `ADD`.|Order matters for cache; identical layers are reused across images.|
|**Digest**|SHA‑256 hash of entire image config + layer graph.|Immutable; safest way to pin prod deploys.|
|**BuildKit**|Modern builder backend (default since Docker 23).|Parallel stage builds, secrets, cache mounts—enable it!|
|**OCI**|Open Container Initiative spec that defines what _all_ compliant runtimes understand.|Docker is one implementation; Podman/containerd/CRIO are others.|

---

## 3. Why Multi‑Stage? Five Engineering Principles

1. **Reproducibility** – Whole build tool‑chain lives inside the builder container; host OS is irrelevant.
    
2. **Immutability** – Runtime image is content‑addressed; never mutate—rebuild.
    
3. **Least privilege** – No compilers or dev packages in prod; run as non‑root.
    
4. **Small attack surface** – Fewer CVEs; faster network pulls; cheaper storage.
    
5. **Single source of truth** – Dockerfile encodes both _how to build_ and _how to run_; no drift between shell scripts and prod.
    

---

## 4. Build ➜ Push ➜ Pull ➜ Run (Step‑by‑Step)

|Step|Command|What’s happening under the hood|
|---|---|---|
|**Build**|`docker build -t myorg/api:1.0 .`|Builder container runs instructions; final stage layers get tag.|
|**Push**|`docker push myorg/api:1.0`|Client uploads manifest + missing blobs to registry.|
|**Reference**|YAML `image: myorg/api:1.0`|Digest/tag tells runtime where to find layers.|
|**Pull**|Automatic|Host asks registry for manifest → downloads any missing layers.|
|**Run**|`docker compose up` / K8s|Engine grafts layers, starts isolated processes sharing kernel.|

---

## 5. Best‑Practice Checklist for Production Images

---

## 6. Common Pitfalls & Fixes

|Symptom|Root cause|Fix|
|---|---|---|
|1 GB image|Compilers, caches copied into runtime|Use multi‑stage; COPY only artifacts.|
|“Works on my machine” networking|Relying on localhost vs container net|Use `EXPOSE`, proper compose networks, and service names.|
|Secret leaked in image history|`RUN echo $SECRET > config.yml`|Pass at runtime; never write secrets during build.|
|Permission errors writing files|Running as root in image but not on host|Create matching UID/GID user inside image.|

---

## 7. FAQ (Lightning Round)

**Q:** _Do end‑users need Docker to access my web app?_  
**A:** No—only the server that _runs_ your container needs it. Browsers just speak HTTP.

**Q:** _Where is the Linux kernel in a container image?_  
**A:** Nowhere; the host supplies it. On macOS/Windows Docker Desktop’s hidden VM supplies it.

**Q:** _Can I debug into a running container?_  
**A:** Yes: `docker exec -it <id> sh`. Just remember: mutate runtime state, not the image; rebuild instead.

---

## 8. Glossary

- **Builder stage** – temporary container packed with tools, lives only during `docker build`.
    
- **Runtime stage** – final, slim image that goes to the registry.
    
- **Registry** – artifact warehouse that stores layer blobs and manifests via the OCI API.
    
- **Compose** – Docker’s local orchestrator; YAML‑driven.
    
- **Kubernetes** – production‑grade orchestrator; declarative manifests plus controller loop.
    
- **Distroless** – Google‑maintained minimal base images containing only runtime libraries.
    

---

### Closing Analogy

> _Docker is LEGO® for software:_  
> build complex models by snapping **layers** together, pack the finished set into a box (**image**), store it in the toy shop (**registry**), and let anyone in the world rebuild exactly the same model on their living‑room floor (**runtime host**)—without shipping your entire workbench or instruction scribbles.

Happy shipping! 🚢