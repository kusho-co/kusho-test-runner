<p align="center">
  <img src="assets/kusho-logo.png" alt="KushoAI" width="96" />
</p>

# KushoAI Test Runner Action

Run KushoAI API and E2E test suites in GitHub Actions using the official KushoAI Docker runner.

```yaml
uses: kusho-co/kusho-test-runner@v1
with:
  api-key: ${{ secrets.KUSHO_API_KEY }}
  environment-id: ${{ vars.ENVIRONMENT_ID }}
  test-suite-uuid: ${{ vars.TEST_SUITE_UUID }}
```

## Prerequisites

GitHub-hosted Ubuntu runners include Docker by default. Before using this action, configure:

| Type | Name | Description |
| --- | --- | --- |
| Secret | `KUSHO_API_KEY` | KushoAI API key |
| Variable | `ENVIRONMENT_ID` | Target KushoAI environment ID |
| Variable | `TEST_SUITE_UUID` | Comma-separated API test suite UUIDs |
| Variable | `KUSHO_BASE_URL` | Optional KushoAI API base URL |

Store sensitive values as GitHub Secrets, not repository variables or workflow literals.

## Inputs

| Input | Required | Description |
| --- | --- | --- |
| `api-key` | Yes | KushoAI API key. Use `${{ secrets.KUSHO_API_KEY }}`. |
| `environment-id` | Yes | KushoAI environment ID to run against. |
| `base-url` | No | KushoAI API base URL. Defaults to `https://be.kusho.ai`. |
| `test-suite-uuid` | No | Comma-separated API test suite UUIDs. |
| `group-id` | No | KushoAI group ID. |
| `tags` | No | Comma-separated API test tags. |
| `e2e-test-suite-uuid` | No | E2E test suite UUID. |
| `execution-profile-uuid` | No | E2E execution profile UUID. |
| `e2e-test-suite-tags` | No | Comma-separated E2E test suite tags. |
| `e2e-profile-tags` | No | Comma-separated E2E execution profile tags. |
| `ci-commit-sha` | No | Commit SHA to associate with the KushoAI run. Defaults to `github.sha`. |
| `ci-commit-message` | No | Commit message to associate with the KushoAI run. Defaults to the push commit message, pull request title, or workflow name. |

Provide at least one execution selector: `test-suite-uuid`, `group-id`, `tags`, `e2e-test-suite-uuid`, or `e2e-test-suite-tags`.

## Workflow Examples

### Run API Test Suites by UUID

```yaml
name: Run Kusho Tests

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  kusho-tests:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Run Kusho API test suites
        uses: kusho-co/kusho-test-runner@v1
        with:
          api-key: ${{ secrets.KUSHO_API_KEY }}
          environment-id: ${{ vars.ENVIRONMENT_ID }}
          base-url: ${{ vars.KUSHO_BASE_URL || 'https://be.kusho.ai' }}
          test-suite-uuid: ${{ vars.TEST_SUITE_UUID }}
```

### Run a Group

```yaml
- name: Run Kusho group
  uses: kusho-co/kusho-test-runner@v1
  with:
    api-key: ${{ secrets.KUSHO_API_KEY }}
    environment-id: ${{ vars.ENVIRONMENT_ID }}
    group-id: ${{ vars.GROUP_ID }}
```

### Run Tagged API Tests

```yaml
- name: Run tagged Kusho tests
  uses: kusho-co/kusho-test-runner@v1
  with:
    api-key: ${{ secrets.KUSHO_API_KEY }}
    environment-id: ${{ vars.ENVIRONMENT_ID }}
    tags: ${{ vars.TEST_TAGS }}
```

### Run E2E Workflow by UUID

```yaml
- name: Run Kusho E2E workflow
  uses: kusho-co/kusho-test-runner@v1
  with:
    api-key: ${{ secrets.KUSHO_API_KEY }}
    environment-id: ${{ vars.ENVIRONMENT_ID }}
    e2e-test-suite-uuid: ${{ vars.E2E_TEST_SUITE_UUID }}
    execution-profile-uuid: ${{ vars.EXECUTION_PROFILE_UUID }}
```

### Run E2E Workflow by Tags

```yaml
- name: Run Kusho E2E workflow by tags
  uses: kusho-co/kusho-test-runner@v1
  with:
    api-key: ${{ secrets.KUSHO_API_KEY }}
    environment-id: ${{ vars.ENVIRONMENT_ID }}
    e2e-test-suite-tags: ${{ vars.E2E_TEST_SUITE_TAGS }}
    e2e-profile-tags: ${{ vars.E2E_PROFILE_TAGS }}
```

## Docker Image

This action runs:

```text
public.ecr.aws/y5g4u6y7/kusho-test-runner:latest
```

The action passes GitHub commit metadata to the container through `CI_COMMIT_SHA` and `CI_COMMIT_MESSAGE`.

## Troubleshooting

| Issue | Check |
| --- | --- |
| Authentication errors | Confirm `KUSHO_API_KEY` is set as a secret and has the right permissions. |
| Missing environment errors | Confirm `ENVIRONMENT_ID` is set and points to the intended KushoAI environment. |
| No tests selected | Provide one of `test-suite-uuid`, `group-id`, `tags`, `e2e-test-suite-uuid`, or `e2e-test-suite-tags`. |
| Docker pull failures | Confirm the GitHub runner has internet access and can reach the public ECR image. |

## License

MIT. See [LICENSE](LICENSE).
