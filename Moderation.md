## Sample 1
```csharp
var moderationResponse = await sdk.Moderation.CreateModeration(new CreateModerationRequest()
{
   Input = "I want to kill them."
});

if (moderationResponse.Results.FirstOrDefault()?.Flagged != true)
{
   ConsoleExtensions.WriteLine("Create Moderation test failed", ConsoleColor.DarkRed);
}

ConsoleExtensions.WriteLine("Create Moderation test passed.", ConsoleColor.DarkGreen);
```