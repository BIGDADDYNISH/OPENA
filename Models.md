# List all models
```csharp
var modelList = await sdk.Models.ListModel();
Console.WriteLine(string.Join(Environment.NewLine, modelList.Models.Select(r => r.Id)));
```
# Retrieve a Model
```csharp
foreach (var modelItem in modelList.Models)
{
    var retrievedModelResponse = await sdk.Models.RetrieveModel(modelItem.Id);
    if (retrievedModelResponse.Successful)
    {
        //Congrats
    }
}

```