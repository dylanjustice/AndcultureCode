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

## Exports

For exporting functions, objects, classes, etc., we prefer named export syntax vs. default exports.
There are a few benefits of named exports over default exports:

1. Ability to export multiple actors from a single file (for example, an interface for a component's
   props as well as the component itself).

    ```TS
    // -----------------------------------------------------------------------------------------
    // #region Interfaces
    // -----------------------------------------------------------------------------------------

    interface ButtonProps {
        // disabled, id, onClick, etc. props here...
    }

    // #endregion Interfaces

    // -----------------------------------------------------------------------------------------
    // #region Component
    // -----------------------------------------------------------------------------------------

    const Button = () => {
        return (<button />);
    };

    // #endregion Component

    // -----------------------------------------------------------------------------------------
    // #region Exports
    // -----------------------------------------------------------------------------------------

    // Newer versions of TypeScript require the `type` keyword before interfaces if not exporting in-line
    export type { ButtonProps };
    export { Button };

    // #endregion Exports
    ```

2. Enforced naming when imported unless explicitly aliased

    ```TS
    // Some other file...
    import { Button } from "../button";
    import { Anchor as AndcultureCodeAnchor } from "andculturecode-javascript-react-components";
    ```

    - With default exports, the name given during the export can be completely ignored:

    ```TS
    // Same component as above, except with a default export
    export default Button;
    ```

    ```TSX
    // Some other file...
    import notAButton from "../button";
    ```

3. Consistency - it's generally easier to read and understand where things are coming from when all
   of the imports are named and aliased when appropriate.

    ```TS
    import { Button } from "../button";
    import { Anchor } from "andculturecode-javascript-react-components";
    import {
        CoreUtils as AndcultureCodeCoreUtils,
        CollectionUtils,
        RouteUtils,
    } from "andculturecode-javascript-core";
    import { CoreUtils } from "../utilities/core-utils";
    ```
