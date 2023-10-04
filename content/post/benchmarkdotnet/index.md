---
author: JB
title: BenchmarkDotNet
date: 2023-02-10
description: Benchmarking in .NET with BenchmarkDotNet
image: "benchmarkdotnet.png"
categories: [ ".net" ]
tags: [
    "benchmarkdotnet"
]
---

Ref: https://github.com/dotnet/BenchmarkDotNet

Sometimes it's useful to benchmark or compare some code. For dotnet one alternative is BenchmarkDotNet and here's a quick howto for you to get started.

Add nuget to your csproj file:
```xml
<PackageReference Include="BenchmarkDotNet" Version="0.13.7" />
```

If you want to e.g. compare multiple .NET versions, make sure to add the versions you want to test in the csproj file:

```xml
<TargetFrameworks>net48;net7.0;net6.0;netcoreapp3.1</TargetFrameworks>
```

Create a class for your benchmarks and add [SimpleJob] attributes for the .NET versions you want. Then tag your benchmark methods with the [Benchmark] attribute.

```cs
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Jobs;

namespace SomeBenchmarkProject
{
  [SimpleJob(RuntimeMoniker.Net60)]
  [SimpleJob(RuntimeMoniker.Net70)]
  [MemoryDiagnoser(true)] // include memory allocation results
  [MinIterationCount(15)] // min number of iterations
  [MaxIterationCount(20)] // max number of iterations
  [RankColumn] // rank the results
  [MeanColumn] // mean of the results
  [RPlotExporter] // plot results with R
  public class EfCoreBenchmarks
  {
    [Benchmark]
    public void ToListCount()
    {
      using var context = new DbContext();
      context.Employees.ToList().Count();
    }
```

Add code to execute the benchmarks like this:

```cs
using BenchmarkDotNet.Running;

BenchmarkRunner.Run<EfCoreBenchmarks>();
```

Then make sure to build for Release and Run without Debugging (Ctrl+F5).


If you also want plots, you need to install R:

```bash
winget install -e --id RProject.R
```

Add the rscript executable to your env path (E.g.: C:\Program Files\R\R-4.3.1\bin) and restart Visual Studio to pick up the environment variable change.
