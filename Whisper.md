```csharp
const string fileName = "micro-machines.mp3";
var sampleFile = await FileExtensions.ReadAllBytesAsync($"SampleData/{fileName}");
var audioResult = await sdk.Audio.CreateTranscription(new AudioCreateTranscriptionRequest
{
    FileName = fileName,
    File = sampleFile,
    Model = Models.WhisperV1,
    ResponseFormat = StaticValues.AudioStatics.ResponseFormat.VerboseJson
});
if (audioResult.Successful)
{
    Console.WriteLine(string.Join("\n", audioResult.Text));
}
else
{
    if (audioResult.Error == null)
    {
        throw new Exception("Unknown Error");
    }
    Console.WriteLine($"{audioResult.Error.Code}: {audioResult.Error.Message}");
}
```