# Clean Code: Error Handling

[Table of Contents](../CLEAN-CODE.md)

## Write Your Try-Catch-Finally Statement First

Failure scenarios should be thought about up front to build reliable software. Too often we are
tempted to simply write the happy-path, leaving our systems to terminate normal execution
of our applications with unhandled errors/exceptions.

Following Microsoft's recommendations to avoid exceptions for normal code flow, we draw inspiration
from their .NET Framework `TryXXX()` Pattern by using [`Do.Try`](https://andculturecode.github.io/AndcultureCode.CSharp.Core/docs/#dotry).

```csharp
public IResult<List<Role>> FindExternalRoles(long userId) => Do<List<Role>>.Try((r) =>
{
    var externalRoles = _thirdPartyProvider.FindRoles(userId);
    // additional logic around returned roles
    return externalRoles.MapToRoles();
})
.Catch(HandleAggregateException)
.Result;

public IResult<List<User>> FindExternalUsers(string searchText) => Do<List<User>>.Try((r) =>
{
    var externalUsers = _thirdPartyProvider.FindUsers(searchText);
    // additional logic around returned users
    return externalUsers.MapToUsers();
})
.Catch(HandleAggregateException)
.Result;

private void HandleAggregateException(AggregateException ex, IResult<bool> result)
    => result.AddErrorAndLog(_logger, _localizer, ERROR_EXTERNAL_API, ex);
```

Leveraging C# expression bodies, a sprinkling of functional programming and the idea that [exceptions should
be exceptional](https://mattwarren.org/2016/12/20/Why-Exceptions-should-be-Exceptional/), we aim to
catch unhandled exceptions where they occur and gracefully translate those errors in a properly
encapsulated fashion before letting them flow through or around the system to layers that would otherwise
have zero context.
