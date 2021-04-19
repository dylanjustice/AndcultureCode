## Data Transfer Objects

A Data Transfer Object (commonly abbreviated DTO) is a specific type of data structure used for
transferring data between actors/systems, such as an entity from the database to the consumer of an API.

The same rules of thumb for regular data structures apply - don't add any functions or business logic.

Below is an example DTO and its corresponding entity. While DTOs are commonly 1:1 mapped with their
database-backed entity, they do not need to be. This is one of the arguments for DTOs - it provides
a layer to allow for data to be hidden/transformed from its original source if needed.

```cs
public class User : Auditable
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string PhoneNumber { get; set; }
    public string UserName { get; set; }
}
```

```cs
public class UserDto : AuditableDto
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string PhoneNumber { get; set; }
    public string UserName { get; set; }
}
```
