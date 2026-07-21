# ci

Shared reusable GitHub Actions workflows for mlm-games projects.

## Workflows

### `android-ci.yml`

Standard Android CI: build + test + lint.

```yaml
jobs:
  ci:
    uses: mlm-games/ci/.github/workflows/android-ci.yml@v1
```

| Input | Default | Description |
|-------|---------|-------------|
| `java-version` | `21` | JDK version |
| `java-distribution` | `corretto` | JDK distribution |
| `build-command` | `./gradlew assembleDebug` | Build command |
| `test-command` | `./gradlew test` | Test command |
| `lint-command` | `./gradlew lint` | Lint command |

### `android-release.yml`

Tag-driven Android release: version bump, signed AAB/APKs, GitHub Release, optional Play Store upload.

```yaml
jobs:
  release:
    uses: mlm-games/ci/.github/workflows/android-release.yml@v1
    with:
      app-name: MyApp
      app-slug: myapp
      application-id: com.example.myapp
    secrets:
      KEYSTORE: ${{ secrets.KEYSTORE }}
      KEYSTORE_PASSPHRASE: ${{ secrets.KEYSTORE_PASSPHRASE }}
      STORE_PASSWORD: ${{ secrets.STORE_PASSWORD }}
      KEY_ALIAS: ${{ secrets.KEY_ALIAS }}
      KEY_PASSWORD: ${{ secrets.KEY_PASSWORD }}
```

### `rust-ci.yml`

Standard Rust CI: fmt + clippy + test + optional audit.

```yaml
jobs:
  ci:
    uses: mlm-games/ci/.github/workflows/rust-ci.yml@v1
```

### `dependabot-auto-merge.yml`

Auto-approve + auto-merge all Dependabot PRs.

```yaml
jobs:
  dependabot:
    uses: mlm-games/ci/.github/workflows/dependabot-auto-merge.yml@v1
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Usage

Pin by tag (e.g. `@v1`) for stability. Releases use semver tags.
