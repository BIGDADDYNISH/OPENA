AddOpenAIService will return an IHttpClientBuilder, which allows you to modify the HttpClient. You can easily add your proxy server using IHttpClientBuilder extensions.
```csharp
serviceCollection.AddOpenAIService()
.ConfigurePrimaryHttpMessageHandler((s => new HttpClientHandler
{
    Proxy = new WebProxy("1.1.1.1:1010"),
}));
```