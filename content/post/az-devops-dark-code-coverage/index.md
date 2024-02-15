---
author: JB
title: Code coverage in Azure DevOps
date: 2024-02-15
description: Setting up code coverage in Azure DevOps (including dark mode)
image: "logos/azure-devops-logo.jpg"
categories: [ "testing" ]
tags: [ "azuredevops", "codecoverage" ]
---

## Running the tests and collecting the code coverage

```yaml
- task: DotNetCoreCLI@2
  displayName: 'Dotnet test'
  inputs:
    command: 'test'
    projects: '**/*Tests.csproj'
    arguments: '--configuration $(buildConfiguration) --filter "Traits!=Local&Traits!=Interactive" --collect:"XPlat Code Coverage"'
    publishTestResults: true
```

## Getting code coverage using scripts

I've been using this way to do code coverage in Azure DevOps by using the dotnet CLI to install the ReportGenerator:

```yaml
# Publish code coverage
- script: 'dotnet tool install -g dotnet-reportgenerator-globaltool'
  displayName: 'Install ReportGenerator tool'

- script: 'reportgenerator -reports:$(Agent.TempDirectory)/**/coverage.cobertura.xml -targetdir:".coverlet" -reporttypes:"HtmlInline_AzurePipelines_Dark;Cobertura;lcov" -filefilters:"" -classfilters:"" -assemblyfilters:"-*.Tests"'
  displayName: 'Run code coverage ReportGenerator'

- task: PublishCodeCoverageResults@1
  displayName: 'Publish code coverage report'
  inputs:
    codeCoverageTool: Cobertura
    summaryFileLocation: '.coverlet/Cobertura.xml'
    reportDirectory: '.coverlet'
```

## Using the ReportGenerator Azure DevOps extension

Another option is to install the Azure DevOps extension (https://marketplace.visualstudio.com/items?itemName=Palmmedia.reportgenerator)

```yaml
# Publish code coverage
- task: reportgenerator@5
  displayName: Run code coverage report generator
  inputs:
    reports: '$(Agent.TempDirectory)/**/coverage.cobertura.xml'
    targetdir: '.coverlet'
    reporttypes: 'HtmlInline_AzurePipelines_Dark;Cobertura;lcov'
    filefilters: ''
    assemblyfilters: '-*.Tests'
    classfilters: ''

- task: PublishCodeCoverageResults@1  
  displayName: Publish code coverage report
  inputs:
    codeCoverageTool: Cobertura
    summaryFileLocation: '.coverlet/Cobertura.xml'
    reportDirectory: '.coverlet'
```

## The trick to get dark mode to work

This is all well and good, but I never liked the light mode on the code coverage report as the contrast is painful on a poor developers eyes. But there is a way to get it published in dark mode. The last time I tried this I overlooked the need for the `disable.coverage.autogenerate` variable, which was required to get the `HtmlInline_AzurePipelines_Dark` theme to work:

```yaml
variables:
  disable.coverage.autogenerate: 'true'
```

Refs:
[Daniel Palme's](https://github.com/danielpalme) ReportGenerator: https://github.com/danielpalme/ReportGenerator