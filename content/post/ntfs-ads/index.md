---
author: JB
title: NTFS and Alternate Data Streams (ADS)
date: 2024-12-19
description: Exploring one of the more hidden sides of NTFS - Alternate Data Streams
image: "ntfs.png"
categories: [ "infosec" ]
tags: [ "powershell" ]
---

### NTFS and Alternate what?

*NTFS, the primary file system for recent versions of Windows and Windows Server, provides a full set of features including security descriptors, encryption, disk quotas, and rich metadata.* - as the [NTFS documentation](https://learn.microsoft.com/en-us/windows-server/storage/file-server/ntfs-overview) puts it. However, one feature it doesn't highlight is **Alternate Data Streams (ADS)**. In fact I had to dig quite deep to find the [official documentation for NTFS Data Streams](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-fscc/b134f29a-6278-4f3f-904f-5e58a713d2c5).

It turns out all files in NTFS are streams. That is, every file consist of at **least** one stream: the main stream. This the plain old file and it's content. The full name of the stream has the following form:

```ntfsstream
<filename>:<stream name>:<stream type>
```

As the default data stream has no name, this is how the stream name would look like for a regular **file.txt** where **$DATA** is the stream type:

```ntfsstream
file.txt::$DATA
```

### More streams

Note that a file consist of at least one stream, meaning it can actually contain more than one. After downloading a file from the Internet you may have noticed the following after checking the properties of the file:

![NTFS file security warning](ntfs-file-security-warning.png)

This warning is actually caused by an Alternate Data Stream with the name **Zone.Identifier**, that has been added to the file by the browser when you downloaded it from the Internet.

You can even add the Zone.Identifier ADS to a file yourself if you like. With Powershell you can do it like this:

```powershell
# in the example below we set the ZoneId to 3 which is 'Internet Zone'
# Ref: | 0 == Local Computer | 1 == Local Intranet Zone | 2 == Trusted Site Zone | 3 == Internet Zone | 4 == Restricted Site Zone
$filePath = ".\file.txt"
$zoneIdentifier = @"
[ZoneTransfer]
ZoneId=3
"@

Set-Content $filePath -Stream Zone.Identifier -Value $zoneIdentifier
```

If we open a cmd prompt and run `dir /r` we can see the result very clearly:

![dir /r](dir-zone-identifier.png)

We see the full name of the stream, with a filename, stream name and stream type:

```ntfsstream
<filename>:<stream name>:<stream type>

file.txt:Zone.Identifier:$DATA
```

With Powershell we can do something similar where we see it listing both the main data stream (:$DATA) and the alternate data stream Zone.Identifier:

```powershell
Get-Item .\file.txt -Stream * 

# or more similar to the dir /r command:
Get-ChildItem -Recurse | Get-Item -Stream * 
```

![Get-Item .\file.txt -Stream *](get-item.png)

To have a look at the content of the file we can of course just open the file, or we can `cat .\file.txt` to print the content to the terminal. We can do something similar to look at the content of the Zone.Identifier stream, but we need to also specify the name of the stream in addition to the file name:

```powershell
cat .\file.txt:Zone.Identifier
```

Which gives the expected result:

![cat .\file.txt:Zone.Identifier](cat-zone-identifier.png)

If we want to programmatically remove the Zone.Identifier (which is what happens if we `Unblock` the file in the Windows Explorer properties) we can do that with Powershell as well:

```powershell
Remove-Item .\file.txt -Stream Zone.Identifier

# or in previous versions of Powershell:
Remove-Item .\file.txt:Zone.Identifier
```

### The dark side of streams

Alternate Data Streams can be misused. For example, instead of storing harmless metadata, you could hide entire files within an alternate data stream.

Let's make an evil.vbs script file with one line of code, that pops up a message box:

```vbs
MsgBox "Secretly doing evil stuff in here!", vbOKCancel, "Sneaky!"
```

And let's proceed to put this as an alternate data stream for the .\file.txt:

```powershell
cat .\evil.vbs > .\file.txt:evil.vbs

# and then let's delete the vbs file so nobody will know
rm evil.vbs
```

Let's check the alternate data streams for the .vbs file:

![.\file.txt:evil.vbs](evil.png)

It's added in there along with the Zone.Identifier we added earlier. These streams are invisible in Windows Explorer, and their presence doesn't affect the visible file size. However, the hidden script can still be executed:

```powershell
wscript.exe .\file.txt:evil.vbs
```

![wscript exec](wscript-exec.png)

While Alternate Data Streams provide useful capabilities, they also pose a security risk if misused. We've seen how they can be used to try improve security by appending a Zone.Identifier, but also how they can be used for bad, by hiding rootkits or similar within files.

References:
- https://learn.microsoft.com/en-us/windows-server/storage/file-server/ntfs-overview
- https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-fscc/b134f29a-6278-4f3f-904f-5e58a713d2c5
- https://owasp.org/www-community/attacks/Windows_alternate_data_stream