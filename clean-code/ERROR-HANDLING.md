# Clean Code: Error Handling

[Table of Contents](../CLEAN-CODE.md)

## Use Exceptions Rather than Error Codes

Generally, exceptions are easier to maintain in comparison to error codes, so we prefer to use them in our applications.
Code that uses error codes can be greatly simplified through the use of a `try-catch` block returning a typed exception and a descriptive error message.

```CSharp
public class DeviceController
{
    public void SendShutDown()
    {
        DeviceHandle handle = GetHandle(DEV1);
        //Check the state of the device
        if (handle != DeviceHandle.INVALID)
        {
            // Save device status to the record field
            GetDeviceRecord(handle);
            //If not suspended, shut down
            if (record.GetStatus() != DEVICE_SUSPENDED)
            {
                PauseDevice(handle);
                ClearDeviceWorkQueue(handle);
                CloserDevice(handle);
            }
            else
            {
                _logger.Log($"Device suspended. Unable to shut down.");
            }
        }
        else
        {
            _logger.Log($"Invalid Handle for:{DEV1.ToString()}");
        }
    }
}
```

In practice, we tend to follow stricter policy regarding when an `Exception` should be used. In the normal flow of a program, we prefer to bubble up errors through the `IResult<T>` pattern and only throw exceptions when an execution failure occurs.

```CSharp
public class DeviceController
{
    public const string ERROR_KEY_DEVICE_SHUTDOWN_FAILURE = "DeviceController.Shutdown.Failure";
    ...
    private ILogger<DeviceController> _logger;
    ...

    public void SendShutDown()
    {
        var shutDownResult = ShutDown();
        if (shutDownResult.HasErrors)
        {
            _logger.LogErrors(shutDownResult.Errors);
        }
    }

    private IResult<bool> ShutDown()
    {
        var deviceHandleResult = GetHandle(DEV1);
        if (deviceHandleResult.HasErrorsOrResultIsNull())
        {
            r.AddErrors(deviceHandleResult);
            return false;
        }

        var handle = deviceHandleResult.ResultObject;
        var deviceRecordResult = GetDeviceRecord(handle);
        if (deviceRecordResult.HasErrorsOrResultIsNull())
        {
            r.AddErrors(deviceRecordResult);
            return false;
        }

        var pauseResult = PauseDevice(handle);
        if (pauseResult.HasErrors)
        {
            // Add Errors and Continue
            r.AddErrors(pauseResult);
        }

        var clearResult = ClearDeviceWorkQueue(handle);
        if (clearResult.HasErrors)
        {
            // Add Errors and Continue
            r.AddErrors(clearResult);
        }

        var closeResult = CloseDevice(handle);
        if (closeResult.HasErrors)
        {
            // Add Errors and Continue
            r.AddErrors(closeResult);
        }

        return true;
    }

    private IResult<DeviceHandle> GetHandle(DeviceId id) => Do<DeviceHandle>().Try((r) =>
    {
        ...
        throw new DeviceShutDownException($"Invalid handle for {id.ToString}");
        ...
    }).Catch((DeviceShutDownException ex, IResult<DeviceHandle> r) =>
    {
        r.AddError(key: ERROR_KEY_DEVICE_SHUTDOWN_FAILURE, message: ex.Message);
        r.ResultObject = default(DeviceHandle);
    });
}
```
