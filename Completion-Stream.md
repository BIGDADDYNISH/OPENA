# Create a completion stream request 
## Sample 1
```csharp
var completionResult = sdk.Completions.CreateCompletionAsStream(new CompletionCreateRequest()
{
   Prompt = "Once upon a time",
   MaxTokens = 500, // optional
   Model = Models.Davinci
});
```
# Handle Response
```csharp
await foreach (var completion in completionResult)
{
   if (completion.Successful)
   {
      Console.Write(completion.Choices.FirstOrDefault()?.Text);
   }
   else
   {
      if (completion.Error == null)
      {
         throw new Exception("Unknown Error");
      }
      Console.WriteLine($"{completion.Error.Code}: {completion.Error.Message}");
   }
}
```