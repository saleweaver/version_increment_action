# Version Increment Action

This GitHub Action automatically increments the version in a specified version file. It can be used in your workflows to manage versioning for your projects.

## Inputs

- `file_path`: The file path where the version is stored.
- `version_key`: The version key to search for (e.g., `__version__`).

## Outputs

- `new_version`: The new version number.

## Example Usage

```yaml
jobs:
  versioning:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Increment version
        uses: yourusername/version-increment-action@v1
        with:
          file_path: '<path_to_your>/__version__.py'
          version_key: '__version__' # key to search for in the file, holding the version 

      - name: Create a new GitHub release
        uses: actions/create-release@v1
        with:
          tag_name: "v${{ env.new_version }}"
          release_name: "v${{ env.new_version }}"