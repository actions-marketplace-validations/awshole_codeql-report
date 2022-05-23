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
      uses: github/codeql-action/analyze@v1.1
    - name: CodeQL Report
      uses: awshole/codeql-report@v1
      with:
        github_issue_assignee: 'awshole'
```

## JavaScript example

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
      uses: awshole/codeql-report@v1.1
      with:
        github_issue_assignee: 'awshole'
```

## .NET Core example

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
        language: [ 'csharp' ]
    steps:
    - name: Checkout repository
      uses: actions/checkout@v2
    - name: Setup .NET 6
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: 6.x.x
    - name: .NET Restore
      run: Get-ChildItem -Filter "*.sln" -Recurse | ForEach-Object {nuget restore $_.FullName}
      shell: pwsh
    - name: Initialize CodeQL
      uses: github/codeql-action/init@v1
      with:
        languages: ${{ matrix.language }}
    - name: Build Project
      run: Get-ChildItem -Filter "*.csproj" -Recurse | ForEach-Object {dotnet build $_.FullName /p:UseSharedCompilation=false /p:Configuration=Release}
      shell: pwsh
    - name: Perform CodeQL Analysis
      uses: github/codeql-action/analyze@v1
    - name: CodeQL Report
      uses: awshole/codeql-report@v1.1
      with:
        github_issue_assignee: 'awshole'
```

The CodeQL Report Action has properties which are passed to the underlying shell executing custom scripts. These are
passed to the action using `with`.

| Property                        | Required | Default | Description                                                                                |
|---------------------------------|----------|---------|--------------------------------------------------------------------------------------------|
| github_issue_assignee           | FALSE    |         | Expects a string value corresponding to the GitHub user to assign issues to.             |
