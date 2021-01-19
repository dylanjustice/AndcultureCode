# Clean Code: Functions

Functions are the first line of organization in any program. Lets prioritize writing them well.

[Table of Contents](../CLEAN-CODE.md)

## Small!

-   [TODO: Issue #33 - Book Club: Document our use of "Clean Code: Functions - Small!"](https://github.com/AndcultureCode/AndcultureCode/issues/33)
-   book = strive for max of 20, us = we will use as general guideline, but not hard requirement
-   by following other principles, helps get toward small

## Do one thing

-   [TODO: Issue #34 - Book Club: Document our use of "Clean Code: Functions - Do one thing"](https://github.com/AndcultureCode/AndcultureCode/issues/34)
-   naming concept - follow what function does
-   code smell if you are adding “and”, “or”... to your names
-   another code smell, boolean parameters as immediate branches of logic

## One level of abstraction per function

-   [TODO: Issue #35 - Book Club: Document our use of "Clean Code: Functions - One level of abstraction per function"](https://github.com/AndcultureCode/AndcultureCode/issues/35)
-   agree and perhaps mention it in documentation with example being it isn’t something many are thinking about

## Switch statements

-   [TODO: Issue #36 - Book Club: Document our use of "Clean Code: Functions - Switch Statements"](https://github.com/AndcultureCode/AndcultureCode/issues/36)
-   if necessary, should live at lowest level of abstraction that is “appropriate”
-   example: reducers, events, “command-pattern”
-   avoid business rules
-   if, if else, if else, else (same goes here) - worse form
-   try to avoid by using other rules

## Use descriptive names

-   [TODO: Issue #37 - Book Club: Document our use of "Clean Code: Functions - Use descriptive names"](https://github.com/AndcultureCode/AndcultureCode/issues/37)
-   spend time choosing a name
-   long names are okay, when married with other naming concepts
-   use context when appropriate

## Function arguments

-   [TODO: Issue #38 - Book Club: Document our use of "Clean Code: Functions - Function arguments"](https://github.com/AndcultureCode/AndcultureCode/issues/38)
-   largely agree with goal to use options bags more (4 around), but hard fast rule
-   function and arguments create verb-noun pair

While an _ideal_ numer of function arguments is zero (0), it's more often necessary to encapsulate a small amount of decision making into a function.
Keeping the function small (See [Small!](#Small!)) as well as making functions flexible enought to provide clear value in a handful of situations is preferable over creating hyper-specific functions in hopes of targeting a single outcome. Remember your audience, if the reader cannot make sense of the function given the arguments, it's likely a candidate for refactoring.

When functions approach four (4) arguments, consider using an argument object or an options bag. The goal of this refactor is to group like arguments into concepts of their own, and/or increase clarity for the consumer.

```TypeScript
// Before
export default function useMyBookmarks(
    collectionId: number | undefined = undefined,
    searchText: string | undefined = undefined,
    colors: Array<UserBookmarkColors> | undefined = undefined,
    filterByCurrentUser: boolean = false,
    sortBy: UserBookmarkSortOption | undefined = undefined,
    skip: number | undefined = undefined,
    take: number | undefined = undefined,
    loadSections: boolean = true,
    loadPublications: boolean = true,
    loadCollections: boolean = true,
    loadUsers: boolean = true,
    onLoadError: () => void = defaultErrorHandler
) { ... }

// After
export interface UserBookmarkIndexParams {
    collectionId?: number,
    searchText?: string,
    sortBy?: userBookmarkSortOption,
    skip?: number,
    take?: number
}

export interface UserBookmarkIncludeProperties {
    sections: boolean,
    publications: boolean,
    collections: boolean,
    users: boolean
}

export default function useMyBookmarks(
    apiParams: UserBookmarkIndexParams,
    includeProperties: UserBookmarkIncludeProperties,
    onLoadError: () => void = defaultErrorHandler
) {...}


```

When naming arguments, choose names that create a verb-noun pair between the function and the argument. The function should be a name reflecting a verb, and the arguments a noun.

```TypeScript
function assertExpectedEqualsActual(expected: any, actual: any)
```

## Have no side effects

-   [TODO: Issue #39 - Book Club: Document our use of "Clean Code: Functions - Have no side effects"](https://github.com/AndcultureCode/AndcultureCode/issues/39)
-   even with private methods, try your best to make it transparent how your data is passed and manipulated.
-   “pure function” minded though OOP

## Command Query Separation

-   [TODO: Issue #40 - Book Club: Document our use of "Clean Code: Functions - Command Query Separation"](https://github.com/AndcultureCode/AndcultureCode/issues/40)
-   do something OR answer something (NOT BOTH)

## Prefer exceptions to returning error codes

-   [TODO: Issue #41 - Book Club: Document our use of "Clean Code: Functions - Prefer exceptions to returning error codes"](https://github.com/AndcultureCode/AndcultureCode/issues/41)
-   largely antiquated
-   extracting the “try” portion is too far, but understand shared “catches”

## Structured Programming

-   [TODO: Issue #42 - Book Club: Document our use of "Clean Code: Functions - Structured Programming"](https://github.com/AndcultureCode/AndcultureCode/issues/42)
-   if functions are small, breaks/continues/next are “okay” - isn’t hard an fast

## How Do You Write Functions Like This?

-   [TODO: Issue #43 - Book Club: Document our use of "Clean Code: Functions - How Do You Write Functions Like This?"](https://github.com/AndcultureCode/AndcultureCode/issues/43)
-   unless absolutely necessary, try to do first pass refactor (per rules above)
-   review your own “PR/commit”
-   try your best to pair up on a second pass from varied audience (ideally different team)
