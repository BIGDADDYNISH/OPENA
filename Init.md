I have tried to create different options for initializing the library so that it will be easy to implement different types of projects.
To able to start using this library you will need API key from OpenAI website:https://platform.openai.com/account/api-keys  
Also, organization Id is optional if you are using an organization account, If you need organization ID please visit here: https://platform.openai.com/account/org-settings

**Important: I will be showing different patterns of how you can keep your API key on the project. never ever do not put your APIkey directly into the project or source code. It will be a security issue for you. Bad people🕵️ may expose your API key and you will be ending up paying huge amounts of bills.**

First, let's start with a simple constructor 
```csharp
var openAiService = new OpenAIService(new OpenAiOptions()
{
    ApiKey =  Environment.GetEnvironmentVariable("MY_OPEN_AI_API_KEY")!, required
    Organization = Environment.GetEnvironmentVariable("MY_OPEN_ORGANIZATION_ID")! //optional
});
```
that's it, and you can start calling services like this
```csharp
var completionResult = await openAiService.Completions.CreateCompletion(new CompletionCreateRequest()
{
    Prompt = "Once upon a time",
    Model = Models.TextDavinciV3
});
```
###