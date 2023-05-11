You can apply any rate-limiting library using HttpClientFactory. I suggest using Polly. I have shared some samples and documentation that you can follow.

https://learn.microsoft.com/en-us/dotnet/architecture/microservices/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests
https://learn.microsoft.com/en-us/aspnet/core/fundamentals/http-requests?view=aspnetcore-7.0#consumption-patterns


```csharp
ServiceCollection.AddOpenAIService()
    .AddPolicyHandler(GetRetryPolicy())
    .AddPolicyHandler(GetCircuitBreakerPolicy());
```

```csharp
serviceCollection.AddOpenAIService().AddTransientHttpErrorPolicy(policyBuilder =>
    policyBuilder.WaitAndRetryAsync(
        3, retryNumber => TimeSpan.FromMilliseconds(600)));
```