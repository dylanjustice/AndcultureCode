Releasing new versions of a package
======

Outlines the general steps for publishing a new version of a package.

## Dotnet Core
* Ensure your `.csproj` has the following details in the `<PropertyGroup>`
    ```xml
    <Company>andculture</Company>
    <Description>Some meaningful description of the package</Description>
    <Authors>andculture</Authors>
    <PackageIconUrl>https://avatars3.githubusercontent.com/u/32297579?s=460&amp;v=4</PackageIconUrl>
    <PackageId>AndcultureCode.{Language}.{ProjectName}</PackageId>
    <PackageLicenseExpression>Apache-2.0</PackageLicenseExpression>
    <PackageLicenseFile>LICENSE.txt</PackageLicenseFile>
    <PackageProjectUrl>https://github.com/AndcultureCode/{RepositoryName}</PackageProjectUrl>
    <Version>1.0.0</Version>
    ```
* Add new `<ItemGroup>` to `.csproj`
    ```xml
    <ItemGroup>
        <None Include="LICENSE.txt" Pack="true" PackagePath="LICENSE.txt" />
    </ItemGroup>
    ```
* Update project's `.csproj` file `<Version>` using [SemVer conventions](https://docs.microsoft.com/en-us/nuget/concepts/package-versioning)
* Package your project with `dotnet pack` to create nupkg file
    * Now you should have a `.nupkg` file in your `bin/Debug` directory
* Publish to nuget.org using one of two methods
    * Manual
        * Log into nuget.org
        * Upload previously generated `.nupkg` file
    * Automatic (cli)
        * Log into nuget.org
        * Go to profile > API Keys
        * Create new key with your name
        * Select packages to manage
        * Copy key and store in password locker
        * Configure nuget locally with key
            * `nuget setapikey <apikey> -Source https://api.nuget.org/v3/index.json`
        * Shell to directory containing .nupkg
        * Use dotnet cli to publish new nupkg
            * `dotnet nuget push AndcultureCode.CSharp.{ProjectName} -s https://api.nuget.org/v3/index.json`