This library offers several options for initializing it, making it easy to use in various project types. To use this library, you need an API key from the OpenAI website. Please obtain your API key by visiting https://platform.openai.com/account/api-keys.  
The organization ID is optional. If you have an organization account, please visit https://platform.openai.com/account/org-settings to obtain the organization ID.

**Please note that it is important to keep your API key secure. Do not put your API key directly into the project or source code, as this can pose a security risk. Third parties may expose your API key and you may end up paying their bills.**

The simplest way to start using the library is through a simple constructor, which looks like this:
```csharp
var openAiService = new OpenAIService(new OpenAiOptions()
{
    ApiKey =  Environment.GetEnvironmentVariable("MY_OPEN_AI_API_KEY")!, required
    Organization = Environment.GetEnvironmentVariable("MY_OPEN_ORGANIZATION_ID") //optional
});
```
Once the constructor is implemented, you can start calling services like this:
```csharp
var completionResult = await openAiService.Completions.CreateCompletion(new CompletionCreateRequest()
{
    Prompt = "Once upon a time",
    Model = Models.TextDavinciV3
});
```
### Dependency Injection
For those who are building a web API or using a service provider in their app, dependency injection is an easy option. Simply add the openAI service to the service collection like this:
```csharp
serviceCollection.AddOpenAIService(settings => { settings.ApiKey = Environment.GetEnvironmentVariable("MY_OPEN_AI_API_KEY"); });
```
Alternatively, if you are using a settings file or user secret, you can bind your API key from there. An example of a secrets.json file looks like this:
```csharp
serviceCollection.AddOpenAIService();
```
secrets.json file:
```json
 "OpenAIServiceOptions": {
    "ApiKey":"Your api key goes here"
    "Organization": "Your Organization Id goes here (optional). If you don't have one, delete me"
  },
```
(How to use [user secret](https://docs.microsoft.com/en-us/aspnet/core/security/app-secrets?view=aspnetcore-6.0&tabs=windows) ?
Right-click your project name in "solution explorer" then click "Manage User Secret", it is a good way to keep your api keys)

Finally, to get your service whenever you need it, use the following code:
```csharp
var openAiService = serviceProvider.GetRequiredService<IOpenAIService>();
```