## Tokenizer GPT-3 
At present, we only offer limited functionality with the GPT-3 tokenizer. However, we plan to introduce more support in the future.

https://platform.openai.com/tokenizer Visit OpenAI website to learn more about tokenizer.

### Get Encoded token list from a string

```csharp
var encodedList = TokenizerGpt3.Encode(input)
```
### Just get estimated token usage from the string

```csharp
var encodedList = TokenizerGpt3.TokenCount("My Sample String");
```
### This will help you clean up end-of-line characters, which will make your token results more accurate.

```csharp
const string fileName = "TokenizerSample.txt";
var input = await FileExtensions.ReadAllTextAsync($"SampleData/{fileName}");
var encodedList = TokenizerGpt3.Encode(input, true);
```