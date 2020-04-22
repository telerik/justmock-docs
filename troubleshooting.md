---
title: Troubleshooting
page_title: Troubleshooting | JustMock Documentation
description: Commonly encountered errors with Telerik JustMock and how to resolve them.
slug: justmock/troubleshooting
tags: troubleshooting
published: True
position: 12
previous_url: /troubleshooting.html
---

# Troubleshooting


| Symptoms | Resolution |
| ------ | ------ |
| __TypeLoadException__  <br/><br/>__Could not load type 'Telerik.JustMock.Profiler' from assembly 'mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'.__ |It is likely that there are NGENed assemblies for the /Profile scenario. Usually, this can happen when some profiler-based software tries to improve its start-up performance by NGENing system assemblies.<br/><br/>To fix this error, run *"Visual Studio Command Prompt"* or *"Developer Command Prompt"*  __as an Administrator__ and execute the following command:  <br/>`>ngen uninstall * /profile`|
| __"Unable to obtain public key for StrongNameKeyPair" exception in non-elevated tests__ |Such exception may occur if the Crypto\RSA folder does not have __Read & execute__ or __List folder contents__ permissions. To fix this you need to set them manually. The folder can be found at *%appdata%\Microsoft\Crypto\rsa* |
| __Missing Visual Studio extension or JustMock menu after successful installation__ |Try the following:<br/><br/> <ol><li>Open Visual Studio, go to __TOOLS__ -> __Extensions and Updates...__.</li><li>If JustMock is in the list of installed extensions, uninstall it from there.</li><li>Go to the extensions folder of Visual Studio:<ul><li>default location for __Visual Studio 2013__ : *C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions or C:\Program Files\Microsoft Visual Studio 12.0\Common7\IDE\Extensions*</li><li>default location for __Visual Studio 2012__ : *C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\Extensions or C:\Program Files\Microsoft Visual Studio 11.0\Common7\IDE\Extensions*</li><li>default location for __Visual Studio 2010__ : *C:\Program Files (x86)\Microsoft Visual Studio 10.0\Common7\IDE\ExtensionsorC:\Program Files\Microsoft Visual Studio 10.0\Common7\IDE\Extensions*</li></ul></li><li>Delete all folders that have __Telerik.JustMock.*__ DLLs in them (the name of the folders should follow pattern like this *"4uus3xan.ur4"* )</li><li>Run __Notepad__ as an Administrator.</li><li>Go to __File__ -> __Open__ and navigate to *C:\Program Files (x86)\Microsoft Visual Studio <10.0, 11.0 or 12.0>\Common7\IDE\Extensions* </li><li>Change the file types to *"All Files"* .</li><li>Open extensions.configurationchanged file and save it without changing anything in it( __File__ -> __Save__ ). This will refresh the list of extensions that Visual Studio uses.</li><li>Go to the JustMock install folder (default is: *C:\Program Files (x86)\Telerik\JustMock\Libraries* ).</li><li>Run __Telerik.JustMock.VS.vsix__ as administrator and proceed with the manual installation of theTelerikJustMockextension.</li><li>Check if the missing JustMock extension/menu is fixed afterwards.</li><li>If the above does not solve the issue, it is likely that you need to repair Visual Studio:<ul><li>Go to __Uninstall a Program__ from the Windows Control Panel.</li><li>Right click on __Microsoft Visual Studio 2010/2012/2013__ and select __Uninstall/Change__ .</li><li>Select __Repair/Reinstall__ from the options.</li></ul></li><ol>|
