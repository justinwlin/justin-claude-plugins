# Depot

Cloud-based Docker image building service with built-in caching and cross-platform support.

## Overview

Depot accelerates Docker builds using remote infrastructure. Key benefits:
- **Faster builds** via cloud resources and persistent caching
- **Cross-platform** builds (build linux/amd64 from Mac)
- **Direct push** to registries from remote (bypass local upload limits)
- **Ephemeral registry** for testing before final deployment

## Installation

```bash
# macOS
brew install depot/tap/depot

# Authenticate
depot login
```

## Common Commands

### Build

```bash
# Basic build (stays in remote cache)
depot build -t myimage:tag .

# Build and load to local Docker
depot build -t myimage:tag . --load

# Build and push directly to registry
depot build -t myimage:tag . --push

# Build for specific platform
depot build -t myimage:tag . --platform linux/amd64

# Build with custom Dockerfile
depot build -t myimage:tag . -f Dockerfile.prod

# Build with build args
depot build -t myimage:tag . --build-arg NODE_ENV=production

# Build specific stage
depot build -t myimage:tag . --target builder

# Save to Depot's ephemeral registry
depot build -t myimage:tag . --save

# Build with linting
depot build -t myimage:tag . --lint
```

### Pull/Push

```bash
# Pull from Depot registry to local
depot pull myimage:tag

# Push from Depot registry to remote registry
depot push myimage:tag registry.example.com/myimage:tag
```

### Project Management

```bash
# Initialize project in current directory
depot init

# List projects
depot projects list

# Create new project
depot projects create --name myproject

# View build history
depot list builds
```

### Cache

```bash
# Reset project cache (force fresh build)
depot cache reset
```

## Key Flags

| Flag | Description |
|------|-------------|
| `--load` | Download image to local Docker daemon |
| `--push` | Push to remote registry from builder |
| `--save` | Save to Depot ephemeral registry |
| `--platform` | Target platform (e.g., `linux/amd64`, `linux/arm64`) |
| `--file`, `-f` | Path to Dockerfile |
| `--target` | Build specific stage |
| `--build-arg` | Set build-time variables |
| `--no-cache` | Build without cache |
| `--project` | Specify Depot project ID |
| `--progress` | Output format: `auto`, `plain`, `tty` |
| `--lint` | Run linting during build |

## Environment Variables

| Variable | Description |
|----------|-------------|
| `DEPOT_PROJECT_ID` | Default project ID (alternative to `--project`) |
| `DEPOT_TOKEN` | Authentication token (for CI) |

## CI/CD Integration

For CI environments, use OIDC trust relationships (preferred) or project tokens:

```bash
# Generate short-lived pull token (1 hour)
depot pull-token

# Use project token
depot build -t myimage:tag . --token $DEPOT_TOKEN
```

## Runpod Usage

When building images for Runpod, you **must** target `linux/amd64` since Runpod runs Linux infrastructure:

```bash
# Build for Runpod (required platform flag)
depot build -t myimage:tag . --platform linux/amd64

# Build and push to registry for Runpod
depot build -t registry.example.com/myimage:tag . --platform linux/amd64 --push

# Build, save to Depot registry, then push to Runpod-compatible registry
depot build -t myimage:tag . --platform linux/amd64 --save
depot push myimage:tag your-registry.com/myimage:tag
```

**Important**: If you're developing on Mac (ARM), always include `--platform linux/amd64` or your images won't run on Runpod.

## Workflow Examples

### Build and Test Locally

```bash
# Build for local testing
depot build -t myapp:dev . --load

# Run locally
docker run myapp:dev
```

### Build and Deploy

```bash
# Build and push to production registry
depot build -t registry.example.com/myapp:v1.0.0 . --push
```

### Multi-Platform Build

```bash
# Build for both amd64 and arm64
depot build -t myapp:latest . --platform linux/amd64,linux/arm64 --push
```

## Troubleshooting

### Slow builds
- Check if cache was reset: `depot list builds`
- Ensure Dockerfile is optimized for caching (dependencies before code)

### Authentication issues
- Re-authenticate: `depot login`
- For CI, verify OIDC trust or token is configured

### Platform mismatch
- Explicitly set `--platform linux/amd64` when building for Linux from Mac

## Resources

- [Depot Documentation](https://depot.dev/docs)
- [CLI Reference](https://depot.dev/docs/cli/reference)
- [GitHub Actions Integration](https://depot.dev/docs/integrations/github-actions)
