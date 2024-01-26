---
author: JB
title: Discriminated unions in C#
date: 2023-03-17
description: Using the library OneOf in C#
image: "logos/c-sharp-logo.png"
categories: [ "dotnet" ]
tags: [ "c-sharp", "f-sharp" ]
---

In F# (and other languages) we have the concept of discriminated unions. These provide support for values that can be `one of a number of named cases, possibly each with different values and types`.

So what does that mean? \
 It allows us to define a type that can be many things, but we get to be really specific about what those things are.
For example in F# we can define a type Shape like this, that can contain a value of 3 specific cases:

```fs
type Shape = 
    | Rectangle of width : float * length : float
    | Circle of radius : float
    | Prism of width : float * float * height : float
```

You can read more about this here: [Discriminated Unions - F# | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/fsharp/language-reference/discriminated-unions)

Rumors has it we will be getting something similar in C# version 12 which will probably be released at the end of the year sometime.

However; while we wait we can take advantage of the library called `OneOf`. 

This comes as a nuget package and the source code can be fetched here: https://github.com/mcintyre321/OneOf. \
(Note that it does not support true discriminated unions as there's nothing stopping you from adding the same type multiple times into the union)

So with OneOf, we can specify a method that returns `OneOf<a number of types>`. E.g. something like this:

```cs
OneOf<Sometype, NotFound, ValidationError> SomeMethod(Sometype input);
```

Here's the most common use case described by the library author. \
We have a method that returns one of 3 types and then handles the different types in an API endpoint:

```cs
// The most frequent use case is a return value, when you need to return different results from a method.
// Here's how you might use it in an MVC controller action:

public OneOf<User, InvalidName, NameTaken> CreateUser(string username)
{
    if (!IsValid(username)) return new InvalidName();
    var user = _repo.FindByUsername(username);
    if(user != null) return new NameTaken();
    var user = new User(username);
    _repo.Save(user);
    return user;
}

[HttpPost]
public IActionResult Register(string username)
{
    OneOf<User, InvalidName, NameTaken> createUserResult = CreateUser(username);
    return createUserResult.Match(
        user => new RedirectResult("/dashboard"),
        invalidName => {
            ModelState.AddModelError(nameof(username), $"Sorry, that is not a valid username.");
            return View("Register");
        },
        nameTaken => {
            ModelState.AddModelError(nameof(username), "Sorry, that name is already in use.");
            return View("Register");
        }
    );
}
```

This interesting youtube video by Nick Chapsas also shows how this can be used together with (fluent)validation in a neat way: \
[(131) How to use Discriminated Unions Today in C# - YouTube](https://www.youtube.com/watch?v=7z-xjijYfcI)

> Ref: https://github.com/mcintyre321/OneOf