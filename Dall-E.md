## Create an Image request
### Sample Usage 1
``` csharp
var imageResult = await sdk.Image.CreateImage(new ImageCreateRequest
{
    Prompt = "Laser cat eyes",
    N = 2,
    Size = StaticValues.ImageStatics.Size.Size256,
    ResponseFormat = StaticValues.ImageStatics.ResponseFormat.Url,
    User = "TestUser"
});
```
### Sample Usage 2
``` csharp
var imageResult = await sdk.Image.CreateImage(new ImageCreateRequest
{
    Prompt = "Laser cat eyes",
    N = 2,
    Size = "256x256",
    ResponseFormat = "url",
    User = "TestUser"
});
```

## Handle Response
``` csharp
if (imageResult.Successful)
{
    Console.WriteLine(string.Join("\n", imageResult.Results.Select(r => r.Url)));
}
else //handle errors
{
    if (imageResult.Error == null)
    {
        throw new Exception("Unknown Error");
    }
    Console.WriteLine($"{imageResult.Error.Code}: {completionResult.Error.Message}");
}
```