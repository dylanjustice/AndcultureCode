# Code Conventions & Best Practices

> The purpose of this and related documents is to outline the established programming conventions for the various languages used by andculture engineering.

---

## General

Our approach to any language used at andculture should be to use the respective community as a guide. By using community established conventions and styles, documentation, onboarding, tooling, etc... just fall into place.

### Formatting

The goal is to maintain auto-formatting no matter what. So if a decision results in needing to disable auto-formatting, it should be avoided.

We try our best to leverage prettier to handle auto-formatting. Check out the [.prettierrc](.prettierrc) file as it contains our common configuration.

Using a [git pre-commit hook](.githooks/pre-commit) we ensure prettier is run on the entire source tree before being commited into source control.

### Organization

When organizing a given file, we try to use these general rules of thumb

#### Alphabetization

-   Properties, fields and members on interfaces, classes, and objects **within a given region**
-   Methods by name within a region
    -   This is less important for test methods/names within a region, so long as the region is alphabetically placed
-   Constructor params
-   Assignments within a constructor
-   Named parameters in function calls (not necessarily on the signature itself)
-   Error keys in localization files
-   Some common examples include:

**Common Examples**

-   AclStrings
-   DBSets in a Context, IContext
-   IServiceCollectionExtensions DI registrations
-   MappingProfile entries
-   Props for rendering a JSX Element/component

Regions themselves whenever a pre-established order does not exist

#### Regions

We like to wrap our logical types of actors in a file within comment regions. Along with allowing editors to collapse groups of code, it more so makes the file easier to read.

Being each language and tech stack can have their own respective actors, look to each language's code convention file for their order to regions.

---

## Language Specific

Each language and framework are broken into their own child document.

-   [CSharp](CODE-CONVENTIONS-CSHARP.md)
-   [TypeScript](CODE-CONVENTIONS-TYPESCRIPT.md)

---

## Tooling

### Visual Studio Code

#### Extensions

We use a few extensions available on the market place for language support, linting, formatting, and snippet generation. When you open the repository in VS Code, it should recommend the extensions listed in [.vscode/extensions.json](.vscode/extensions.json).

-   [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
    -   CSharp plugin for language support, intellisense, etc
-   [C# XML Documentation Comments](https://marketplace.visualstudio.com/items?itemName=k--kato.docomment)
    -   CSharp XML doc generation
-   [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
    -   ESLint plugin for linting
-   [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)
    -   Enables attaching to Chrome processes for frontend debugging
-   [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
    -   Prettier plugin for formatting
-   [Trailing Spaces](https://marketplace.visualstudio.com/items?itemName=shardulm94.trailing-space)
    -   Highlights & deletes trailing whitespace characters
-   [TSLint](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-typescript-tslint-plugin)
    -   TSLint plugin for language support
