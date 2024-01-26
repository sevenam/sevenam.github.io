---
author: JB
title: Debugging IdentityServer refresh tokens
date: 2024-01-26
description: Looking up refresh tokens in the IdentityServer database
image: "logos/duende-logo.png"
categories: [ "dotnet", "security" ]
tags: [ "identityserver", "linqpad" ]
---

A refresh token returned by Duende IdentityServer should look something like this:

`F84EC044B3389D99AF31949D4884C92323AD393844DD399912F4D321AA33CC3B-1`

To be able to lookup this token in the IdentityServer `PersistedGrants` table we need to hash it with Sha256 to give it the same format as the entries in the `Key` column of this table.

That can be done with the following c# code (simple LINQPad example):

```cs
using System;
using System.Security.Cryptography;

void Main()
{
	var value = "F84EC044B3389D99AF31949D4884C92323AD393844DD399912F4D321AA33CC3B-1"; //example refresh token from IdentityServer
	var keyseparator = ":";
	var granttype = "refresh_token";

	var key = sha256_hash(value + keyseparator + granttype);
	Console.WriteLine(key);
}

public static String sha256_hash(string value)
{
	var sb = new StringBuilder();

	using (var hash = SHA256.Create())
	{
		var enc = Encoding.UTF8;
		byte[] result = hash.ComputeHash(enc.GetBytes(value));

		foreach (byte b in result)
			sb.Append(b.ToString("x2"));
	}

	return sb.ToString();
}
```

The output of the key variable can then be used to lookup a refresh token in the `PersistedGrants` table:

```sql
SELECT * FROM PersistedGrants WHERE [key] = '49f2b873bc7106926723de2861f9057d4e9b68f3871f4e6047026d9172d48f00'
```

> Refs:
> - https://duendesoftware.com/products/identityserver
> - https://www.linqpad.net