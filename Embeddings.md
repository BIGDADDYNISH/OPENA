# Create an embedding request 
## Sample 1
```csharp
var embeddingResult = await sdk.Embeddings.CreateEmbedding(new EmbeddingCreateRequest()
{
   InputAsList = new List<string> {"The quick brown fox jumped over the lazy dog."},
   Model = Models.TextSearchAdaDocV1
});
```
# Handle Response
```csharp
if (embeddingResult.Successful)
{
   Console.WriteLine(embeddingResult.Data.FirstOrDefault());
}
else
{
   if (embeddingResult.Error == null)
   {
      throw new Exception("Unknown Error");
   }
   Console.WriteLine($"{embeddingResult.Error.Code}: {embeddingResult.Error.Message}");
}
```