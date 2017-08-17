---
title: "Solving The Hanging Problem In Ember CLI"
date: "2017-08-11T17:51:29Z"
tags: ["Ember.js", "ember-cli", "NPM", "Debug"]
comments: "true"
flag: "gb"
---

Working on some project today I had a problem with building an Ember application.
The process has stuck right after running `ember build` or `ember serve`.
No output, no CPU activity, nothing.

The first thing that people usually do to solve any issue in JavaScript project,
it's a clearing the NPM and other package managers cache. I did the same.

<!--more-->

{{<highlight bash>}}
npm cache verify
bower cache clean
rm -rf node_modules bower_components
npm install && bower install
{{</highlight>}}

It didn't help much.
After spending some time on Ember CLI documentation I found an incredibly usable command line
argument **DEBUG=ember-cli*** that outputs everything happening during the command execution.

Now I saw that the log has stopped on the next lines:

{{<highlight bash>}}
ember-cli:cli command: build +0ms
ember-cli:testing cli: command.validateAndRun +2ms
ember-cli:watcher no watcher preference set, lets attempt watchman +3ms
ember-cli:watcher execSync("watchman version") +0ms
{{</highlight>}}

That's it! I realized that something wrong with the `watcher` package.
Namely, another program called `watcher` was installed on my laptop in `/usr/bin` folder.

{{<highlight bash>}}
~/W/a/client » watchman version
> ^C
~/W/a/client » watchman --version
> 4.7.0
~/W/a/client » npm install watchman -g
> /usr/local/bin/watchman -> /usr/local/lib/node_modules/watchman/watchman
> + watchman@0.1.8
> added 17 packages in 19.787s
~/W/a/client » watchman --version
> ERROR: Unknown option --version
~/W/a/client » watchman version
> Please specify a target and action
{{</highlight>}}

Then I setup the NPM package globally in the system and it solved my issue.

<div class="alert alert-warning"><strong>Tip of the day:</strong> DEBUG=ember-cli* ember [command]</div>