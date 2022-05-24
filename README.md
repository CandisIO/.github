# Candis Workflows

This repo contains all shared workflows for candis organization.

## Supported flows:

### `build_and_test`
This flow is trying to build application and then run all basic checks against the codebase
(lint, prettier, unit tests, e2e tests).

#### prerequisites
- Service need to use MongoDB or Postgresql
- These commands need to be defined inside package.json: `lint`, `test:ci`, `test:e2e`, `build`

#### `secrets`
- `PUBLISH_PACKAGES` - Github token with permission to write/read candis's npm packages

### `deploy`
This process builds an application and docker image, push it to ECR, and then render yml files with helm which are sent to S3.
At the end, the spinnaker is triggered with all necessary metadata to deploy new version of the app.

#### prerequisites
- These commands need to be defined inside package.json: `build`
- In the repo we need `Dockerfile` at the root level
- In the repo has to be `_infra` directory with basic resources and `_infra/values` directory which contains `values.<ENV>.yaml` files for each env (`dev`, `prod`, `rc`).

#### `inputs`
- `ENVIRONMENT` - name of the env to deploy (`production`, `development`), determines which env will be deployed
#### `secrets`
- `PUBLISH_PACKAGES` - GitHub token with permission to write/read candis's npm packages
- `AWS_ECR_ACCESS_KEY_ID` - AWS access key id with ECR permissions. Used to push docker images.
- `AWS_ECR_SECRET_ACCESS_KEY` - AWS access key secret with ECR permissions
- `AWS_ECR_REGION`
- `AWS_S3_ACCESS_KEY_ID` - AWS access key id with S3 permissions. Used to push k8s's yml files to S3.
- `AWS_S3_SECRET_ACCESS_KEY` - AWS access key secret with S3 permissions
- `AWS_S3_REGION`
- `SPINNAKER_AUTH_USERNAME` - Spinnaker username, used to trigger new deploy.
- `SPINNAKER_AUTH_PASSWORD`- Spinnaker password.
- 
### `publish`
This flow is used to publish npm packages into private GitHub's registry.

#### prerequisites
- These commands need to be defined inside package.json: `build`
- Lerna needs to be added to `package.json`, each package needs to be able to run `tsc` command
- `*` for `GENERATE_CLIENT=true` repo needs to contain `packages` directory for all publishable packages.
- Lerna and its packages will publish everything into `https://npm.pkg.github.com/candisio`

#### `inputs`
- `ENVIRONMENT` - name of the env to deploy (`production`, `development`), determines if publish new version or new canary release
#### `secrets`
- `PUBLISH_PACKAGES` - GitHub token with permission to write/read candis's npm packages
- `GENERATE_CLIENT` - This boolean determines if the SDK client should be generated.
If true, it will generate the swagger's definition, then generate typescript API client and commit changes.
At the end it will publish new version of the packages. For `GENERATE_CLIENT=false` it won't commit changes but simply publish the current state.
