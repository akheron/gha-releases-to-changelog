# Releases to Changelog Action

Convert the list of releases to a changelog

## Inputs

### `token`

**Required**: The GITHUB_TOKEN secret.

### `title-template`

Configure the title of the release. Default: `%%TITLE%%`.

### `description-template`

Configure the description of the release. Default: `%%DESCRIPTION%%`.

## Outputs

### `changelog`

The changelog that was generated.

### `latest`

The tag name of the latest release.

## Example usage

The workflow below assumes that your main branch is called `main` and writes the
changelog to `CHANGELOG.md`.

```
name: Changelog
on:
  release:
    types: [published, edited]
jobs:
  update-changelog:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          ref: main
      - name: Generate markdown changelog
        id: changelog
        uses: akheron/gha-releases-to-changelog@v1
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          title-template: '# %%TITLE%%'
      - run: |
          cat >CHANGELOG.md <<EOF
          ${{ steps.changelog.outputs.changelog }}
          EOF
      - run: |
          git config user.name "changelog-bot"
          git config user.email "<>"
      - run: |
          git add CHANGELOG.md
          git commit -m "Update changelog"
          git push origin main
```
