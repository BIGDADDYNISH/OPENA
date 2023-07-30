## Changelog
### 7.1.3
- This release was a bit late and took longer than expected due to a couple of reasons. The future was quite big, and I couldn't cover all possibilities. However, I believe I have covered most of the function definitions (with some details missing). Additionally, I added an option to build it manually. If you don't know what I mean, you don't need to worry. I plan to cover the rest of the function definition in the next release. Until then, you can discover this by playing in the playground or in the source code. This version also support using other libraries to export your function definition.
- We now have support for functions! Big cheers to @rzubek for completing most of this feature.
- Additionally, we have made bug fixes and improvements. Thanks to @choshinyoung, @yt3trees, @WeihanLi, @N0ker, and all the bug reporters. (Apologies if I missed any names. Please let me know if I missed your name and you have a commit.) 
### 7.1.2-beta
- Bugfix https://github.com/betalgo/openai/pull/302
- Added support for Function role https://github.com/betalgo/openai/issues/303
### 7.1.0-beta
- Function Calling: We're releasing this version to bring in a new feature that lets you call functions faster. But remember, this version might not be perfectly stable and we might change it a lot later. A big shout-out to @rzubek for helping us add this feature. Although I liked his work, I didn't have enough time to look into it thoroughly. Still, the tests I did showed it was working, so I decided to add his feature to our code. This lets everyone use it now. Even though I'm busy moving houses and didn't have much time, seeing @rzubek's help made things a lot easier for me.
- Support for New Models: This update also includes support for new models that OpenAI recently launched. I've also changed the naming style to match OpenAI's. Model names will no longer start with 'chat'; instead, they'll start with 'gpt_3_5' and so on.
### 7.0.0
- The code now supports .NET 7.0. Big cheers to @BroMarduk for making this happen.
- The library now automatically disposes of the Httpclient when it's created by the constructor. This feature is thanks to @BroMarduk.
- New support has been added for using more than one instance at the same time. Check out this [link](https://github.com/betalgo/openai/wiki/Working-with-Multiple-Instances) for more details. Thanks to @remixtedi for bringing this to my attention.
- A lot of small improvements have been done by @BroMarduk.
- **Breaking Changes** 😢
  - I've removed 'GPT3' from the namespace, so you might need to modify some aspects of your project. But don't worry, it's pretty simple! For instance, instead of writing `using OpenAI.GPT3.Interfaces`, you'll now write `using OpenAI.Interfaces`.
  - The order of the OpenAI constructor parameters has changed. It now takes 'options' first, then 'httpclient'.
    ```csharp
	//Before
	var openAiService = new OpenAIService(httpClient, options);
	//Now
	var openAiService = new OpenAIService(options, httpClient);
	```
### 6.8.6
- Updated Azure OpenAI default API version to the preview version to support ChatGPT. thanks to all [issue reporters](https://github.com/betalgo/openai/issues/181)
- Added support for an optional chat `name` field. thanks to @shanepowell
- Breaking Change
   - `FineTuneCreateRequest.PromptLossWeight` converto to float thanks to @JohnJ0808
### 6.8.5
- Mostly bug fixes
- Fixed Moderation functions. https://github.com/betalgo/openai/issues/214 thanks to @scolmarg @AbdelAzizMohamedMousa @digitalvir
- Added File Stream support for Whisper, Thanks to @Swimburger 
- Fixed Whisper default response type, Thanks to @Swimburger 
- Performance improvements and code clean up,again Thanks to @Swimburger 👏
- Code clenaup, Thanks to @WeihanLi
### 6.8.4
- Released update message about nuget Package ID change
### 6.8.3
- **Breaking Changes**: 
    - ~~I am going to update library namespace from `Betalgo.OpenAI.GPT3` to `OpenAI.GPT3`. This is the first time I am trying to update my nuget packageId. If something broken, please be patient. I will be fixing it soon.~~
    Reverted namespace change, maybe next time.
    - Small Typo change on model name `Model.GPT4` `to Model.GPT_4`

    - `ServiceCollection.AddOpenAIService();` now returns `IHttpClientBuilder` which means it allows you to play with httpclient object. Thanks for all the reporters and @LGinC.
    Here is a little sample
```csharp
ServiceCollection.AddOpenAIService()
.ConfigurePrimaryHttpMessageHandler((s => new HttpClientHandler
{
    Proxy = new WebProxy("1.1.1.1:1010"),
});
``` 
### 6.8.1
- **Breaking Changes**: Typo fixed in Content Moderation CategoryScores, changing `Sexualminors` to `SexualMinors`. Thanks to @HowToDoThis.
- Tokenizer changes thanks to @IS4Code.
    - Performance improvement
    - Introduced a new method `TokenCount` that returns the number of tokens instead of a list.
    - **Breaking Changes**: Removed overridden methods that were basically string conversions. 
    I think these methods were not used much and it is fairly easy to do these conversions outside of the method. 
    If you disagree, let me know and I can consider adding them back.

### 6.8.0
* Added .Net Standart Support, Massive thanks to @pdcruze and @ricaun

### 6.7.3
* **Breaking change**: `ChatMessage.FromAssistance` is now `ChatMessage.FromAssistant`. Thanks to @Swimburger 
* The Tokenizer method has been extended with `cleanUpCREOL`. You can use this option to clean up Windows-style line endings. Thanks to @gspentzas1991

### 6.7.2
* Removed Microsoft.AspNet.WebApi.Client dependency
* The action build device has been updated to ubuntu due to suspicions that the EOL of the vocab.bpe file had been altered in the last few Windows builds.
* Added support for TextEmbeddingAdaV2 Model.
 
### 6.7.1
* Introduced support for Whisper.
* Grateful thanks to @shanepowell for contributing RetrieveFileContent.
* Resolved an issue that was causing problems with the tokenizer. A clean build should hopefully address this.
* Added support for skip options validation

### 6.7.0
* We all beeen waiting for this moment. Please enjoy Chat GPT API
* Added support for Chat GPT API
* Fixed Tokenizer Bug, it was not working properly.

### 6.6.8
* **Breaking Changes**
    * Renamed `Engine` keyword to `Model` in accordance with OpenAI's new naming convention.
    * Deprecated `DefaultEngineId` in favor of `DefaultModelId`.
    * `DefaultEngineId` and `DefaultModelId` is not static anymore.

* Added support for Azure OpenAI, a big thanks to @copypastedeveloper!
* Added support for Tokenizer, inspired by @dluc's https://github.com/dluc/openai-tools repository. Please consider giving the repo a star.  

These two changes are recent additions, so please let me know if you encounter any issues.
* Updated documentation links from beta.openai.com to platform.openai.com.
### 6.6.5
* Sad news, we have Breaking Changes.
    * `SetDefaultEngineId()` replaced by `SetDefaultModelId()`
    * `RetrieveModel(modelId)` will not use the default Model anymore. You have to pass modelId as a parameter.
    * I implemented Model overwrite logic.  
        * If you pass a modelId as a parameter it will overwrite the Default Model Id and object modelId  
        * If you pass your modelId in your object it will overwrite the Default Model Id
        * If you don't pass any modelId  it will use Default Model Id
        * If you didn't set a Default Model Id, SDK will throw a null argument exception 
            * Parameter Model Id > Object Model Id > Default Model Id
            * If you find this complicated please have a look at the implementation, OpenAI.SDK/Extensions/ModelExtension.cs -> ProcessModelId()
* New Method introduced:  GetDefaultModelId();
* Some name changes about the legacy `engine` keyword with the new `model` keyword
* Started to use the latest Completion endpoint. This expecting to solve finetuning issues. Thanks to @maiemy and other reporters.
### 6.6.4
* Bug-fix, ImageEditRequest.Mask now is optional. thanks to @hanialaraj 
*(if you are using edit request without mask your image has to be RGBA, RGB is not allowed)*

### 6.6.3
* Bug-fix, now we are handling logprops response properly, thanks to @KosmonikOS
* Code clean-up, thanks to @KosmonikOS

### 6.6.2
* Bug-fix,added jsonignore for `stop` and `stopAsList`, thanks to @Patapum 
    
### 6.6.1
* **Breaking change**. 
    * `EmbeddingCreateRequest.Input` was a ***string list*** type now it is a ***string*** type.  
    I have introduced `InputAsList` property instead of `Input`. You may need to update your code according the change.  
    ***Both Input(string) and InputAsList(string list) avaliable for use***

* Added string and string List support for some of the propertis.
    * CompletionCreateRequest --> Prompt & PromptAsList / Stop & StopAsList 
    * CreateModerationRequest --> Input & InputAsList 
    * EmbeddingCreateRequest --> Input & InputAsList
    
### 6.6.0
* Added support for new models (davinciv3 & edit models)
* Added support for Edit endpoint.
* (*Warning*: edit endpoint works with only some of the models, I couldn't find documentation about it, please follow the thread for more information: https://community.openai.com/t/is-edit-endpoint-documentation-incorrect/23361 )
* Some objects were created as class instead of record at last version. I change them to record. This will be breaking changes for some of you.
* With this version I think we cover all of openai APIs 
* In next version I will be focusing on code cleanup and refactoring. 
* If I don't need to relase bug-fix for this version also I will be updating library with dotnet 7 in next version as I promised.
### 6.5.0
* OpenAI made a surprise release yesterday and they have announced DALL·E API. I needed to do other things but I couldn't resist. Because I was rushing, some methods and class names may will change in the next release. Until that day, enjoy your creative AI.  
* **This library now fully support all DALL·E features**.
* I tried to complete Edit API too bu unfortunately something was wrong with the documentation, I need to ask some questions in the community forum.
### 6.4.1
* Bug-fixes 
    * FineTuneCreateRequest suffix json property name changed "Suffix" to "suffix"
    * CompletionCreateRequest user json property name changed "User" to "user" (Thanks to @shaneqld), also now it is a nullable string 
### 6.4.0
* I have good news and bad news
* Moderation feature implementation is done. Now we support Moderation.
* Updated some request and response models to catch up with changes in OpenAI API
* New version has some breaking changes. Because we are in the fall season I needed to do some cleanup. Sorry for breaking changes but most of them are just renaming. I believe they can be solved before your coffee finish.
* I am hoping to support Edit Feature in the next version.
### 6.3.0
* Thanks to @c-d and @sarilouis for their contributions to this version.
* Now we support Embedding endpoint. Thanks to @sarilouis
* Bug fixes and updates for Models
* Code clean-up
### 6.2.0
* Removed deprecated Answers, Classifications, and Search endpoints https://community.openai.com/t/answers-classification-search-endpoint-deprecation/18532. They will be still available until December at web-API. If you still need them please do not update to this version.
* Code clean-up
### 6.1.0
* Organization id is not a required value anymore, Thanks to @samuelnygaard
* Removed deprecated Engine Endpoint and replaced it with Models Endpoint. Now Model response has more fields.
* Regarding OpenAI Engine naming, I had to rename Engine Enum and static fields. They are quite similar but you have to replace them with new ones. Please use Models class instead of Engine class.
* To support fast engine name changing I have created a new Method, `Models.ModelNameBuilder()` you may consider using it.