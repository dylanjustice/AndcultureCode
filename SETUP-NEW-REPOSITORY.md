Setting up a new repository
======

Below is a quick checklist to use a guide when setting up a new repository to share on github.

* Create new repository on andcultureCode github account using established naming conventions
    ```csharp
        AndcultureCode.{LanguageName}.{PackageName}.{ChildPackageName}
        // ex. AndcultureCode.CSharp.Extensions

        AndcultureCode.{LanguageName}.{PopularPackageName}
        // ex. AndcultureCode.CSharp.FactoryGirl
    ```
* Configure the correct contributers from the development team
* Add relevent topics and description
* Unless otherwise specified: Select Apache 2.0 license
* Add default `CONTRIBUTING.md` document that links to this repository
    ```
    How to Contribute
    ======
    Information on contributing to this repo is in the [Contributing Guide](https://github.com/
    AndcultureCode/AndcultureCode/blob/master/CONTRIBUTING.md) in the Home repo.
    ```
* Regardless of technology, please ensure the following...
    * Clear and concise setup and baseline usage guidelines in top-level README
    * Automated test projects setup
    * Continuous integration builds configured (See [Travis CI](https://travis-ci.org))


## Setting up Travis CI
See [Travis Tutorial](https://docs.travis-ci.com/user/tutorial/)

* Log into Travis CI organization account (github auth)
* Ensure your repository is public
* Go to settings for project and select following options...
    * General
        * Build pushed branches: yes
        * Build pushed pull requests: yes
* Add `.travis.yml` to top-level of repository
    ```yaml
    dotnet: 2.2.1
    language: csharp
    mono: none
    script:
      - dotnet restore
      - dotnet test
    solution: All.sln
    ```
* After push of `.travis.yml` file, force build (if not already triggered) and debug.
* Add status image for job to top-level README
    ```markdown
    [![Build Status](https://travis-ci.org/AndcultureCode/AndcultureCode.CSharp.Extensions.svg?branch=master)](https://travis-ci.org/AndcultureCode/AndcultureCode.CSharp.Extensions)
    ```
