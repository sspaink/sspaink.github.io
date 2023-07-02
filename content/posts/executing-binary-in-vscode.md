+++ 
draft = false
date = 2023-07-01T16:09:58-05:00
title = "Extending a vscode extension with Go"
description = "Beginners look into executing Go code from a vscode extension."
slug = ""
authors = []
tags = ['vscode', 'go']
categories = []
externalLink = ""
series = []
+++

![do something](/executing-binary-in-vscode/poke.png "Poke")

A recent project led me to investigate how to integrate a CLI written in Go into a Visual Studio Code extension. My main goal was to have all of the business logic reside in the CLI, and have the extension only act as a frontend. While I had developed a [vscode extension before](https://github.com/sspaink/kivy-vscode) I wrote it entirely in Typescript so this was an interesting new challenge. From using the [vscode-shellcheck](https://github.com/vscode-shellcheck/vscode-shellcheck) extension in the past I knew it calls the [shellcheck binary](https://github.com/koalaman/shellcheck). This served as a great reference and I learned a lot from studying the codebase. The next two sections highlights the initial challenges of the project and how I learned to solved them.

### Distributing the binary

This ended up being a lot easier then I first thought it would have been, because of the default behavior of the vscode extension packaging tool vsce. By default when you package your extension for distribution by running `vsce package` it will add everything in the project directory to the final `*.vsix` artifact. So as long as the binary that you want to distribute with your extension is in the root project directory it will get packaged into the extension. Then during installation the binary will be written to the extensions working directory, maintaining the original file structure. So if you put the executable binary in a folder called `bin` inside the root project, it will also be in a folder called `bin` when the extension is installed.

Using this approach proved to be simple and effective, although with some drawbacks. Depending on your target audience of the extension you will have to provide different compiled versions of the binary for the required platforms and architectures. NodeJS provides a library called `process` that has the properties `platform` and `architecture` that will identify the system. You can use this to dynamically construct the path to the correct binary to use during execution.

### How to execute the binary

The vscode-shellcheck extension uses a dependency called [execa](https://github.com/sindresorhus/execa) in order to execute the shellcheck binary. Originally I tried using the same dependency but then I stumbled upon a weird problem, version 6 and later of execa requires using ES modules. Unfortunately [vscode doesn't support ES modules](https://github.com/microsoft/vscode/issues/130367), there is more information in the linked issue but from the comments it doesn't seem it will be supported anytime soon. Once I figured this out, setting the dependency to `v5.0.0` did allow me to execute the binary and move forward. Although, using a version of a dependency released 3 years ago didn't sit right with me. Luckily using the native methods provided by the `child_process` library proved to be sufficient for what I wanted to do.

You can try this yourself with a simple hello world example. If you create a Go program that prints out some text to stdout you can execute it from your extension like so:

```typescript
import { execSync } from 'child_process';

....

let result = execSync(context.asAbsolutePath("bin/go_tool.exe"));
vscode.window.showInformationMessage(result.toString());
```

When you run the vscode extension it will execute the binary and display the text gathered from go_tool.exe in the bottom right corner in an information box like so:

![Hello World](/executing-binary-in-vscode/HelloWorldVScode.JPG "HelloWorldVScode")

## Final words

The main vscode team has a project called [vscode-python-tools-extension-template](https://github.com/microsoft/vscode-python-tools-extension-template) that provides a template project on how to use Python. I decided to create a similar project called [vscode-go-tools-extension-template](https://github.com/sspaink/vscode-go-tools-extension-template) to help provide a starting template using Go. At the moment all it has is the "hello world" example. I am hoping to expand it and make it easier to understand and learn from in the future.
