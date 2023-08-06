Code samples may appear a bit lengthy for function calls, but that does not necessarily mean they are complicated. We have two different samples. The first one demonstrates basic usage in a couple of different ways. The second example utilizes an experimental utility library, which is fairly easy to use, but as mentioned, it is still experimental.

# Example 1: 
```csharp
// example taken from:
// https://github.com/openai/openai-cookbook/blob/main/examples/How_to_call_functions_with_chat_models.ipynb
var fn1 = new FunctionDefinitionBuilder("get_current_weather", "Get the current weather")
    .AddParameter("location", PropertyDefinition.DefineString("The city and state, e.g. San Francisco, CA"))
    .AddParameter("format", PropertyDefinition.DefineEnum(new List<string> {"celsius", "fahrenheit"}, "The temperature unit to use. Infer this from the users location."))
    .Validate()
    .Build();
var fn2 = new FunctionDefinitionBuilder("get_n_day_weather_forecast", "Get an N-day weather forecast")
    .AddParameter("location", new PropertyDefinition {Type = "string", Description = "The city and state, e.g. San Francisco, CA"})
    .AddParameter("format", PropertyDefinition.DefineEnum(new List<string> {"celsius", "fahrenheit"}, "The temperature unit to use. Infer this from the users location."))
    .AddParameter("num_days", PropertyDefinition.DefineInteger("The number of days to forecast"))
    .Validate()
    .Build();
var fn3 = new FunctionDefinitionBuilder("get_current_datetime", "Get the current date and time, e.g. 'Saturday, June 24, 2023 6:14:14 PM'")
    .Build();
var fn4 = new FunctionDefinitionBuilder("identify_number_sequence", "Get a sequence of numbers present in the user message")
    .AddParameter("values", PropertyDefinition.DefineArray(PropertyDefinition.DefineNumber("Sequence of numbers specified by the user")))
    .Build();
try
{
    ConsoleExtensions.WriteLine("Chat Function Call Test:", ConsoleColor.DarkCyan);
    var completionResult = await sdk.ChatCompletion.CreateCompletion(new ChatCompletionCreateRequest
    {
        Messages = new List<ChatMessage>
        {
            ChatMessage.FromSystem("Don't make assumptions about what values to plug into functions. Ask for clarification if a user request is ambiguous."),
            ChatMessage.FromUser("Give me a weather report for Chicago, USA, for the next 5 days.")
        },
        Functions = new List<FunctionDefinition> {fn1, fn2, fn3, fn4},
        // optionally, to force a specific function:
        // FunctionCall = new Dictionary<string, string> { { "name", "get_current_weather" } },
        MaxTokens = 50,
        Model = Models.Gpt_3_5_Turbo
    });
    /*  expected output along the lines of:
     
        Message:
        Function call:  get_n_day_weather_forecast
          location: Chicago, USA
          format: celsius
          num_days: 5
    */
    if (completionResult.Successful)
    {
        var choice = completionResult.Choices.First();
        Console.WriteLine($"Message:        {choice.Message.Content}");
        var fn = choice.Message.FunctionCall;
        if (fn != null)
        {
            Console.WriteLine($"Function call:  {fn.Name}");
            foreach (var entry in fn.ParseArguments())
            {
                Console.WriteLine($"  {entry.Key}: {entry.Value}");
            }
        }
    }
    else
    {
        if (completionResult.Error == null)
        {
            throw new Exception("Unknown Error");
        }
        Console.WriteLine($"{completionResult.Error.Code}: {completionResult.Error.Message}");
    }
}
catch (Exception e)
{
    Console.WriteLine(e);
    throw;
}
```

# Example 2:
For this example you also need to use `Betalgo.OpenAI.Utilities` library

```csharp
var calculator = new Calculator();
var req = new ChatCompletionCreateRequest
{
    //Functions = FunctionCallingHelper.GetFunctionDefinitions(calculator),
    //Functions = FunctionCallingHelper.GetFunctionDefinitions(typeof(Calculator)),
    Functions = FunctionCallingHelper.GetFunctionDefinitions<Calculator>(),
    Messages = new List<ChatMessage>
    {
        ChatMessage.FromSystem("You are a helpful assistant."),
        ChatMessage.FromUser("What is 2 + 2 * 6?") // GPT4 is needed for this
        //ChatMessage.FromUser("What is 2 + 6?"),  // GPT3.5 is enough for this
    }
};
do
{
    var reply = await openAIService.ChatCompletion.CreateCompletion(req, Models.Gpt_4_0613);
    if (!reply.Successful)
    {
        Console.WriteLine(reply.Error?.Message);
        break;
    }
    var response = reply.Choices.First().Message;
    if (response.FunctionCall != null)
    {
        Console.WriteLine($"Invoking {response.FunctionCall.Name} with params: {response.FunctionCall.Arguments}");
    }
    else
    {
        Console.WriteLine(response.Content);
    }
    req.Messages.Add(response);
    if (response.FunctionCall != null)
    {
        var functionCall = response.FunctionCall;
        var result = FunctionCallingHelper.CallFunction<float>(functionCall, calculator);
        response.Content = result.ToString(CultureInfo.CurrentCulture);
    }
} while (req.Messages.Last().FunctionCall != null);

public class Calculator
{
    public enum AdvancedOperators
    {
        Multiply,
        Divide
    }
    [FunctionDescription("Adds two numbers.")]
    public float Add(float a, float b)
    {
        return a + b;
    }
    [FunctionDescription("Subtracts two numbers.")]
    public float Subtract(float a, float b)
    {
        return a - b;
    }
    [FunctionDescription("Performs advanced math operators on two numbers.")]
    public float AdvancedMath(float a, float b, AdvancedOperators advancedOperator)
    {
        return advancedOperator switch
        {
            AdvancedOperators.Multiply => a * b,
            AdvancedOperators.Divide => a / b,
            _ => throw new ArgumentOutOfRangeException(nameof(advancedOperator), advancedOperator, null)
        };
    }
}

```