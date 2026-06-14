# flutter_ci_cd_ios_ipa_builder

A GitHub Actions CI/CD workflow that builds a Flutter iOS app into an unsigned `.ipa` archive.

## Prerequisites

- A Flutter project in a `frontend/` subdirectory (relative to the workflow)
- iOS configured in the Flutter project (`ios/` directory with Runner.xcodeproj)
- A GitHub repository with GitHub Actions enabled

## Usage

1. Place `build-ios.yml` in `.github/workflows/` of your repository.
2. Go to the **Actions** tab in your GitHub repo.
3. Select **Build iOS IPA** and click **Run workflow**.
4. Fill in the input values (or keep defaults) and run.
5. Once complete, download the artifact containing the `.ipa`.

### Workflow Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `api_url` | Yes | `http://32.236.105.9` | API URL written into `.env` |
| `app_name` | Yes | `sbku_app` | Name used for the `.ipa` filename |
| `artifact_name` | Yes | `sbku_app_ipa` | Name of the uploaded artifact |

## What it does

| Step | Action |
|------|--------|
| 1 | Checkout repository |
| 2 | Setup Flutter (stable) |
| 3 | `flutter pub get` |
| 4 | Create `.env` file with `API_URL` from input |
| 5 | `flutter build ios --no-codesign --release` |
| 6 | Package `Runner.app` into `<app_name>.ipa` |
| 7 | Upload `.ipa` as a GitHub Actions artifact |

## Configuration

- **Code signing**: Remove `--no-codesign` and add signing credentials via secrets
- **Environment variables**: Use [GitHub Secrets](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions) for sensitive values instead of the `api_url` input

## Notes

- This workflow **must** run on `macos-latest` (iOS builds require macOS).
- The `.ipa` is **unsigned** (`--no-codesign`) and cannot be installed on physical devices without proper distribution certificates.
- The workflow is triggered manually via `workflow_dispatch` — it does **not** run on push or PR.
