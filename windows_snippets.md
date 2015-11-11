---
layout: page
title: Windows Snippets
---

A collection of Snippets, commands and other useful things for Windows that I often forget, and need to lookup. 

## PowerShell

### Recursive delete of a Folder and it's contents

    Remove-Item -Recurse -Force folderName

### Reload your profile.

    . $Profile

### PowerShell equivalent of `which`

    (Get-Command cmd).Path

NB: PowerShell 3+

### Windows Environment Commands

- %USERPROFILE% - Equivalent of ~ on *nix systems. 
- %WINDIR% - Windows Directory

## General Windows

### .Directories and .Files

To create .Directories in File Explorer append a "."" to the directory name. e.g.

    .folderName.

To create .Directories in Command Prompt or Powershell:

    mkdir .Directory

To create .Files in File Explorer append a "." to the file name; e.g.

    .fileNsame.
