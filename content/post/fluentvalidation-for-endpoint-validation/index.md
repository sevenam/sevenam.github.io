---
author: JB
title: FluentValidation for endpoint input validation in .net
date: 2023-03-10
description:
image: "logos/fluentvalidation-logo.png"
categories: ["dotnet"]
tags: [ "c-sharp", "fluentvalidation", "asp.net" ]
---

Ref: https://docs.fluentvalidation.net/en/latest

FluentValidation is a neat library that allows you to define validation rules using method chaining. It also allows you to hook into the asp.net core ModelState with very little plumbing, making it really easy to integrate into your existing API.

So what you need is to do is:

### Install the nuget packages you need
- FluentValidation (for writing validation code)
- FluentValidation.AspNetCore (if you want to auto-validate [ApiController] classes)
- FluentValidation.DependencyInjectionExtensions (if you want to inject the validator into any classes and invoke manual validation)

### Write your validation logic

You need to inherit your validation class from an `AbstractValidator<OfTheClassYouWantToValidate>` like shown below:

```cs
using FluentValidation;
using FluentValidationExample.Dtos;

namespace FluentValidationExample.Validators {
  public class StuffDtoValidator : AbstractValidator<StuffDto> {
    public StuffDtoValidator() {
      RuleFor(x => x.Name).NotEmpty().Length(0, 20);
    }
  }
}
```

### Tell asp.net core where to find your validators

In your `Program.cs/Startup.cs` - tell it where to find your validation classes. This can be done by pointing it to an assembly containing the validators or more easily by pointing it to a validator class like this:

```cs
// in your Program.cs or Startup.cs
builder.Services.AddValidatorsFromAssemblyContaining<StuffDtoValidator>();
```

### Tell it to do AutoValidation on controllers

It has an option to do AutoValidation on endpoints in classes with the `[ApiController]` attribute set (this unfortunately does not work with Minimal APIs). This will automatically run the validation logic for any types that are parameters for the endpoints, before the endpoint is even hit. Any validation errors will be included in the BadRequest returned by the endpoint if the validation fails. You enable AutoValidation for controllers like this:

```cs
// the following will automatically include FluentValidators into asp.net core
// and validate any models where [ApiController] is set
services.AddFluentValidationAutoValidation();
```

A controller that uses the validated `StuffDto` class, is then all set and ready to go:

```cs
[ApiController]
public class StuffController : ControllerBase
{
  [HttpPost]
  public IActionResult CreateStuff([FromBody] StuffDto stuffDto)
  {
    var stuff = mapper.Map<Stuff>(stuffDto);
    var result = stuffService.AddStuff(stuff);
    return Ok(result);
  }
}
```

If you have multiple endpoints that use the same DTO class they are all validated as soon as you add a validation class for that type. For example you might have create and update endpoints that use the same DTO as input - both will be covered!