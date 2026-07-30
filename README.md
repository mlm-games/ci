# ci

Shared reusable GitHub Actions workflows for projects that are structured like my projects.

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
```

### `aur-upload.yml`

Publish a PKGBUILD to AUR on release. Downloads the Linux binary from the GitHub release, computes SHA256, and pushes to AUR.

```yaml
jobs:
  aur:
    uses: mlm-games/ci/.github/workflows/aur-upload.yml@v1
    with:
      pkgname: myapp-bin
      pkgdesc: "Description of my app"
      binary-name: myapp-linux-x86_64
      license: "MIT OR Apache-2.0"
    secrets:
      AUR_SSH_PRIVATE_KEY: ${{ secrets.AUR_SSH_PRIVATE_KEY }}
```

| Input | Default | Description |
|-------|---------|-------------|
| `pkgname` | — | AUR package name (required) |
| `pkgdesc` | — | Package description (required) |
| `binary-name` | — | Release asset name (required, e.g. `myapp-linux-x86_64`) |
| `app-slug` | `pkgname` with `-bin` stripped | Installed binary name |
| `license` | `MIT OR Apache-2.0` | Package license |
| `depends` | `""` | Space-separated AUR dependencies |
| `provides` | `""` | Space-separated provides |
| `conflicts` | `""` | Space-separated conflicts |
| `arch` | `x86_64` | Package architecture |
| `maintainer-name` | `""` | AUR maintainer name |
| `maintainer-email` | `""` | AUR maintainer email |

| Secret | Description |
|--------|-------------|
| `AUR_SSH_PRIVATE_KEY` | SSH private key for AUR push (required) |

## Usage

Pin by tag (e.g. `@v1`) for stability. Releases use semver tags.
