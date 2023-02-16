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