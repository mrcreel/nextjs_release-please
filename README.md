``` bash
npm i -D commitizen cz-conventional-changelog
```

``` bash
# Initialize Commitizen
npx commitizen init cz-conventional-changelog --save-dev --save-exact --force
```

``` json
"scripts": {
    // Add script to package.json
    "commit": "cz"
  },
```

``` yml
// .github/workflows/release-please.yml

on:
  push:
    branches:
      - main

permissions:
  contents: write
  pull-requests: write

name: release-please

jobs:
  release-please:
    runs-on: ubuntu-latest
    steps:
      - uses: googleapis/release-please-action@v4
        id: release
        with:
          release-type: node
      - uses: actions/checkout@v4
      - name: tag major and minor versions
        if: ${{ steps.release.outputs.release_created }}
        run: |
          git config user.name github-actions[bot]
          git config user.email 41898282+github-actions[bot]@users.noreply.github.com
          git remote add gh-token "https://${{ secrets.GITHUB_TOKEN }}@github.com/googleapis/release-please-action.git"
          git tag -d v${{ steps.release.outputs.major }} || true
          git tag -d v${{ steps.release.outputs.major }}.${{ steps.release.outputs.minor }} || true
          git push origin :v${{ steps.release.outputs.major }} || true
          git push origin :v${{ steps.release.outputs.major }}.${{ steps.release.outputs.minor }} || true
          git tag -a v${{ steps.release.outputs.major }} -m "Release v${{ steps.release.outputs.major }}"
          git tag -a v${{ steps.release.outputs.major }}.${{ steps.release.outputs.minor }} -m "Release v${{ steps.release.outputs.major }}.${{ steps.release.outputs.minor }}"
          git push origin v${{ steps.release.outputs.major }}
          git push origin v${{ steps.release.outputs.major }}.${{ steps.release.outputs.minor }}
```

git/repo/settings/actions/general

[x] Allow GitHub Actions to create and approve pull requests