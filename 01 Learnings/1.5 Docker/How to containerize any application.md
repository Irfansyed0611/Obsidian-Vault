As a DevOps engineer, you don't need to know how to code in all programming languages - just understand their **build patterns**. Here's a systematic approach:

## Universal Pattern Recognition (Works for ANY Stack)

### 5 Questions to Ask for Any Application:

1. **How are dependencies defined?** (package.json, requirements.txt, go.mod)
2. **How are dependencies installed?** (npm install, pip install, go mod download)
3. **Is there a build step?** (Compiled vs interpreted)
4. **How does the app start?** (node index.js, python [app.py](http://app.py/), ./binary)
5. **What port/config does it expect?** (Check docs or config files)

## **Quick Reference - Major Tech Stacks**

### **Python/Flask**

```Dockerfile
FROM python:3.11-slim

WORKDIR /app

# Dependencies file
COPY requirements.txt .
# Install command
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Start command (check their README!)
CMD ["python", "app.py"]
# or for production Flask:
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]

```

**Look for:** `requirements.txt`, `Pipfile`, `setup.py`, `pyproject.toml`

### **Go** (Compiled - needs build step!)

```Dockerfile
# Build stage
FROM golang:1.20 AS builder
WORKDIR /app
# Dependencies
COPY go.mod go.sum ./
RUN go mod download
# Build
COPY . .
RUN CGO_ENABLED=0 go build -o main .

# Run stage (tiny!)
FROM alpine:latest
RUN apk --no-cache add ca-certificates
COPY --from=builder /app/main .
CMD ["./main"]

```

**Look for:** `go.mod`, `main.go`, `Makefile`

### **Java/Spring Boot**

```Dockerfile
# Using Maven
FROM maven:3.8-openjdk-17 AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

FROM openjdk:17-jre-slim
COPY --from=builder /app/target/*.jar app.jar
CMD ["java", "-jar", "app.jar"]

```

**Look for:** `pom.xml` (Maven), `build.gradle` (Gradle)

### **PHP**

```Dockerfile
FROM php:8.1-apache
# or php:8.1-fpm for nginx

# Install PHP extensions if needed
RUN docker-php-ext-install pdo_mysql

# Copy app to Apache's directory
COPY . /var/www/html/

# If using Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
RUN composer install --no-dev --optimize-autoloader

```

**Look for:** `composer.json`, `index.php`, `.htaccess`

### **Ruby/Rails**

```Dockerfile
FROM ruby:3.1-slim

RUN apt-get update -qq && apt-get install -y nodejs postgresql-client build-essential

WORKDIR /app
# Dependencies
COPY Gemfile Gemfile.lock ./
RUN bundle install

COPY . .

# Rails specific
RUN rails assets:precompile
CMD ["rails", "server", "-b", "0.0.0.0"]

```

**Look for:** `Gemfile`, `config.ru`, `Rakefile`

## **Quick Investigation Workflow**

### **1. Check Project Structure (30 seconds)**

```bash
# See what files exist
ls -la
tree -L 2  # See structure

# Look for dependency files
find . -name "package.json" -o -name "requirements.txt" \\\\
       -o -name "go.mod" -o -name "pom.xml" \\\\
       -o -name "Gemfile" -o -name "composer.json"

```

### **2. Check README/Docs (2 minutes)**

```bash
# Usually has "Getting Started" or "Installation"
cat README.md | grep -A 10 -i "install\\\\|run\\\\|start\\\\|build"

```

### **3. Look for Existing Dockerfile**

```bash
# Often there's already one!
find . -name "Dockerfile*" -o -name "docker-compose*"
# Check CI/CD files for build commands
cat .gitlab-ci.yml .github/workflows/*.yml

```

### **4. Find the Entry Point**

```python
# Python
grep -r "if __name__" --include="*.py"
grep -r "app.run()" --include="*.py"

# Go
find . -name "main.go"

# Java
find . -path "*/main/java/*" -name "*Application.java"

# PHP
find . -name "index.php"

```

## **Cheat Sheet: Dependency Files → Docker Commands**

| Stack       | Dependency File  | Install Command                 | Common Port |
| ----------- | ---------------- | ------------------------------- | ----------- |
| Node.js     | package.json     | npm install                     | 3000, 8080  |
| Python      | requirements.txt | pip install -r requirements.txt | 5000, 8000  |
| Go          | go.mod           | go mod download                 | 8080        |
| Java/Maven  | pom.xml          | mvn install                     | 8080        |
| Java/Gradle | build.gradle     | gradle build                    | 8080        |
| PHP         | composer.json    | composer install                | 80          |
| Ruby        | Gemfile          | bundle install                  | 3000        |
| .NET        | *.csproj         | dotnet restore                  | 5000        |
| Rust        | Cargo.toml       | cargo build                     | 8000        |

## **Pro Workflow for Unknown Apps**

### **1. Use AI/ChatGPT as Assistant**

```bash
# Copy these to ChatGPT:
ls -la
cat package.json  # or equivalent dependency file
head -50 app.py   # or main file

# Ask: "Help me write a Dockerfile for this app"

```

### **2. Test Locally First**

```bash
# Try running without Docker first
python app.py
# Note what fails, what ports open, what it needs

```

### **3. Use Official Examples**

```bash
# Google: "[framework] dockerfile example"
# Go to framework's official docs
# Check their GitHub for Dockerfile examples

```

## **Learning Resources (Specific for DevOps)**

1. **"Dockerfile for X" Repos:**
    - awesome-compose (Docker official examples)
    - dockersamples on GitHub
2. **Quick Patterns Site:**
    - dockerfile.dev (patterns for each language)
3. **Build Process Guides:**
    - "12 Factor App" (understand why apps are structured certain ways)

## **My Personal Trick**

Keep a "snippets" file with working Dockerfiles:

```yaml
# docker-snippets.yaml
python-flask: |
  FROM python:3.11-slim
  ...

go-api: |
  FROM golang:1.20 AS builder
  ...

```

Then just copy-paste and modify!