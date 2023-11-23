---
author: JB
title: Snapshot testing with Verify
date: 2023-02-10
description:
image: "logos/net-logo.png"
categories: [ ".net", "testing" ]
tags: [ "c-sharp", "verify" ]
---

Ref: https://github.com/VerifyTests/Verify


I have used this in the past and it's pretty cool as you can compare complex data models, like JSON objects (it does serializing for you) during assert and you can ignore fields that doesn't matter. I think it even ignores fields that typically changes (like guid's) by default...

![Verify Example](verify-example.png)