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

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: tlylt/reposense-action@main
      with:
        token: ${{ secrets.GITHUB_TOKEN }} # Required
        version: 'release' # Optional | Default: release | Other: master/tag v1.6.1/etc
        configDirectory: 'configs' # Optional | Default: configs
        service: 'gh-pages' # Optional | Default: gh-pages | Other: surge
        domain: '' # Optional (Required if service = surge) | Default: '' | Other: '<subDomain>.surge.sh'
```

## Option details

### service

Currently two types of publishing services are supported:

- GitHub Pages
  - `'gh-pages'`
  - See [here](https://docs.github.com/en/pages/getting-started-with-github-pages) for more details
- Surge.sh
  - `'surge'`
  - See [here](https://surge.sh/) for more details

### token

Currently two types of tokens are supported (correspond to the two supported publishing services):

- Token for GitHub Pages
  - Simply use `${{ secrets.GITHUB_TOKEN }}`
  - Note that you need to ensure that you have selected the branch that you want to deploy to in your [GitHub repo's Pages settings](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#choosing-a-publishing-source)
  - Deployment to GitHub Pages might take 5-10 minutes before the site is available/refreshed
- Token for Surge.sh
  - `${{ secrets.SURGE_TOKEN }}`
    - `SURGE_TOKEN` is the environment secret name
  - Require registration with an email address
  - After retrieving the token, put the token as a repository secret in your repository
  - See [here](https://markbind.org/userGuide/deployingTheSite.html#previewing-prs-using-surge) for a detailed guide on how to retrieve the token

### domain

The domain that the site is available at. Required if `service` chosen is Surge.

- A surge.sh subdomain
  - `'<subDomain>.surge.sh'`
  - Surge allows you to specify a subdomain for free as long as it has not been taken up by others. You have to ensure that the `<subDomain>` is unique. 
  - A possible subdomain to use is your repository name: e.g. `mb-test.surge.sh`
- A custom domain that you have configured with Surge
  - Read the [Surge documentation](https://surge.sh/help/adding-a-custom-domain) to understand how to set it up

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
