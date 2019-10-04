# setup-dotnet

<p align="left">
  <a href="https://github.com/actions/setup-dotnet"><img alt="GitHub Actions status" src="https://github.com/actions/setup-dotnet/workflows/Main%20workflow/badge.svg"></a>
</p>

This action sets up a dotnet environment for use in actions by:

- optionally downloading and caching a version of dotnet by SDK version and adding to PATH
- registering problem matchers for error output

# Usage

See [action.yml](action.yml)

Basic:
```yaml
steps:
- uses: actions/checkout@master
- uses: actions/setup-dotnet@v1
  with:
    dotnet-version: '2.2.103' # SDK Version to use.
- run: dotnet build <my project>
```

Matrix Testing:
```yaml
jobs:
  build:
    runs-on: ubuntu-16.04
    strategy:
      matrix:
        dotnet: [ '2.2.103', '3.0.100-preview8-013656', '4.5.1' ]
    name: Dotnet ${{ matrix.dotnet }} sample
    steps:
      - uses: actions/checkout@master
      - name: Setup dotnet
        uses: actions/setup-dotnet@v1
        with:
          dotnet-version: ${{ matrix.dotnet }}
      - run: dotnet build <my project>
```

Authentication to GPR:
```yaml
steps:
- uses: actions/checkout@master
- uses: actions/setup-dotnet@v1
  with:
    dotnet-version: '2.2.103' # SDK Version to use.
    source-url: https://nuget.pkg.github.com
  env:
    NUGET_AUTH_TOKEN: ${{secrets.GITHUB_TOKEN}}
- run: dotnet build <my project>
- name: Create the package
  run: dotnet pack --configuration Release <my project>
  env:
    DOTNET_SKIP_FIRST_TIME_EXPERIENCE: true
 - name: Publish the package
  run: dotnet nuget push <my project>/bin/Release/*.nupkg
  env:
    DOTNET_SKIP_FIRST_TIME_EXPERIENCE: true
```

Authentication to Azure Artifacts:
```yaml
steps:
- uses: actions/checkout@master
- uses: actions/setup-dotnet@v1
  with:
    dotnet-version: '2.2.103' # SDK Version to use.
    source-url: https://pkgs.dev.azure.com/<your-organization>/_packaging/<your-feed-name>/nuget/v3/index.json
  env:
    NUGET_AUTH_TOKEN: ${{secrets.AZURE_DEVOPS_PAT}} # Note, create a secret with this name in Settings
- run: dotnet build <my project>
- name: Create the package
  run: dotnet pack --configuration Release <my project>
  env:
    DOTNET_SKIP_FIRST_TIME_EXPERIENCE: true
 - name: Publish the package
  run: dotnet nuget push <my project>/bin/Release/*.nupkg
  env:
    DOTNET_SKIP_FIRST_TIME_EXPERIENCE: true
```

# License

The scripts and documentation in this project are released under the [MIT License](LICENSE)

# Contributions

Contributions are welcome!  See [Contributor's Guide](docs/contributors.md)
