
## **Option 1: Build + Serve with Nginx (Most Common)**

```Dockerfile
# Build stage - Generate static files
FROM node:18-alpine AS builder

WORKDIR /app

# Cache dependencies (rebuilds only when package.json changes)
COPY package*.json ./
RUN npm ci --only=production

# Copy source and build
COPY . .
RUN npm run build

# Serve stage - Nginx serving static files
FROM nginx:alpine

# Copy built files from builder stage
COPY --from=builder /app/public /usr/share/nginx/html

# Optional: Custom nginx config for SPA routing
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

```

### **nginx.conf** (for client-side routing)

```nginx.conf
server {
    listen 80;
    server_name localhost;
    root /usr/share/nginx/html;
    index index.html;

    # Gatsby specific - handle client-side routing
    location / {
        try_files $uri $uri/ /index.html;
    }

    # Cache static assets
    location ~* \\.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}

```

**Note**: By default the nginx listens on port 80. So if you don’t change custom port in `nginx.conf` then you should map the port using `docker run -d -p 8000:80 —name <container-name> <image-name>`

### **Run locally:**

```bash
# Build
docker build -t my-portfolio .

# Run
docker run -p 8080:80 my-portfolio

# Visit <http://localhost:8080>

```

## **Option 2: Build-Only Container (For CI/CD to S3)**

```docker
FROM node:18-alpine AS builder

WORKDIR /app

# Install AWS CLI for S3 sync
RUN apk add --no-cache aws-cli

# Cache dependencies
COPY package*.json ./
RUN npm ci --only=production

# Build
COPY . .
RUN npm run build

# Option A: Output files (extract with docker cp)
# Stops here - use docker cp to get files

# Option B: Direct S3 deploy from container
# ENTRYPOINT ["sh", "-c", "aws s3 sync public/ s3://$S3_BUCKET --delete"]

```

### **Extract files for S3:**

```bash
# Build
docker build -t gatsby-builder .

# Create container
docker create --name temp gatsby-builder

# Extract public folder
docker cp temp:/app/public ./public-dist

# Sync to S3
aws s3 sync ./public-dist s3://your-bucket --delete

# Cleanup
docker rm temp
```

## **Option 3: Dev + Build + Deploy (All-in-One)**

```Dockerfile
# Development + Build + Deploy container
FROM node:18-alpine

WORKDIR /app

# Install AWS CLI
RUN apk add --no-cache aws-cli

# Dependencies
COPY package*.json ./
RUN npm ci

COPY . .

# Multiple purposes via different commands
# Development: docker run -p 8000:8000 portfolio npm run develop
# Build: docker run portfolio npm run build
# Deploy: docker run -e AWS_ACCESS_KEY_ID=xxx portfolio sh deploy.sh

```

### [**deploy.sh**](http://deploy.sh/)

```bash
#!/bin/sh
npm run build
aws s3 sync public/ s3://${S3_BUCKET} --delete
aws cloudfront create-invalidation --distribution-id ${CF_DIST_ID} --paths "/*"

```

## **Docker Compose for Development**

```yaml
version: '3.8'

services:
  # Development server
  dev:
    image: node:18-alpine
    working_dir: /app
    volumes:
      - .:/app
      - /app/node_modules  # Don't overwrite node_modules
    ports:
      - "8000:8000"
    command: sh -c "npm install && npm run develop"
    environment:
      - GATSBY_TELEMETRY_DISABLED=1

  # Production build test
  prod:
    build: .
    ports:
      - "8080:80"

```

## **GitHub Actions with Docker (CI/CD)**

```yaml
name: Deploy Portfolio

on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Build with Docker
        run: |
          docker build -t portfolio-builder .
          docker create --name extract portfolio-builder
          docker cp extract:/app/public ./public
          docker rm extract

      - name: Deploy to S3
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        run: |
          aws s3 sync ./public s3://${{ secrets.S3_BUCKET }} --delete
          aws cloudfront create-invalidation --distribution-id ${{ secrets.CF_DIST_ID }} --paths "/*"

```

## **Optimizations for Gatsby**

### **1. Cache Gatsby's .cache directory**

```Dockerfile
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

# Cache Gatsby cache between builds (if using BuildKit)
RUN --mount=type=cache,target=/app/.cache \\
    --mount=type=cache,target=/app/public \\
    npm run build

```

### **2. Smaller final image**

```Dockerfile
# Use distroless or busybox for even smaller size
FROM busybox:1.35
COPY --from=builder /app/public /www
EXPOSE 80
CMD ["httpd", "-f", "-v", "-p", "80", "-h", "/www"]
```

## **Quick Decision Guide**

- **Local testing only?** → Use Option 1 (Nginx)
- **Still want S3 deployment?** → Use Option 2 (Build-only)
- **Want flexibility?** → Use Option 3 (All-in-one)
- **Already have CI/CD?** → Just add Docker build step

## **Common Issues with Gatsby + Docker**

1. **Large image size** → Use multi-stage build
2. **Slow builds** → Mount cache directories
3. **Memory issues** → Add `NODE_OPTIONS=--max-old-space-size=4096`
4. **Sharp/image processing errors** → Use `node:18` (full) instead of alpine

**Testing your setup:**

```bash
# Build and run
docker build -t portfolio . && docker run -p 8080:80 portfolio

# Check image size
docker images portfolio

# Test the site
curl <http://localhost:8080>

```

---

## **When You DON'T Need Custom nginx.conf in option 1 (70% of cases)**

### **Simple static sites without client-side routing:**

```docker
FROM nginx:alpine
# Default nginx config works fine!
COPY --from=builder /app/public /usr/share/nginx/html
# That's it - no custom config needed

```

This works perfectly for:

- Basic HTML/CSS/JS sites
- Gatsby sites with **only static pages** (no client-side routing)
- Portfolio sites with simple navigation
- Documentation sites

## **When You NEED Custom nginx.conf (30% of cases)**

### **1. Client-Side Routing (SPA behavior)**

If your Gatsby site uses:

```jsx
// gatsby-node.js - Creating client-only routes
exports.onCreatePage = ({ page, actions }) => {
  if (page.path.match(/^\\/app/)) {
    page.matchPath = "/app/*"  // Client-side routing
  }
}

// Or using @reach/router or react-router
<Router>
  <Dashboard path="/app/dashboard" />
  <Profile path="/app/profile/:id" />
</Router>

```

**Without the config:** Visiting `/app/dashboard` directly = 404 error

**With the config:** nginx sends all routes to index.html, React Router takes over

### **2. Custom Caching Strategy**

Default nginx doesn't optimize caching. The config adds:

```
# This tells browsers to cache JS/CSS for 1 year
location ~* \\.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2)$ {
    expires 1y;
    add_header Cache-Control "public, immutable";
}

```

**Impact:**

- First visit: 2MB download
- Second visit: 50KB (only HTML, rest from cache)
- **Huge performance boost!**

### **3. Custom Headers/Security**

```
server {
    # ... basic config ...

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";
    add_header X-XSS-Protection "1; mode=block";

    # CORS headers
    add_header Access-Control-Allow-Origin "*";

    # Custom redirects
    location /old-blog {
        return 301 <https://new-site.com/blog>;
    }
}

```

## **How to Know If You Need It?**

### **Quick Test:**

```bash
# 1. Build and run with default nginx
docker build -t test-site .
docker run -p 8080:80 test-site

# 2. Test your routes
curl localhost:8080  # Works
curl localhost:8080/about  # Works if about.html exists
curl localhost:8080/app/dashboard  # 404? You need the config!

# 3. Check browser network tab
# Are JS/CSS files reloading on every page? You need caching config!

```

## **Real-World Examples**

### **Gatsby Blog (No config needed)**

```
/public/
  index.html
  about.html
  blog/post-1/index.html
  blog/post-2/index.html

```

Each route has an actual HTML file = nginx finds it automatically

### **Gatsby App with Auth (Config NEEDED)**

```
/public/
  index.html
  about.html
  app.html  # Single entry point for entire app section

```

Routes like `/app/dashboard`, `/app/profile` don't have HTML files = need routing config

## **Progressive Approach (Recommended)**

### **Start Simple:**

```docker
# Try without config first
FROM nginx:alpine
COPY --from=builder /app/public /usr/share/nginx/html

```

### **Add Config When Needed:**

```docker
FROM nginx:alpine
COPY --from=builder /app/public /usr/share/nginx/html
# Add this line when you hit issues
COPY nginx.conf /etc/nginx/conf.d/default.conf

```

## **Common Patterns by Framework**

| Framework | Usually Needs Config? | Why? |
| --- | --- | --- |
| **Gatsby** | Sometimes | Only if using client-side routes |
| **Create React App** | Yes | Always SPA with client routing |
| **Next.js (exported)** | No | Generates HTML for each route |
| **Vue SPA** | Yes | Client-side routing |
| **Nuxt (static)** | No | Generates HTML for each route |
| **Plain HTML** | No | Just serving files |

## **Quick Decision Tree**

```
Does your site work with `python -m http.server`?
├─ Yes → You DON'T need nginx config
└─ No → You probably need it
    └─ Do you have routes without .html files?
        ├─ Yes → You NEED the config
        └─ No → Check if you need caching optimization

```

## **Pro Tips**

1. **Test locally first:**

```bash
# Serve with simple server
cd public && python -m http.server 8000
# If everything works, you don't need config

```

1. **Start without, add if needed:**
Most tutorials include the config "just in case" - but you might not need it!
2. **For S3 hosting:**
S3 + CloudFront handles routing differently - this nginx config is only for container deployments
3. **Modern alternative:**

```docker
# Caddy server (auto-handles SPAs better)
FROM caddy:alpine
COPY --from=builder /app/public /usr/share/caddy
COPY Caddyfile /etc/caddy/Caddyfile

```

**Bottom line:** Start without the config. You'll know quickly if you need it (404 errors on routes). Most simple Gatsby sites don't need it unless you're doing fancy client-side routing!