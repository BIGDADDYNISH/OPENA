# Create a completion request 
## Sample 1
```csharp
var completionResult = await openAiService.Completions.CreateCompletion(new CompletionCreateRequest()
{
    Prompt = "Once upon a time",
    Model = Models.TextDavinciV3
});
```
## Sample 2
```csharp
var completionResult = await sdk.Completions.CreateCompletion(new CompletionCreateRequest()
{
    Prompt = "Once upon a time"
}, Models.TextDavinciV3);
```
# Handle Response
```csharp
if (completionResult.Successful)
{
    Console.WriteLine(completionResult.Choices.FirstOrDefault());
}
else //handle errors
{
    if (completionResult.Error == null)
    {
        throw new Exception("Unknown Error");
    }
    Console.WriteLine($"{completionResult.Error.Code}: {completionResult.Error.Message}");
}
```