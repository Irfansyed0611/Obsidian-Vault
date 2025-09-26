Here's how to handle Docker image naming, authentication, and pushing:

## Docker Image Naming Convention

The standard format is: `[registry-host]/[username]/[repository]:[tag]`

For Docker Hub specifically:
```
username/repository:tag
```

Examples:
- `myusername/myapp:latest`
- `myusername/myapp:v1.2.3`
- `myusername/myapp:production`

Best practices for naming:
- Use lowercase letters, numbers, hyphens, and underscores only
- Keep names descriptive but concise
- Use semantic versioning for tags (v1.0.0, v1.2.3)
- Always tag with specific versions, not just "latest"

## Login to Docker Hub with Personal Access Token

First, create a Personal Access Token in Docker Hub (Account Settings → Security → New Access Token).

Then login via CLI:
```bash
docker login -u your-username --password-stdin
```
When prompted, paste your PAT (not your password).

Alternatively, you can pass the token directly:
```bash
echo "your-pat-token" | docker login -u your-username --password-stdin
```

## Push Image to Docker Hub

1. **Tag your image** (if not already properly named):
```bash
docker tag local-image:tag username/repository:tag
```

2. **Push the image**:
```bash
docker push username/repository:tag
```

Complete example workflow:
```bash
# Build image
docker build -t myusername/myapp:v1.0.0 .

# Login to Docker Hub
docker login -u myusername --password-stdin

# Push image
docker push myusername/myapp:v1.0.0
```

The repository will be created automatically on Docker Hub if it doesn't exist when you push.