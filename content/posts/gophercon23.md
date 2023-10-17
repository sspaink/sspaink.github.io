---
title: "Gophercon '23 Recap"
date: 2023-09-28T15:19:36-07:00
draft: false
tags: ['gophercon', 'go']
---

![gopher plushie](/gophercon23/plushie.jpg "plushie")

> Gophercon is a conference for people excited to learn and create, it just so happens Go makes it easy to do just that.

I was lucky enough to visit San Diego and attend Gophercon again this year. I had a great experience, got to listen to some interesting talks and interact with lots of smart people. Seeing the excitement and energy people have while discussing projects they have been working on is inspiring. I do think my biggest takeaway from attending is a renewed energy and motivation to continue learning new things, start new projects and revive dead ones.

There was a lightening talk given by [Benjamin Bryant](https://www.linkedin.com/in/benjamin-bryant-dev/) titled `The Blueprints to Building Your Own Badass Community`. It was on how he revived the London Go meetup and gave tips on how to do something similar based on his experience. There aren't any Go meetup groups in my area, seems the closet one is in [Saint Louis](https://www.meetup.com/stl-go/) which could be fun to attend in the future.

I've found the lightening talks are always worth attending. The time constraint seems to help the speaker create a concise and straight to the point talk. Another memorable lightening talk was titled `Can ChatGPT Do My Job?` presented by [Michael Richman](https://www.linkedin.com/in/michael-richman-b7807b2/) was an entertaining case study on trying to write a program using ChatGPT, ending with a lot of apologizing from ChatGPT. Luckily it seems our jobs are safe, although it is very impressive to see what it was able to create with the simple prompts ChatGPT was given.

The talk I really enjoyed was `The Future of JSON in Go` by [Joe Tsai](https://www.linkedin.com/in/dsnet/). I think Joe did a great job explaining the current problems with the JSON package and why there is a need for a new one. [The proposed new package is still experimental](https://github.com/go-json-experiment/json), but the hope is to get it merged into the standard library. I thought the use of options for backwards compatibility was a clever design to enable a slower adoption approach. Allowing users to pick and choose what breaking features to use. The performance improvements also seemed quite impressive. Not sure what project I have that I can start using this package but I definitely would like to try it out.

TinyGo had a big presence again this year at the conference. There were 4 talks about using TinyGo that I attended, there was also the fun community day event. TinyGo is great for hobbyist and probably why there is so much interest in it. The talk given by [Jeremy Fleitz](https://www.linkedin.com/in/jfleitz/) on restoring a pinball machine with Go was very creative use case. Definitely inspired me to look into finding more hardware projects that I can use Go with. So far I've only used TinyGo to help light up a [thermal detonator](https://github.com/sspaink/go-thermal) and that still needs some work.

## Static Analysis tools

There were two talks about two different static analysis tools that have been created to help with Go dependency security. One is called [capslock](https://github.com/google/capslock) and the other [govulncheck](https://pkg.go.dev/golang.org/x/vuln/cmd/govulncheck).

The tool [capslock](https://github.com/google/capslock) was shown by the presenter [Jess McClintock](https://www.linkedin.com/in/jessica-mcclintock-6b02b6273/). It reports on the [capabilities](https://github.com/google/capslock#what-are-capabilities) of a Go package. I assume integrating capslock into some sort of reporting system, perhaps during a PR, to warn reviewers of newly introduced privileges. With its endless dependencies I think the project [Telegraf](https://github.com/influxdata/telegraf) could benefit from using this tool. Could maybe be used to revive this [closed pull request](https://github.com/influxdata/telegraf/pull/9668) to limit plugin privileges.

The tool [govulncheck](https://pkg.go.dev/golang.org/x/vuln/cmd/govulncheck) was presented by [Julie Qiu](https://www.linkedin.com/in/julieyeqiu/). This tool checks the [Go vulnerability database](https://vuln.go.dev/), which I didn't know existed! I didn't follow this in the talk but it seems dependabot might be using this? This could be very useful as well to be included in a CI system.

---

It was a great week, although definitely happy to be back home. My social battery has been completely depleted. Next years Gophercon will be back in Chicago sometime in July. I would love to visit the [Chicago Field Museum](https://www.fieldmuseum.org/) again, so many cool dinosaur skeletons.

![gopher plushie](/gophercon23/plushie_home.jpg "plushie")
