# gh-ci

A Tauri desktop application for learning GitHub Actions CI/CD and auto-update.

## Features

- GitHub Actions CI/CD workflow
- Tauri auto-update support
- Windows installer (NSIS, MSI)

## Development

```bash
# Install dependencies
pnpm install

# Run in development mode
pnpm tauri dev

# Build for production
pnpm tauri build
```

## CI/CD

- **CI**: Runs on every push to `main` branch
- **Release**: Triggers on git tag push (`v*.*.*`)
