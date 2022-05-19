# CodeQL Report Action

A [GitHub Action](https://github.com/features/actions) for using [CodeQL](https://codeql.github.com/docs/) and posting output to a GitHub Issue.

You can use the Action as follows:

## Python example

```yaml
name: codeql-analysis
on:
  workflow_dispatch:
  push: 
    branches: [main]
  pull_request:
    branches: [main]
jobs:
  codeql-analysis:
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false
      matrix:
        language: [ 'python' ]
    steps:
    - name: Checkout repository
      uses: actions/checkout@v2
    - name: Initialize CodeQL
      uses: github/codeql-action/init@v1
      with:
        languages: ${{ matrix.language }}
    - name: Perform CodeQL Analysis
      uses: github/codeql-action/analyze@v1
    - name: CodeQL Report
      uses: awshole/codeql-report@v1
      with:
        github_repository: ${{ github.repository }}
        codeql_github_integration_token: ${{ github.token }}
        github_issue_assignee: 'awshole'
```

## Javascript example

```yaml
name: codeql-analysis
on:
  workflow_dispatch:
  push: 
    branches: [main]
  pull_request:
    branches: [main]
jobs:
  codeql-analysis:
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false
      matrix:
        language: [ 'javascript' ]
    steps:
    - name: Checkout repository
      uses: actions/checkout@v2
    - name: Initialize CodeQL
      uses: github/codeql-action/init@v1
      with:
        languages: ${{ matrix.language }}
    - name: Perform CodeQL Analysis
      uses: github/codeql-action/analyze@v1
    - name: CodeQL Report
      uses: awshole/codeql-report@v1
      with:
        github_repository: ${{ github.repository }}
        codeql_github_integration_token: ${{ github.token }}
        github_issue_assignee: 'awshole'
```

The CodeQL Report Action has properties which are passed to the underlying shell executing custom scripts. These are
passed to the action using `with`.

| Property                        | Required | Default | Description                                                                                |
|---------------------------------|----------|---------|--------------------------------------------------------------------------------------------|
| github_issue_assignee           | FALSE    |         | Expects a string value corresponding to the GitHub user to assign issues to.             |
