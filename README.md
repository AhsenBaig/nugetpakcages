# NuGet package manager - Test push package
## Initial Setup
Only need to do this once unless you setup expiration.
### Setup a personal access token
#### GitHub -> Settings -> Developer Settings -> Personal access tokens

> **NOTE:** Had to check **repo** before selecting write:packages and delete:packages. It seems to be a bug in GitHub. Use best practices on **access** and **expirations**.
![PAT Screenshot](images/GitHub-personal_access_token_dark.png#gh-dark-mode-only)
![PAT Screenshot](images/GitHub-personal_access_token_light.png#gh-light-mode-only)

> **NOTE:** Make sure to copy your personal access token now. You won’t be able to see it again!
![PAT Key Screenshot](images/GitHub-personal_access_token_key_dark.png#gh-dark-mode-only)
![PAT Key Screenshot](images/GitHub-personal_access_token_key_light.png#gh-light-mode-only)

### Configure NuGet source

Only need to do this once unless your <YOUR_PAT_TOKEN> token expired.

```ps
nuget source Add -Name "AhsenBaigGitHub" -Source "https://nuget.pkg.github.com/AhsenBaig/index.json" -UserName <YOUR_USERNAME> -Password <YOUR_PAT_TOKEN>
nuget setapikey <YOUR_PAT_TOKEN> -Source "https://nuget.pkg.github.com/AhsenBaig/index.json"
```
---
## Create .net package
```ps
dotnet new console --name nugetpackages
```

```ps
cd nugetpackages
```
### Modify OctocatApp.csproj

```xml
<Version>1.0.0</Version>
<Authors>Octocat and AhsenBaig</Authors>
<Company>AhsenBaig</Company>
<PackageDescription>This package adds an Octocat!</PackageDescription>
<RepositoryUrl>https://github.com/AhsenBaig/nugetpackages</RepositoryUrl>
```

### Create nuget.config
```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <packageSources>
        <clear />
        <add key="AhsenBaigGitHub" value="https://nuget.pkg.github.com/AhsenBaig/index.json" />
    </packageSources>
</configuration>
```

### Build project
```ps
dotnet pack --configuration Release
```

#### Push the package
```ps
nuget push "bin/Release/OctocatApp.1.0.0.nupkg" -Source "AhsenBaigGitHub"
```

#### or

```ps
dotnet nuget push "/bin/Release/nugetpakcages.1.0.0.nupkg" --source "AhsenBaigGitHub"
```
