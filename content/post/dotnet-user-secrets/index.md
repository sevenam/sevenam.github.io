---
author: JB
title: Dotnet user-secrets
date: 2023-04-14
description: Don't store secrets in your configs!
image: "logos/net-logo.png"
categories: [ "dotnet", "security" ]
tags: [ ]
---

Even locally on your dev machine there's options. \
In .NET you can replace a setting in appsettings.json with a secret from the "user-secrets" store. \
To add a secret to the user store, run the following command from the folder where appsettings.json resides:

```bash
dotnet user-secrets set "ThePath:ToTheSecret:InAppsettingsJson" "secretgoeshere"
```
