# Docker Compose Templates

A curated collection of Docker Compose template files with automated index generation.

## 📁 Repository Structure

```
/templates/
   fullstack-starter.yml
   microservices.yml
   ...
index.json
```

## 🚀 Quick Start

1. **Browse Templates**: Check the `index.json` file for available templates
2. **Use a Template**: Download and run any template:

```bash
# Example: Fullstack Starter
docker-compose -f templates/fullstack-starter.yml up

# Example: Microservices Architecture  
docker-compose -f templates/microservices.yml up
```

## 📋 Template Schema

Each template follows this custom schema:

```yaml
version: "3.9"

# 🔹 Main group metadata
name: "Template Name"
description: "Short description"
techstack:
  - Technology1
  - Technology2
imageUrl: "https://example.com/icon.png"
sharedVariable:
  ENVIRONMENT: "development"

services:
  service-name:
    type: Application|Database|Cache|Gateway
    order: 1
    serviceUrl: "https://example.com/service.zip"
    imageUrl: "https://example.com/service.png"
    image: image:tag
    # ... standard docker-compose service configuration
```

## 🔄 Automated Index Generation

The `index.json` file is automatically generated and updated via GitHub Actions:

- **Trigger**: Every push to `main` branch that modifies files in `/templates/*.yml`
- **Process**: Parses all YAML templates and extracts metadata
- **Output**: Updates `index.json` with template information

### Index.json Structure

```json
[
  {
    "name": "Template Name",
    "techstack": ["Technology1", "Technology2"],
    "description": "Short description",
    "iconUrl": "https://example.com/icon.png",
    "services": ["service1", "service2"],
    "usage": "docker-compose -f templates/template-name.yml up"
  }
]
```

## 🛠️ Adding New Templates

1. **Create Template**: Add your `.yml` file to the `/templates/` directory
2. **Follow Schema**: Ensure your template includes the required metadata fields
3. **Commit & Push**: The GitHub Action will automatically update `index.json`

### Required Metadata Fields

- `name`: Template display name
- `description`: Brief description of the template
- `techstack`: Array of technologies used
- `imageUrl`: URL to template icon/image
- `services`: Object containing service definitions

## 🔧 Manual Index Generation

If you need to manually regenerate the index (for testing):

```bash
# The GitHub Action workflow contains the generation logic
# You can run it manually by triggering the workflow
```

## 📊 Available Templates

Check `index.json` for the complete list of available templates with their metadata.

## 🤝 Contributing

1. Fork the repository
2. Add your template to `/templates/`
3. Ensure it follows the schema
4. Submit a pull request
5. The index will be automatically updated upon merge

## 📝 License

This project is open source and available under the [MIT License](LICENSE).
