---
title: "Removing Old Versions Of Homebrew Packages"
date: "2017-03-17T16:39:29Z"
tags: ["MacOS", "Homebrew", "brew", "Tips & Tricks"]
comments: "true"
flag: "gb"
---

{{<highlight bash>}}
brew update && brew upgrade && brew cleanup
brew doctor
{{</highlight>}}
