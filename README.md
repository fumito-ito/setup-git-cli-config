# Setup Git Config Action

A custom GitHub Action for configuring Git settings in GitHub Actions workflows. Easily configure user information and authentication settings.

## Features

- Configure Git configurations
  - User name (`git config user.name`)
  - User email (`git config user.email`)
  - Authentication header (`git config http.https://github.com/.extraheader`)

## Input Parameters

### `user-name`

**Description**: Value to be set via `git config user.name`

**Required**: No

**Default**: `github-actions[bot]`

**Recommended**: `actions-user` is also a recommended value

### `user-email`

**Description**: Value to be set via `git config user.email`

**Required**: No

**Default**: `41898282+github-actions[bot]@users.noreply.github.com`

**Note**: When `user-name` is `actions-user`, the recommended value is `65916846+actions-user@users.noreply.github.com`

### `token`

**Description**: GitHub token for authentication

**Required**: No

**Default**: `${{ github.token }}`

**Important**: The specified value will override `http.https://github.com/.extraheader`

## Usage Examples

### Basic Usage (Default Settings)

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: fumito-ito/set-up-git-config@v0.0.1
```

### Custom Configuration

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: fumito-ito/set-up-git-config@v0.0.1
    with:
      user-name: 'actions-user'
      user-email: '65916846+actions-user@users.noreply.github.com'
      token: ${{ secrets.GITHUB_TOKEN }}
```

### Using Personal Access Token

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: fumito-ito/set-up-git-config@v0.0.1
    with:
      user-name: 'my-bot'
      user-email: 'my-bot@example.com'
      token: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
```

## Configuration Details

This Action sets the following git configurations:

1. **User Information**:
   ```bash
   git config user.name "<specified value>"
   git config user.email "<specified value>"
   ```

2. **Authentication Header**:
   ```bash
   NEW_AUTH="AUTHORIZATION: basic $(echo -n "x-access-token:<token>" | base64)"
   git config http.https://github.com/.extraheader "$NEW_AUTH"
   ```

## Notes

- This Action is implemented as a `composite` action
- The authentication token setting will override existing `http.https://github.com/.extraheader`
- The token is used temporarily as an environment variable and set as a base64-encoded authentication header

## License

MIT License