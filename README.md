# KushoAI Test Runner Action

This action runs KushoAI test suites using Docker.

## Inputs

### `test-suite-uuid`
**Required** The Test Suite UUID.

### `environment-id`
**Required** The Environment ID.

### `api-key`
**Required** Your KushoAI API Key.

### `email`
**Required** Comma separated email addresses for notifications.

## Example usage

```yaml
uses: kusho-co/kusho-test-runner@v1
with:
  test-suite-uuid: 'your-test-suite-uuid'
  environment-id: 'your-environment-id'
  api-key: ${{ secrets.KUSHOAI_API_KEY }}
  email: 'user1@example.com,user2@example.com'