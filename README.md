# reposense-action

This action provides the following functionality for GitHub Actions users:

- Deploy a RepoSense report to github pages.

# Usage

```yaml
steps:
- uses: tlylt/reposense-action@main
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
```

Work in progress