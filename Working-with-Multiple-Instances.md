In some cases, you may need to use multiple instances of the OpenAIService. This can be achieved in a variety of ways. This feature is available from version 6.9.0 onwards.

## Implementation with Typed Clients
To create multiple instances, start by creating a new class that extends from the OpenAIService. You can name it according to your preference, and there is no limit on the number of such classes you can create. It's important to ensure that each class has a unique setting key.

Below is an example of creating two classes, MyOpenAIService and MyAzureOpenAIService, that extend from OpenAIService:
```csharp
public class MyOpenAIService : OpenAIService
{
    public const string SettingKey = "MyOpenAIService";
    [ActivatorUtilitiesConstructor]
    public MyOpenAIService(HttpClient httpClient, IOptionsSnapshot<OpenAiOptions> settings) : base(settings.Get(SettingKey),httpClient){}
    public MyOpenAIService(OpenAiOptions settings, HttpClient? httpClient = null) : base(settings, httpClient){}
}
public class MyAzureOpenAIService : OpenAIService
{
    public const string SettingKey = "MyAzureOpenAIService";
    [ActivatorUtilitiesConstructor]
    public MyOpenAIService(HttpClient httpClient, IOptionsSnapshot<OpenAiOptions> settings) : base(settings.Get(SettingKey),httpClient){}
    public MyOpenAIService(OpenAiOptions settings, HttpClient? httpClient = null) : base(settings, httpClient){}
}
```
### Dependency Injection
After defining your classes, prepare the dependency injection for them as shown:
```csharp
serviceCollection.AddOpenAIService<MyOpenAIService>(MyOpenAIService.SettingKey);
serviceCollection.AddOpenAIService<MyAzureOpenAIService >(MyAzureOpenAIService.SettingKey);
```
### Usage
You can then use your typed clients as follows:
```csharp
var sdk = serviceProvider.GetRequiredService<MyOpenAIService>();
```
Or inject them into a controller like this:
```csharp
public MyController(MyOpenAIService myOpenAIService, MyAzureOpenAIService myAzureOpenAIService)
{
   _myOpenAIService = myOpenAIService;
   _myAzureOpenAIService = myAzureOpenAIService;
}
```
### Configuration Settings
Ensure your settings file has been configured appropriately. Here is an example of what it might look like:
```json
{
  "OpenAIServiceOptions": {
    "MyOpenAIService": {
      "ApiKey": "sk-***Q"
    },
    "MyAzureOpenAIService": {
       "ApiKey": "3**a",
       "DeploymentId": "myDeploymentId",
       "ResourceName": "myResourceName",
       "ProviderType": "Azure"
    },
}
```
Configuration Settings
Ensure your settings file has been configured appropriately. Here is an example of what it might look like: