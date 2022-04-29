# reposense-action

This action provides the following functionality for GitHub Actions users:

- Deploy a RepoSense report to github pages/surge

# Usage

```yaml
on:
# For active deployment on push to main branch
  push:
    branches:
      - main

# For scheduled deployment with Cron Jobs.
## Examples of cron schedule expressions:
### '0 * * * *': hourly
### '0 0 * * *': daily
### '0 0 1 * *': monthly
#   schedule:
#     - cron:  '0 0 * * *'

steps:
- uses: tlylt/reposense-action@main
  with:
    token: ${{ secrets.GITHUB_TOKEN }} # Required
    version: 'master' # Optional | Default: release | Other: master/tag v1.6.1/etc
    configDirectory: 'configs' # Optional | Default: configs
    service: 'gh-pages' # Optional | Default: gh-pages | Other: surge
```

# Testing

```yaml
name: Test reposense-action

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - uses: tlylt/reposense-action@main
      with:
        token: ${{ secrets.GITHUB_TOKEN }}
        version: 'release'
        configDirectory: 'configs'
        service: 'gh-pages'
```