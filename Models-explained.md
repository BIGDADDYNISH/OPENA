Since I began working on this library, I have found that the models are the most frequently modified part of the API. As a result, there are several ways to pass models to OpenAI. Although it may seem complicated, I will explain each method in detail.
## Passing Models
### Basic way
```csharp
openAiService.SetDefaultModelId(Models.Davinci);
var completionResult = await openAiService.Completions.CreateCompletion(new CompletionCreateRequest()
{
    Prompt = "Once upon a time",
    Model = Models.TextDavinciV3
});
```
### Setting Default model
```csharp
openAiService.SetDefaultModelId(Models.TextDavinciV3);
//Because you set the default model you don't need to specify again in the request object
var completionResult = await openAiService.Completions.CreateCompletion(new CompletionCreateRequest()
{
    Prompt = "Once upon a time",
});
```
What if I set a different model in the object after setting the default model? The new model will overwrite the default model for that specific request, but the default model will still be in effect for all other requests.
```csharp
openAiService.SetDefaultModelId(Models.TextDavinciV3);
//This request's model will be TextAdaV1
var completionResult = await openAiService.Completions.CreateCompletion(new CompletionCreateRequest()
{
    Prompt = "Once upon a time",
    Model = Models.TextAdaV1
});
```
### Parameter way
Is there another way? Yes, there is. However, I don't recommend using this method as it is primarily for backward compatibility.
```csharp
//This request's model will be TextCurieV1. (TextAdaV1 will have no effect)
var completionResult = await openAiService.Completions.CreateCompletion(new CompletionCreateRequest()
{
    Prompt = "Once upon a time",
    Model = Models.TextAdaV1
}, Models.TextCurieV1);
```

### Final word
Default Model < Model in Request Object Body <Parameter (try to not use Parameter) 
## Model Names
Model names are becoming increasingly complex with each passing day. All model parameters in the library are currently strings. To address this issue, I have created OpenAI.GPT3.ObjectModels.Models class, which contains a static string collection of models. Since OpenAI updates their models frequently, I may not be able to keep up with the pace or you may not want to update the library every time a new model is released. As a result, feel free to use the static string collection or simply provide the model name as a text string.

```csharp
var completionResult = await openAiService.Completions.CreateCompletion(new CompletionCreateRequest()
{
    Prompt = "Once upon a time",
    Model = Models.TextDavinciV3
});
```
or
```csharp
var completionResult = await openAiService.Completions.CreateCompletion(new CompletionCreateRequest()
{
    Prompt = "Once upon a time",
    Model = "text-davinci-003"
});
```
