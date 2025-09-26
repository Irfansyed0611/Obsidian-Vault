## What is Dockerfile.dev?

`Dockerfile.dev` is a development-specific Dockerfile used alongside your main `Dockerfile`. It's a naming convention to distinguish between production and development container configurations.

## Key Differences

**Dockerfile (Production)**:
- Optimized for size and security
- Multi-stage builds to exclude dev dependencies
- Minimal base images (alpine, distroless)
- No development tools
- Hardened security settings

**Dockerfile.dev (Development)**:
- Includes development tools and dependencies
- Hot reloading capabilities
- Debug tools and utilities
- More verbose logging
- Volume mounts for live code changes

## Example Comparison

**Dockerfile (Production)**:
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["npm", "start"]
```

**Dockerfile.dev (Development)**:
```dockerfile
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install  # Includes dev dependencies
COPY . .
EXPOSE 3000
EXPOSE 9229  # Debug port
CMD ["npm", "run", "dev"]
```

## Common Use Cases

**Local Development**: Fast iteration with hot reloading and debugging tools
```bash
docker build -f Dockerfile.dev -t myapp:dev .
docker run -v $(pwd):/app -p 3000:3000 myapp:dev
```

**CI/CD Pipelines**: Different stages for testing vs deployment
```bash
# Development/testing stage
docker build -f Dockerfile.dev -t myapp:test .

# Production stage  
docker build -f Dockerfile -t myapp:prod .
```

**Team Development**: Consistent development environment across team members

## Is it Good Practice?

**Yes, it's considered a best practice** for several reasons:

**Environment Separation**: Clear distinction between development and production concerns without cluttering the main Dockerfile

**Optimized Builds**: Production images stay lean and secure while development images include necessary tooling

**Developer Experience**: Development-specific features like hot reloading, debugging tools, and verbose logging improve productivity

**Flexibility**: Different base images, dependencies, and configurations for different needs

## Alternative Approaches

You can also use:
- **Multi-stage builds** with target stages
- **Docker Compose** with override files
- **Build arguments** to conditionally include development features

But `Dockerfile.dev` remains popular because it's explicit, simple, and keeps concerns clearly separated.