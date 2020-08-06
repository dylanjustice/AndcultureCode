# Code Conventions > TypeScript

## Language Conventions

By default, we look to [ESLint](https://eslint.org/) as a guide before making a decision.

## Regions

When breaking a given file into logical regions the team has decided to standardize on the following
comment format. This format supports code folding while providing a header format for readability.

Check out the [typescript.code-snippets](.vscode/typescript.code-snippets) configuration for vscode snippet support.

In TypeScript, as well as other languages, we like to provide the name of the region at the start and end. This goes a long way in orienting the reader.

```TSX
// -----------------------------------------------------------------------------------------
// #region Public Methods
// -----------------------------------------------------------------------------------------

public someMethod() {}

// #endregion Public Methods
```

1. Constants
2. Enums
3. Types
4. Interfaces
5. Class / Component
    1. Private properties
    2. Protected properties
    3. Public properties
    4. Constructor(s)
    5. Public methods
    6. Protected methods
    7. Private methods
6. Exports
