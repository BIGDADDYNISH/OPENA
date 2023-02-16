Note: you may need to wait a bit after you upload a file. OpenAI services need to process your file.
# Create a files
```csharp
const string fileName = "SentimentAnalysisSample.jsonl";
var sampleFile = await File.ReadAllBytesAsync($"SampleData/{fileName}");  

var uploadFilesResponse = await sdk.Files.FileUpload(UploadFilePurposes.UploadFilePurpose.FineTune, sampleFile, fileName);

if (uploadFilesResponse.Successful)
{
   // Congrats
}
```
# List Files
```csharp
var uploadedFiles = await sdk.Files.ListFile();

foreach (var uploadedFile in uploadedFiles.Data)
{
   ConsoleExtensions.WriteLine($"File: {uploadedFile.FileName}", ConsoleColor.DarkCyan);
}
```
# Retrieve a File
```csharp
var retrieveFileResponse = await sdk.Files.RetrieveFile(uploadedFiles.Data.First().Id);

if (retrieveFileResponse.Successful)
{
   // Congrats
}
```
# Delete a file
```csharp
var deleteResponse = await sdk.Files.DeleteFile(uploadedFiles.Data.First().Id);

if (deleteResponse.Successful)
{
   // Congrats
}
```