# Clean Code: Boundaries

Best practices for keeping the boundaries in our code clean.

[Table of Contents](../CLEAN-CODE.md)

### Using Third-Party Code

Third party software provides a great example of boundary setting within our applications.
Their interfaces fulfill a plethora of use cases well beyond our needs.
It is important to set boundaries around these actors through use of small interfaces,
encapsulation and other software design best practices.

#### Example: Remote file storage on S3

Take `IAmazonS3` for example, even a small sampling of this massive 12,000+ line interface
demonstrates the need to set boundaries.

```csharp
public interface IAmazonS3
{
    CopyObjectResponse CopyObject(...);
    DeleteBucketResponse DeleteBucket(...);
    DeleteBucketOwnershipControlsResponse DeleteBucketOwnershipControls(...);
    PutPublicAccessBlockResponse PutPublicAccessBlock(...);
    RestoreObjectResponse RestoreObject(...);
}
```

Passing such an interface throughout our system is outright dangerous. Most applications
have no need to delete entire S3 buckets, set permissions or more fine grained aspects unique
to S3. Additionally, this couples our application to terms and actors very specific to this
particular storage provider. Suppose we want to do local file storage or Azure blob storage?

Instead we should aim to create more focused interfaces.

```csharp
public interface IStorageProvider
{
    string Name { get; } // ie. AmazonS3, AzureBlob, Local, etc...

    IResult<bool> CopyFile(string sourcePath, string destinationPath);
    IResult<bool> DeleteFile(string path);
    IResult<string> GetFileContents(string path);
    IResult<bool> MoveFile(string sourcePath, string destinationPath);
}

public class AmazonS3StorageProvider : IStorageProvider
{
    public AmazonS3StorageProvider(IAmazonS3 client)
    {
        _client = client;
    }
    // ...
}

public class AzureBlobStorageProvider : IStorageProvider
{
    public AzureBlobStorageProvider(BlobClient client)
    {
        _client = client;
    }
    // ...
}
```

#### The REAL challenge

The challenge comes when abstracting this exercise to the actors _WE_ create across the
layers of our application. Whether it be across the literal boundaries of an assembly/module or
the more challenging to conceptualize boundary of a design pattern such as controllers, models,
conductors and so forth. One actor we create is often the "third party" to another actor across the
system that is also under our control.

Just because we _currently_ have control of the code or assembly, does not mean it might not later
be shared or our customer's needs change. If we don't take necessary actions up front to design
small and well-designed interfaces/APIs now, we can find ourselves standing at the foot of an
insurmountable mountain of refactoring and risky deployments later when those things inevitably
change.

### Learning Boundaries & Tests

It is common to write some test code when exploring a new library - it's a quick, easy way to familiarize
yourself with the API. They can also be a valuable tool for ensuring expected behavior of the library
is maintained between releases.

While you shouldn't unit test 100% of the library's API, there are certain scenarios where you'd
like to be notified with a failing test if an upgrade changes the assumptions your code is written with.

For example, `immutable`, the library we use for constructing view models on the frontend, currently
behaves in a way that requires us to pass an object with every single property set in order to be
"settable" when instantiated. Without doing so, the instance will return `undefined` for properties
not initially given, which is not obvious or expected behavior.

```ts
// interfaces/user-topic.ts
interface UserTopic {
    description?: string;
    name: string;
    userId: number;
}

// models/user-topic-record.ts
const defaultValues: UserTopic = {
    // Forgot to set "description" here, which should be okay - it's optional
    name: "",
    userId: 0,
};

class UserTopicRecord extends Record(defaultValues) implements UserTopic {}

// component.tsx
const record = new UserTopicRecord({
    name: "Other",
    description: "Miscellaneous items",
    userId: 123,
});

console.log(record.description); // Outputs `undefined`
```

To verify this behavior, we can write a test (example from [`AndcultureCode.JavaScript.Core`](https://github.com/AndcultureCode/AndcultureCode.JavaScript.Core) -> `record-utils.test.ts`)

```ts
test("when property values not provided in class definition, returns record without properties set", () => {
    // Arrange

    // Because we're not sending through values (even undefined) to the class definition
    // they will not be set even when supplied during construction. If immutable ever
    // changes this behavior, this test should let us know
    class UserRecord extends Record({}) implements User {}

    // Act
    const sut = new UserRecord({
        createdById: TestUtils.faker.datatype.number(),
        createdOn: TestUtils.faker.datatype.string(),
        deletedById: TestUtils.faker.datatype.number(),
        deletedOn: TestUtils.faker.datatype.string(),
        updatedById: TestUtils.faker.datatype.number(),
        updatedOn: TestUtils.faker.datatype.string(),
    });

    // Assert
    expect(sut.createdById).toBeUndefined();
    expect(sut.createdOn).toBeUndefined();
    expect(sut.deletedById).toBeUndefined();
    expect(sut.deletedOn).toBeUndefined();
    expect(sut.updatedById).toBeUndefined();
    expect(sut.updatedOn).toBeUndefined();
});
```

### Using Code That Does Not Yet Exist

https://github.com/AndcultureCode/AndcultureCode/issues/110

### Clean Boundaries

https://github.com/AndcultureCode/AndcultureCode/issues/111
