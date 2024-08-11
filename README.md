# Version Increment Action

![GitHub release (latest by date)](https://img.shields.io/github/v/release/saleweaver/version_increment_action)

## Overview

The **Version Increment Action** is a GitHub Action designed to automatically manage version increments in your project. It reads the version from a specified file, increments it based on your commit messages, and commits the updated version back to your repository. This action is particularly useful in continuous integration (CI) workflows to automate versioning.

## Features

- **Automatic Version Increments**: Automatically increments the patch version number by default (e.g., `1.0.0` to `1.0.1`).
- **Custom Version Support**: Allows setting a custom version number directly from the commit message (e.g., `version: 2.0.0`).
- **Git Commit & Push**: Commits the updated version file back to the repository.
- **Outputs New Version**: Exposes the new version as an output for subsequent steps in your GitHub Workflow.

## Usage

### Example Workflow

Here’s an example workflow that uses the Version Increment Action:

```yaml
name: CI

on:
  push:
    branches:
      - main

jobs:
  versioning:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      actions: write 
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Increment version
        uses: saleweaver/version_increment_action@v1
        with:
          file_path: "<path_to_your>/__version__.py"
          version_key: "__version__"

      - name: Create a new GitHub release
        if: env.new_version != ''
        uses: actions/create-release@v1
        with:
          tag_name: "v${{ env.new_version }}"
          release_name: "v${{ env.new_version }}"
          draft: false
          prerelease: false
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```


## Inputs

- **`file_path`** (required): The path to the file containing the version string.
- **`version_key`** (required): The key used to identify the version string in the file.

## Outputs

- **`new_version`**: The newly incremented or specified version number.

## Custom Versioning

To specify a custom version number in your commit message, include `version: X.Y.Z` in the message. The action will then use this version number instead of incrementing the patch version.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.