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

Publish a PKGBUILD to AUR on release. Downloads the Linux binary from the GitHub release, computes SHA256, installs a `.desktop` launcher + icon by default, and pushes to AUR.

```yaml
jobs:
  aur:
    uses: mlm-games/ci/.github/workflows/aur-upload.yml@v1
    with:
      pkgname: myapp-bin
      pkgdesc: "Description of my app"
      binary-name: myapp-1.2.3-x86_64-unknown-linux-gnu.tar.gz
      binary-name-aarch64: myapp-1.2.3-aarch64-unknown-linux-gnu.tar.gz
      app-name: "My App"
      desktop-categories: "Utility;"
      desktop-args: "%F"
      icon-url: "https://raw.githubusercontent.com/me/myapp/main/icon.png"
      license: "MIT OR Apache-2.0"
    secrets:
      AUR_SSH_PRIVATE_KEY: ${{ secrets.AUR_SSH_PRIVATE_KEY }}
```

| Input | Default | Description |
|-------|---------|-------------|
| `pkgname` | — | AUR package name (required) |
| `pkgdesc` | — | Package description (required) |
| `binary-name` | — | Release asset name (required, e.g. `myapp-linux-x86_64`) |
| `binary-name-aarch64` | `""` | aarch64 release asset name; if set, builds a dual-arch (`x86_64` + `aarch64`) package where `binary-name` is the x86_64 asset |
| `app-slug` | `pkgname` with `-bin` stripped | Installed binary name |
| `app-name` | `app-slug` | Display name for the `.desktop` entry |
| `license` | `MIT OR Apache-2.0` | Package license |
| `depends` | `""` | Space-separated AUR dependencies |
| `provides` | `""` | Space-separated provides |
| `conflicts` | `""` | Space-separated conflicts |
| `version` | `""` (latest release) | Version to publish |
| `arch` | `x86_64` | Package architecture (single-arch only; ignored for dual-arch) |
| `install-desktop` | `true` | Install a `.desktop` launcher to `/usr/share/applications` |
| `desktop-comment` | `pkgdesc` | `Comment` field for the `.desktop` entry |
| `desktop-categories` | `"Utility;"` | Semicolon-separated `.desktop` Categories |
| `desktop-mime-type` | `""` | Semicolon-separated `.desktop` MimeType |
| `desktop-args` | `""` | Extra `Exec` args (e.g. `"%F"` or `"%U"`) |
| `icon-url` | `""` | App icon URL (png/svg); installed to `pixmaps` + `hicolor`. Empty disables icon install |
| `maintainer-name` | `""` | AUR maintainer name |
| `maintainer-email` | `""` | AUR maintainer email |

| Secret | Description |
|--------|-------------|
| `AUR_SSH_PRIVATE_KEY` | SSH private key for AUR push (required) |

## Usage

Pin by tag (e.g. `@v1`) for stability. Releases use semver tags.
