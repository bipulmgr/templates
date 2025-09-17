# Docker Compose Templates

A curated collection of Docker Compose template files with automated index generation for our application.

## 📁 Repository Structure

```
/templates/
   fullstack-starter.yml
   microservices.yml
   react-node-mongodb.yml
   vue-nuxt-postgres.yml
   laravel-php-mysql.yml
   django-python-postgres.yml
   spring-boot-java-mysql.yml
   wordpress-php-mysql.yml
   nextjs-typescript-postgres.yml
   ...
index.json (auto-generated)
```

## 🚀 Quick Start

1. **Browse Templates**: Check the `index.json` file for available templates
2. **Use a Template**: Download and run any template:

```bash
# Example: Fullstack Starter
docker-compose -f templates/fullstack-starter.yml up

# Example: Microservices Architecture  
docker-compose -f templates/microservices.yml up

# Example: React Node.js MongoDB Stack
docker-compose -f templates/react-node-mongodb.yml up
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

### Required Fields for Our Application

#### Template Level:
- ✅ `name` - Template display name
- ✅ `description` - Brief description
- ✅ `techstacks` - Array of technology objects with `name` and `iconUrl`
- ✅ `iconUrl` - Main template icon URL
- ✅ `sharedVariable` - Shared environment variables

#### Service Level:
- ✅ `type` - Service type (Application, Database, Cache, Gateway)
- ✅ `order` - Display order in UI
- ✅ `serviceUrl` - Download URL for service code
- ✅ `iconUrl` - Service-specific icon URL
- ✅ `buildConfig` - Build configuration with packages, commands, and ports

### Example Template Structure

```yaml
version: "3.9"

name: "My Custom Stack"
description: "A custom application stack with frontend and backend."
techstacks:
  - name: "React"
    iconUrl: "https://cdn.simpleicons.org/react"
  - name: "Node.js"
    iconUrl: "https://cdn.simpleicons.org/nodedotjs"
iconUrl: "https://example.com/my-stack-icon.png"
sharedVariable:
  NODE_ENV: "development"
  API_URL: "http://localhost:5000"

services:
  frontend:
    type: Application
    order: 1
    serviceUrl: "https://example.com/frontend.zip"
    iconUrl: "https://example.com/react-icon.png"
    image: node:18-alpine
    ports:
      - "3000:3000"
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

  backend:
    type: Application
    order: 2
    serviceUrl: "https://example.com/backend.zip"
    iconUrl: "https://example.com/nodejs-icon.png"
    image: node:18-alpine
    ports:
      - "5000:5000"
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

## 📊 Available Templates

Check `index.json` for the complete list of available templates with their metadata. The file is automatically updated whenever templates are added or modified.

## 🤝 Contributing

1. **Fork the repository**
2. **Add your template** to `/templates/` directory as a `.yml` file
3. **Follow our custom schema** with all required fields
4. **Submit a pull request**
5. **GitHub Actions will automatically update** `index.json` upon merge

## ⚠️ Important Notes

- **Never edit `index.json` manually** - it's auto-generated
- **Always include all required fields** for our application to work properly
- **Use the exact field names** as shown in the schema
- **Test your template locally** before submitting a PR

## 📝 License

This project is open source and available under the [MIT License](LICENSE).
