---
author: JB
title: Minimizing the pain of re-installing Visual Studio
date: 2024-08-30
description: Sometimes Visual Studio needs to be re-installed (due to new computer or change of version), but there are some tricks to make the process a bit smoother.
image: "logos/visual-studio-logo.jpg"
categories: [ ]
tags: [ "visualstudio" ]
---

### Exporting the installation configuration

To export the installation configuration from your **old** Visual Studio installation, open the `Visual Studio Installer`, click `More` and choose `Export configuration:

![Export Visual Studio Configuration 01](vs-install-config-01.png)

Select a location for the config:

![Export Visual Studio Configuration 02](vs-install-config-02.png)

You get to select what to export. You can include/exclude here and click `Export`:

![Export Visual Studio Configuration 03](vs-install-config-03.png)

The installation configuration was successfully exported:

![Export Visual Studio Configuration 04](vs-install-config-04.png)

### Importing the installation configuration

To import the installation configuration into your **new** Visual Studio installation, open `Visual Studio Installer`, click `More` and choose `Import configuration:

![Import Visual Studio Configuration 01](vs-install-config-05.png)

Select the file you previously exported as the file to import:

![Import Visual Studio Configuration 02](vs-install-config-06.png)

Make any manual adjustments if needed and then click `Modify` to start the installation process:

![Import Visual Studio Configuration 03](vs-install-config-07.png)

### Exporting Visual Studio settings

Open your **old** Visual Studio instance and go to `Tools` and `Import and Export Settings...`

![Export Visual Studio Settings 01](vs-settings-01.png)

Select `Export selected environment settings` and click `Next`:

![Export Visual Studio Settings 02](vs-settings-02.png)

Choose the settings to export and click `Next`:

![Export Visual Studio Settings 03](vs-settings-03.png)

Name the export file, choose a location and press `Finish`:

![Export Visual Studio Settings 04](vs-settings-04.png)

Click `Close` when the export is completed:

![Export Visual Studio Settings 05](vs-settings-05.png)

### Importing Visual Studio settings

Open your **new** Visual Studio instance and go to `Tools` and `Import and Export Settings...`:

![Import Visual Studio Settings 01](vs-settings-01.png)

Select `Import selected environment settings`:

![Import Visual Studio Settings 02](vs-settings-06.png)

Save your existing configuration, or just import the new settings:

![Import Visual Studio Settings 03](vs-settings-07.png)

Click `Browse` to select the settings file you already exported and click `Next`:

![Import Visual Studio Settings 04](vs-settings-08.png)

Choose which settings to import and click `Finish`:

![Import Visual Studio Settings 05](vs-settings-09.png)

Once the import is complete you'll get a review. In my case some settings could not be imported and some manual changes had to be done as well:

![Import Visual Studio Settings 06](vs-settings-0a.png)

### Keeping a backup

Generally it can be a good idea to keep a backup of settings like this.

A simple way would be to create a new git repo and commit the files there for safekeeping.
