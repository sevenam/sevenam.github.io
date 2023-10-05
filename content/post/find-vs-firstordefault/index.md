---
author: JB
title: Find() vs FirstOrDefault() in C#
date: 2023-08-11
description: Collection and EntityFramework performance
image: "headers/c-sharp-logo.png"
categories: [".net"]
tags: [ "c#", "sonarlint", "entityframework" ]
---

Ref: https://rules.sonarsource.com/csharp/RSPEC-6602/ \
Ref: https://learn.microsoft.com/en-us/ef/ef6/querying/

My favourite code analyser (SonarLint) told me something this week that I was not aware of: \
`List.Find()` can be faster than `IEnumerable.FirstOrDefault()` for List objects.

For small collections, the performance difference may be minor, but for large collections, it can make a noticeable difference. The same applies for ImmutableList and arrays too. `SonarLint` claims to have measured at least 2x improvement in the execution time.
Also for database queries it could make a difference as `FirstOrDefault()` will always execute the query against the database while `Find()` will try the memory db context first.

