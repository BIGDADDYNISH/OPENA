Since I began working on this library, I have found that the models are the most frequently modified part of the API. As a result, there are several ways to pass models to OpenAI. Although it may seem complicated, I will explain each method in detail.

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
//Because you set default model you don't need to specify again in the request object
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
