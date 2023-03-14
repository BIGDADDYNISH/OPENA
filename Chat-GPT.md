# Create a completion request 
## Sample 1
```csharp
var completionResult = await sdk.ChatCompletion.CreateCompletion(new ChatCompletionCreateRequest
{
    Messages = new List<ChatMessage>
    {
        ChatMessage.FromSystem("You are a helpful assistant."),
        ChatMessage.FromUser("Who won the world series in 2020?"),
        ChatMessage.FromAssistant("The Los Angeles Dodgers won the World Series in 2020."),
        ChatMessage.FromUser("Where was it played?")
    },
    Model = Models.ChatGpt3_5Turbo
});
if (completionResult.Successful)
{
    Console.WriteLine(completionResult.Choices.First().Message.Content);
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
## Sample 2 Stream
```csharp
var completionResult = sdk.ChatCompletion.CreateCompletionAsStream(new ChatCompletionCreateRequest
{
    Messages = new List<ChatMessage>
    {
        new(StaticValues.ChatMessageRoles.System, "You are a helpful assistant."),
        new(StaticValues.ChatMessageRoles.User, "Who won the world series in 2020?"),
        new(StaticValues.ChatMessageRoles.System, "The Los Angeles Dodgers won the World Series in 2020."),
        new(StaticValues.ChatMessageRoles.User, "Tell me a story about The Los Angeles Dodgers")
    },
    Model = Models.ChatGpt3_5Turbo,
    MaxTokens = 150//optional
});

await foreach (var completion in completionResult)
{
    if (completion.Successful)
    {
        Console.Write(completion.Choices.First().Message.Content);
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