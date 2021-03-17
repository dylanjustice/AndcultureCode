# Clean Code: Formatting

[Table of Contents](../CLEAN-CODE.md)

## Vertical Formatting

### General

https://github.com/AndcultureCode/AndcultureCode/issues/67

### Ordering

https://github.com/AndcultureCode/AndcultureCode/issues/68

## Horizontal Formatting

### General

Strive to maintain a 100 character width maximum per line (shorter is typically better). Ideally,
this would be enforced by an auto-formatter or linting tools, but lines that are hard to read due to
length can also be called out in code reviews.

### Alignment

Horizontal alignment is nice to look at, but it can take away from the true intent of your code.

```CSharp
 var pasteValidationResult = _pasteValidateConductor.Validate(
     contentAreaVersionComponentId:       id,
     courseId:                            dto.CourseId,
     sourceContentAreaVersionComponentId: dto.SourceId
);
```

If a list of declarations is more easily read when aligned horizontally, the chances are you should consider breaking up the list into another class, etc.

Furthermore, we highly encourage you and your team should consider the use of automated formatting tools to standardize and simplify your formatting practice.

```CSharp
var pasteValidationResult = _pasteValidateConductor.Validate(
    contentAreaVersionComponentId: id,
    courseId: dto.CourseId,
    sourceContentAreaVersionComponentId: dto.SourceId
);
```

### Indentation

https://github.com/AndcultureCode/AndcultureCode/issues/71

### Team Rules

We value consistency across our projects. While we will always have disagreements about
conventions, all project teams should up hold conventions until the team has agreed to make a change.

Team members should strive to make their code indistinguishable. We could go as far as to say it is a
code smell if you can tell who authored a piece of code.

That said, these documents are in no way set in stone. Proposals to change our practices are welcomed
and very much expected.
