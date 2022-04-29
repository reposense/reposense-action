# reposense-action

This action provides the following functionality for GitHub Actions users:

- Deploy a RepoSense report to github pages/surge

[RepoSense](https://reposense.org/) is a tool for analyzing GitHub repositories and generating reports.

- Read the User Guide [here](https://reposense.org/ug/index.html)
- Understand how to specify the config files [here](https://reposense.org/ug/configFiles.html)

## Usage

A simple 2-step process:

- Specify the config files in the `configs` folder of your repo.
  - You may see a sample config files in the [`configs` folder](./configs).
  - Using a different folder name is possible, just ensure you pass the correct folder name to the action.
- Create a `reposense.yml` with the following content in `.github/workflows/`
  - Make changes as necessary, such as the triggering event

```yaml
name: Deploy RepoSense report

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

## Testing

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
