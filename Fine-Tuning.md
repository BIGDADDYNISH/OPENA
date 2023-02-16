Sample fine-tuning file is available [here](https://github.com/betalgo/openai/blob/master/OpenAI.Playground/SampleData/FineTuningSample1.jsonl)
```csharp
try
{
    const string fileName = "FineTuningSample1.jsonl";
    var sampleFile = await File.ReadAllBytesAsync($"SampleData/{fileName}");

    ConsoleExtensions.WriteLine($"Uploading file {fileName}", ConsoleColor.DarkCyan);
    var uploadFilesResponse = await sdk.Files.FileUpload(UploadFilePurposes.UploadFilePurpose.FineTune, sampleFile, fileName);
    if (uploadFilesResponse.Successful)
    {
        ConsoleExtensions.WriteLine($"{fileName} uploaded", ConsoleColor.DarkGreen);
    }
    else
    {
        ConsoleExtensions.WriteLine($"{fileName} failed", ConsoleColor.DarkRed);
    }

    var createFineTuneResponse = await sdk.FineTunes.CreateFineTune(new FineTuneCreateRequest()
    {
        TrainingFile = uploadFilesResponse.Id,
        Model = Models.Ada
    });

    var listFineTuneEventsStream = await sdk.FineTunes.ListFineTuneEvents(createFineTuneResponse.Id, true);
    using var streamReader = new StreamReader(listFineTuneEventsStream);
    while (!streamReader.EndOfStream)
    {
        Console.WriteLine(await streamReader.ReadLineAsync());
    }

    FineTuneResponse retrieveFineTuneResponse;
    do
    {
        retrieveFineTuneResponse = await sdk.FineTunes.RetrieveFineTune(createFineTuneResponse.Id);
        if (retrieveFineTuneResponse.Status == "succeeded" || retrieveFineTuneResponse.Status == "cancelled" || retrieveFineTuneResponse.Status == "failed")
        {
            ConsoleExtensions.WriteLine($"Fine-tune Status for {createFineTuneResponse.Id}: {retrieveFineTuneResponse.Status}.", ConsoleColor.Yellow);
            break;
        }

        ConsoleExtensions.WriteLine($"Fine-tune Status for {createFineTuneResponse.Id}: {retrieveFineTuneResponse.Status}. Wait 10 more seconds", ConsoleColor.DarkYellow);
        await Task.Delay(10_000);
    } while (true);

    do
    {
        var completionResult = await sdk.Completions.CreateCompletion(new CompletionCreateRequest()
        {
            MaxTokens = 1,
            Prompt = @"https://t.co/f93xEd2 Excited to share my latest blog post! ->",
            Model = retrieveFineTuneResponse.FineTunedModel,
            LogProbs = 2
        });
        if (completionResult.Successful)
        {
            Console.WriteLine(completionResult.Choices.FirstOrDefault());
            break;
        }
        else
        {
            ConsoleExtensions.WriteLine($"failed{completionResult.Error?.Message}", ConsoleColor.DarkRed);
        }
    } while (true);
}
catch (Exception e)
{
    Console.WriteLine(e);
    throw;
}

```