# Docker BuildKit Cache - Complete Notes

## Overview

- **BuildKit** uses cache to speed up builds significantly
- Best demonstrated by building an image on clean Docker host, then repeating immediately
    - First build: Pulls images and builds all layers (slow)
    - Second build: Instantly completes using cached layers (fast)

## Cache Scope

### Local Builder

- Cache only available to builds on the **same system**
- Limited to individual developer's machine

### Docker Build Cloud

- **Entire team can share cache**
- Improves build times across organization

## How Docker Cache Works

### Build Process

1. Builder iterates through Dockerfile **line by line**
2. Starts from the **top** of Dockerfile
3. For each line, checks if layer exists in cache
4. **Cache Hit**: Uses cached layer
5. **Cache Miss**: Builds new layer from instruction

> Key Insight: Cache hits are one of the best ways to make builds faster

## Detailed Cache Mechanism

### Example Dockerfile

```docker
FROM alpine
RUN apk add --update nodejs npm
COPY . /src
WORKDIR /src
RUN npm install
EXPOSE 8080
ENTRYPOINT ["node", "./app.js"]

```

### Step-by-Step Cache Process

### Step 1: Base Image (FROM alpine)

- Checks for local copy of `alpine:latest`
- **If exists**: Moves to next instruction
- **If missing**: Pulls from Docker Hub

### Step 2: Package Installation (RUN apk...)

- Docker checks cache for layer built from:
    - Same base image (`alpine:latest`)
    - Same instruction (`RUN apk add --update nodejs npm`)
- **Cache Hit**: Links to cached layer, continues with cache intact
- **Cache Miss**: Invalidates cache, builds layer

### Step 3: Copy Files (COPY . /src)

- Assumes previous layer (RUN) had cache hit with ID "AAA"
- Checks for cached layer built by running `COPY . /src` on layer AAA
- **Cache Hit**: Links to layer, proceeds to next instruction
- **Cache Miss**: Builds layer, invalidates remaining cache

### Step 4: Subsequent Instructions

- Process continues for remaining Dockerfile instructions
- Once cache is invalidated, all remaining instructions must be built

## Critical Cache Rules

### 1. Cache Invalidation

- **Once invalidated, cache is NOT checked for rest of build**
- All subsequent instructions must be executed in full
- Cannot recover cache within same build

### 2. Dockerfile Optimization Strategy

```docker
# Place stable instructions first (rarely change)
FROM alpine
RUN apk add --update nodejs npm

# Place volatile instructions last (frequently change)
COPY . /src
RUN npm install

```

**Best Practice**: Write Dockerfiles so instructions most likely to invalidate cache go **near the end**

### 3. Force Cache Bypass

```bash
docker build --no-cache .

```

- Ignores all cached layers
- Useful for troubleshooting or ensuring fresh build

## COPY and ADD Special Behavior

### Checksum Validation

- **COPY** and **ADD** instructions include special logic
- Ensures copied content hasn't changed since last build

### How It Works

1. Docker performs **checksums** on each file being copied
2. Compares checksums with cached layer's files
3. If files changed → **Cache miss** (even if instruction is identical)
4. Protects against using outdated file versions

### Example Scenario

```docker
COPY . /src  # Same instruction, but files changed

```

- Even though instruction is identical
- If source files changed, cached layer is **NOT** used
- Ensures image always has latest file versions

## Cache Optimization Tips

### 1. Layer Order Strategy

```docker
# ✅ GOOD: Dependencies before code
FROM node:18
WORKDIR /app
COPY package*.json ./    # Changes rarely
RUN npm install          # Cached unless package.json changes
COPY . .                 # Changes frequently

# ❌ BAD: Code before dependencies
FROM node:18
WORKDIR /app
COPY . .                 # Any file change invalidates
RUN npm install          # Rebuilds every time

```

### 2. Separate Concerns

```docker
# System dependencies (rarely change)
RUN apt-get update && apt-get install -y curl git

# Application dependencies (occasionally change)
COPY requirements.txt .
RUN pip install -r requirements.txt

# Application code (frequently changes)
COPY src/ ./src/

```

### 3. Multi-Stage Builds for Better Caching

```docker
# Build stage - cached independently
FROM node:18 AS builder
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Runtime stage - reuses builder cache
FROM node:18-slim
COPY --from=builder /app/dist ./dist

```

## Common Cache Pitfalls

### 1. Dynamic Content in RUN Commands

```docker
# ❌ Invalidates cache every time
RUN echo "Built at: $(date)" > build.txt

# ✅ Use build args if needed
ARG BUILD_DATE
RUN echo "Built at: ${BUILD_DATE}" > build.txt

```

### 2. Large Build Context

```bash
# ❌ Sends unnecessary files
docker build .

# ✅ Use .dockerignore
echo "node_modules" >> .dockerignore
echo ".git" >> .dockerignore

```

### 3. Unnecessary Updates in RUN Commands

```docker
# ❌ May get different package versions
RUN apt-get update && apt-get install -y curl

# ✅ Pin versions for consistency
RUN apt-get update && apt-get install -y \\\\
    curl=7.81.0-1ubuntu1.13 \\\\
    && rm -rf /var/lib/apt/lists/*

```

## Key Takeaways

1. **Cache is sequential** - Invalid cache cannot be recovered in same build
2. **Order matters** - Place stable layers first
3. **File changes matter** - COPY/ADD check file contents, not just instructions
4. **Cache sharing** - Use Docker Build Cloud for team-wide benefits
5. **Optimization impact** - Can reduce build times from minutes to seconds