# Clean Code: Formatting

[Table of Contents](../CLEAN-CODE.md)

## Use Exceptions Rather than Error Codes

Error codes are a relic of languages that didn't have exceptions. Since we work with languages that do support exceptions, we do not use error codes in our applications. Code that uses error codes can be greatly simplified through the use of a `try-catch` block returning a typed exception and a descriptive error message.

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

Now without error codes.

```CSharp
public class DeviceController
{
    public void SendShutDown()
    {
        try
        {
            ShutDown();
        }
        catch (DeviceShutDownException e)
        {
            _logger.LogException(e);
        }
    }

    private void ShutDown()
    {
        DeviceHandle handle = GetHandle(DEV1);
        DeviceRecord record = GetDeviceRecord(handle);
        PauseDevice(handle);
        ClearDeviceWorkQueue(handle);
        CloseDevice(handle);
    }

    private DeviceHandle GetHandle(DeviceId id) {
        ...
        throw new DeviceShutDownException($"Invalid handle for {id.ToString}");
        ...
    }
}
```

In practice, we tend to follow stricter policy regarding when an `Exception` should be used. In the normal flow of a program, we prefer to bubble up errors through the `IResult<T>` pattern and only throw exceptions when an execution failure occurs.
