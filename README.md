# Asynclass

Asynclass is a library that allows the developer to create projects using asynchronous constructors, following a `Config-Init-Catch` pattern. The main objective is to expand asynchronous driven development to cover OOP concepts concisely.

> **Warning**
>
> This project is in development and **should not** be used in production due to unpredictable behaviors

## Basic usage

With Asynclass, it is possible to create async constructors that can be called using the `new` keyword. This allows asynchronous methods and features to be executed directly on the object's construction, acting as a "sugar" to reduce code, improve readability and make new ways to develop programs.

**Async classes can be called as the following:**

```cs
var currentWeather = await new WeatherInfo(DateTime.Now);

Console.WriteLine($"The current temperature is {currentWeather.Temperature}"); 
Console.WriteLine($"The current wind speed is {currentWeather.WindSpeed}"); 
```

**Creating classes using the `Config-Init-Catch` pattern:**

```cs
using Asynclass;
using System;

class WeatherInfo : Async<WeatherInfo> 
{
    public double Temperature { get; set; }
    public double WindSpeed { get; set; }
    
    public WeatherInfo(DateTime dateTime) 
    {
        Config(options => 
        {
            options.ThrowOnError = false;
            options.RetryTimes = 5;
        });

        Init(async instance => 
        {
            var fullWeatherData = await _someWeatherProvider.GetFullData(dateTime);
            
            instance = fullWeatherData.temperature;
            instance = fullWeatherData.windSpeed;
        });
        
        Catch(errors => 
        {
            errros.ForEach(e => Console.WriteLine(e.Message));
            Environment.Exit(0);
        });
    }
}
```
