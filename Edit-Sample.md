# Create an Edit request
**Important: Edit endpoint works with only edit Models. If you use TextDavinci instead of TextEditDavinci you will get an error from the OpenAI service.**
## Sample 1
```csharp
var completionResult = await sdk.Edit.CreateEdit(new EditCreateRequest()
{
   Input = "What day of the wek is it?",
   Instruction = "Fix the spelling mistakes",
   Model = Models.TextEditDavinciV1
});
```
# Handle Response
```csharp

if (completionResult.Successful)
{
   Console.WriteLine(completionResult.Choices.FirstOrDefault());
}
else
{
   if (completionResult.Error == null)
   {
      throw new Exception("Unknown Error");
   }
   Console.WriteLine($"{completionResult.Error.Code}: {completionResult.Error.Message}");
}
```