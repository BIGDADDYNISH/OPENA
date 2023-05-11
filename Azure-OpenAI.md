Here is a sample configuration to use Azure OpenAI. Simply setting provider type to Azure will allow you to use library with Azure resources.

Azure OpenAI currently has two API versions: 2022-12-01 and 2023-03-15-preview. The 2022-12-01 version does not support ChatGpt, so 2023-03-15-preview is the default version. I suggest setting the APIVersion even if you use the default version, because I will be updating it with the latest version in the future.   
https://azure.microsoft.com/en-us/products/cognitive-services/openai-service

```
{
 "OpenAIServiceOptions": {
    "ApiKey": "35...3a", //How can I get an app key? https://azure.microsoft.com/en-us/products/cognitive-services/openai-service
    "DeploymentId": "test1", // marked as 2 in the above screenshot
    "ResourceName": "betalgotest", // marked as 1 in the above screenshot
    "ProviderType": "Azure", //Important, don't forget to add this if you want to use Azure OpenAI
    //"ApiVersion": "2022-12-01" //optional
  }
}
```
<img width="1601" alt="image" src="https://github.com/betalgo/openai/assets/1096362/e9b872aa-c00c-4cd9-b035-240afaa152e0">
