# Candis Workflows

This repo contains all shared workflows for candis organization.

## Supported flows:

### `build_and_test`
This flow is trying to build application and then run all basic checks against the codebase
(lint, prettier, unit tests, e2e tests).

#### `secrets`
- `PUBLISH_PACKAGES` - Github token with permission to write/read candis's npm packages

### `deploy`
#### `secrets`
- `PUBLISH_PACKAGES` - Github token with permission to write/read candis's npm packages

### `publish`
This flow is used to publish npm packages into private github's registry.
#### `secrets`
- `PUBLISH_PACKAGES` - Github token with permission to write/read candis's npm packages

