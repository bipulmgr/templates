# Docker Compose Templates

A curated collection of Docker Compose template files with automated index generation for our application.

## 📁 Repository Structure

```
/templates/
   employee-app-starter-pack.yml
   ...
index.json (auto-generated)
```
## 📋 Custom Template Schema

Our templates extend the standard Docker Compose format with additional metadata fields that our application requires:

### 🔹 Main Template Metadata

```yaml
version: "3.9"

# 🔹 Main group metadata (REQUIRED for our application)
name: "Template Name"                    # Display name for the template
description: "Short description"         # Brief description of what this template does
techstacks:                              # Array of technology objects with icons
  - name: "Technology Name"
    iconUrl: "https://cdn.simpleicons.org/technology"
  - name: "Another Technology"
    iconUrl: "https://cdn.simpleicons.org/another-tech"
iconUrl: "https://example.com/template-icon.png"  # Main template icon
sharedVariable:                          # Environment variables shared across services
  ENVIRONMENT: "development"
  TIMEZONE: "UTC"
```

### 🔹 Service Configuration

Each service in our templates includes additional fields beyond standard Docker Compose:

```yaml
services:
  service-name:
    # Standard Docker Compose fields
    image: image:tag
    ports:
      - "3000:3000"
    volumes:
      - data:/app/data
    environment:
      NODE_ENV: "development"
    
    # 🔹 Custom fields for our application (REQUIRED)
    type: Application|Database|Cache|Gateway  # Service type classification
    order: 1                                  # Display order in UI
    serviceUrl: "https://example.com/service.zip"  # Download URL for service code
    iconUrl: "https://example.com/service-icon.png"  # Service-specific icon
    environment:                              # Environment variables with dynamic references
      API_URL: ${domainUrl_other-service}    # Reference other service domains
      DB_HOST: ${host_database-service}      # Reference container hostnames
      DATABASE_URL: ${connectionString_db}   # Reference connection strings
      DB_USER: ${env_database_POSTGRES_USER} # Reference specific env vars from other services
    buildConfig:                              # Build configuration details
      packages:                               # Package dependencies
        - "package-name"
        - "another-package"
      aptPackages:                           # System packages
        - "curl"
        - "git"
      installCommand: "npm install"          # Installation command
      startCommand: "npm start"              # Start command
      ports:                                 # Port configuration
        - 3000
```

## 🔄 Automated Index Generation

The `index.json` file is **automatically generated** and updated via GitHub Actions:

- **Trigger**: Every push to `main` branch that modifies files in `/templates/*.yml`
- **Process**: Parses all YAML templates and extracts metadata
- **Output**: Updates `index.json` with template information
- **No Manual Work**: You never need to edit `index.json` manually!

### Index.json Structure

```json
[
  {
    "name": "Template Name",
    "techstacks": [
      {
        "name": "Technology Name",
        "iconUrl": "https://cdn.simpleicons.org/technology"
      }
    ],
    "description": "Short description",
    "iconUrl": "https://example.com/template-icon.png",
    "services": ["service1", "service2", "service3"],
    "filename": "template-name.yml"
  }
]
```

## 🛠️ Creating New Templates

### Step 1: Add Your Template File

1. **Create a new `.yml` file** in the `/templates/` directory
2. **Follow our custom schema** (see above)
3. **Include all required fields** for our application
4. **Commit and push** to the repository

### Step 2: Automatic Processing

- **GitHub Actions will automatically detect** your new template
- **Parse the metadata** and extract information
- **Update `index.json`** with your template details
- **No manual intervention required!**

### Example Template Structure

```yaml
version: "3.9"

name: "My Custom Stack"
description: "A custom application stack with frontend, backend, and database."
techstacks:
  - name: "React"
    iconUrl: "https://cdn.simpleicons.org/react"
  - name: "Node.js"
    iconUrl: "https://cdn.simpleicons.org/nodedotjs"
  - name: "PostgreSQL"
    iconUrl: "https://cdn.simpleicons.org/postgresql"
iconUrl: "https://example.com/my-stack-icon.png"
sharedVariable:
  NODE_ENV: "development"
  TIMEZONE: "UTC"

services:
  frontend:
    type: Application
    order: 1
    serviceUrl: "https://example.com/frontend.zip"
    iconUrl: "https://example.com/react-icon.png"
    image: node:18-alpine
    ports:
      - "3000:3000"
    environment:
      API_URL: ${domainUrl_backend-api}  # References backend domain
      APP_URL: ${domainUrl_frontend}     # References frontend domain
    buildConfig:
      packages:
        - "react"
        - "vite"
      aptPackages:
        - "curl"
      installCommand: "npm install"
      startCommand: "npm run dev"
      ports:
        - 3000

  backend-api:
    type: Application
    order: 2
    serviceUrl: "https://example.com/backend.zip"
    iconUrl: "https://example.com/nodejs-icon.png"
    image: node:18-alpine
    ports:
      - "5000:5000"
    environment:
      DATABASE_URL: ${connectionString_production-db}  # Complete DB connection string
      DB_HOST: ${host_production-db}                   # DB container hostname
      DB_USER: ${env_production-db_POSTGRES_USER}      # DB username from DB service
      DB_PASSWORD: ${env_production-db_POSTGRES_PASSWORD}  # DB password from DB service
      API_URL: ${domainUrl_backend-api}                # Backend domain
      APP_URL: ${domainUrl_frontend}                   # Frontend domain
    buildConfig:
      packages:
        - "express"
        - "cors"
      aptPackages:
        - "curl"
      installCommand: "npm install"
      startCommand: "npm start"
      ports:
        - 5000

  production-db:
    type: Database
    order: 3
    serviceUrl: "https://example.com/database-setup.zip"
    iconUrl: "https://example.com/postgresql-icon.png"
    image: postgres:15-alpine
    ports:
      - "5432:5432"
    volumes:
      - db-data:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: secretpassword
      POSTGRES_DB: myapp
      POSTGRES_HOST: ${host_production-db}  # Container hostname
```

## 🔧 Key Differences from Standard Docker Compose

Our templates include these **additional fields** that standard Docker Compose doesn't have:

### Template Level Extensions:
- `name` - Display name for our application UI
- `description` - User-friendly description
- `techstacks` - Technology stack with icons for UI display
- `iconUrl` - Template icon for our application
- `sharedVariable` - Shared environment variables across services

### Service Level Extensions:
- `type` - Service classification for our application logic
- `order` - UI display order
- `serviceUrl` - Download link for service source code
- `iconUrl` - Service-specific icon for UI
- `buildConfig` - Detailed build configuration for our application

## 🌐 Environment Variable Setup

Our templates use a special environment variable syntax that allows services to reference each other dynamically. This system automatically resolves service dependencies and provides the correct connection information.

### Environment Variable Patterns

#### 1. Domain URL Reference
```yaml
${domainUrl_SERVICENAME}
```
- **Purpose**: References the domain URL of a specific service
- **Example**: `${domainUrl_employee-info-api}` → `https://api.example.com`
- **Usage**: Used when services need to communicate via HTTP/HTTPS

#### 2. Host Reference
```yaml
${host_SERVICENAME}
```
- **Purpose**: References the container hostname of a specific service
- **Example**: `${host_production-db}` → `production-db`
- **Usage**: Used for internal container-to-container communication

#### 3. Database Connection String
```yaml
${connectionString_SERVICENAME}
```
- **Purpose**: References the complete database connection string
- **Example**: `${connectionString_production-db}` → `postgresql://user:password@host:5432/database`
- **Usage**: Used when services need to connect to databases

#### 4. Service Environment Variable Reference
```yaml
${env_SERVICENAME_ENV_VARIABLE_NAME}
```
- **Purpose**: References a specific environment variable from another service
- **Example**: `${env_production-db_POSTGRES_USER}` → `postgres`
- **Usage**: Used to access specific configuration values from other services

### Real-World Example

Here's how these patterns work in practice:

```yaml
services:
  frontend:
    environment:
      API_URL: ${domainUrl_backend-api}  # Points to backend's domain
      APP_URL: ${domainUrl_frontend}     # Points to frontend's domain

  backend-api:
    environment:
      DATABASE_URL: ${connectionString_production-db}  # Complete DB connection
      DB_HOST: ${host_production-db}                   # DB container hostname
      DB_USER: ${env_production-db_POSTGRES_USER}      # DB username from DB service
      DB_PASSWORD: ${env_production-db_POSTGRES_PASSWORD}  # DB password from DB service

  production-db:
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: secretpassword
      POSTGRES_DB: myapp
```

### Variable Resolution Process

1. **Service Discovery**: The system identifies all services and their configurations
2. **Dependency Mapping**: Creates a map of service relationships
3. **Variable Resolution**: Replaces environment variable patterns with actual values
4. **Container Deployment**: Deploys containers with resolved environment variables

### Best Practices

- **Use descriptive service names**: Make service names clear and consistent
- **Group related services**: Use consistent naming patterns (e.g., `api`, `api-db`, `api-cache`)
- **Document dependencies**: Clearly indicate service dependencies in your template
- **Test locally**: Verify environment variable resolution in your development environment

## 📊 Available Templates

Check `index.json` for the complete list of available templates with their metadata. The file is automatically updated whenever templates are added or modified.
