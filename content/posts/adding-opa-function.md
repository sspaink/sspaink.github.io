+++
draft = false
date = 2023-09-14T00:42:07-05:00
title = "Adding a built-in function to OPA"
description = ""
slug = ""
authors = []
tags = ['opa', 'rego', 'open-source', 'go']
categories = []
externalLink = ""
series = []
+++

![gopher viking](/adding-opa-function/viking-gopher.png "Viking")

I've been using the tool [Open Policy Agent](https://github.com/open-policy-agent/opa) at work. Everyone refers to the project using the acronym OPA. I'm Dutch, and opa means grandpa. It's a bit funny to hear it in sentences like "have we deployed the latest version of opa". Evidently it is also a [common expression in the mediterranean](opa)? The more you know 🌈

I was able to make a contribution to OPA. I added [a new built-in function to the project](https://github.com/open-policy-agent/opa/pull/6187). I had a positive experience  contributing, the documentation was great and the maintainers were friendly and quick to respond. The new built-in function I added, [numbers.range_step](https://www.openpolicyagent.org/docs/latest/policy-reference/#builtin-numbers-numbersrange_step), even got released already in [v0.56.0](https://github.com/open-policy-agent/opa/releases/tag/v0.56.0):smile:.

In this post I wanted to extend [the existing documentation on how to add a built-in function](https://www.openpolicyagent.org/docs/latest/contrib-adding-builtin-functions/) with my experience.

## Declare and Register the function

Declaring the function is just a matter of creating a declaring a variable of type `Builtin` and filling out the fields. The description is the trickiest part to describe the function in an easy to understand way but without being too wordy. The only hiccup I had here was needing to run `make generate`. There is another file called `builtin_metadata.json` that gets updated by running this command and this update is required in order to pass the CI. While the documentation does mention needing to run this to update the `capabilities.json` it overlooks that it is also needed to update `built_metadata.json`. Not a big deal. I just struggled with it for a moment because unfortunately `make generate` failed to run on my Windows machine, and updating the json files manually didn't work. Worked just fine on my linux laptop so it was an easy hurdle to get over.

## Test the function

The OPA project has abstracted the process of adding unit tests by providing a common template to fill out with test cases. So instead of creating your own table test within a new Go test file you will create a yaml file under `test/cases/testdata`. There are a lot of folders in this directory, so lot of references on how to write one, [although be careful most of them were auto generated magically long ago!](https://github.com/open-policy-agent/opa/pull/6187#discussion_r1308536821) So the formatting of this auto generated tests shouldn't be copied. For example the auto-generated cases split all the test cases for one built-in function into multiple files, this isn't necessary. And you also don't need to copy the naming convention `__local0__`, use something more descriptive (curious where the name came from).

Let's take a look at an example test case I added, that I think highlights the essential fields you need in a test case:

```yaml
- note: numbersrangestep/memoryexample
  query: data.test.p = x
  modules:
  - |
    package test
    p = num {
      num := numbers.range_step(1024, 4096, 1024)
    }
  want_result:
  - x:
    - 1024
    - 2048
    - 3072
    - 4096
```

From context clues it is fairly straightforward but I did find this a little bit confusing at first. I think the best way to understand the fields is by looking at [the test case execution logic](https://github.com/open-policy-agent/opa/blob/main/topdown/exported_test.go#L50-L126), because these test cases are just supplied to a regular Go test.

* **note:** A string that will be the name provided to [testing.Run](https://pkg.go.dev/testing#T.Run), a bit odd it is called note instead of name?
* **query:** A string representing a path in dot syntax to the result you expect (seems `data.test.p = x` is most likely what you will always use)
* **modules:** An array of Rego policies that invoke your built-in function, although from the examples it seems having a single Rego policy per test case is common.
* **want_result:** A map, where the key is a string and the value an empty interface. The key will be the name of the variable which can be assigned anything you expect the test to return. In this case I expect an array of numbers.

## Final words

I do wonder if there are any funny use cases for this tool in a hobby context. Is there any reason to setup OPA on a raspberry pi? Are there any silly use cases for it, could you make a game with it?
