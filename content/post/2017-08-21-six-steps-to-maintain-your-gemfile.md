---
title: "Six Steps to Maintain Your Gemfile"
date: "2017-08-21T21:29:29Z"
tags: ["Gemfile", "Maintenance", "Best Practices"]
comments: "true"
flag: "gb"
---

If you work with Ruby, you use Bundler all the time, I believe. Bundler manages all the gems that
the application or library depends on. You just have to define them in Gemfile and execute a proper
command for installing or updating your libraries. Sounds easy, right? But you, of course, know that
is not 100% true for big projects or for projects where the team does releases frequently during the day.

In most cases, the Gemfile grows fast, dependency definitions scattered in the file, versions are
not properly set and so on, and so forth. Have you ever had a problem with running `bundle update`
and getting conflicting gems message? When a security patch for some gem was released,
but your hands were tied because of reason above? I have this issue almost every time when I start
working on an application which is already in active development or, even worse, abandoned for a while.

In this guide, I would like to share with you some steps for preventing such issues in the
Gemfile-based applications. Most likely, Rails projects.

## Step 0: use RVM and Rbenv for having a uniform version of Ruby across the team

Create `.ruby-verion` and `.ruby-gemset` files (RVM) with specific version of Ruby and gem set name.
Commit them into revision control system. Use the same version on continuous integration server and
all environments. This action will stop an introduction of not compatible and old dependencies
by team members.

## Step 1: be aware of language vulnerabilities

Make checking [the Ruby security page](https://www.ruby-lang.org/en/security/) a part of your
maintenance process. Subscribe to the mailing list and immediately update Ruby when new
vulnerabilities are fixed.

## Step 2: check vulnerable versions in dependencies

[Bundler Audit](https://github.com/rubysec/bundler-audit) solves this problem for you.
Just execute `bundle-audit` and make sure that you get next message: **No vulnerabilities found.**

Otherwise, all warnings must be fixed. Usually, it includes 2 major things:

* upgrading vulnerable gem to the teeny version.
  You can find a bit more about Ruby versioning policy [here](https://www.ruby-lang.org/en/news/2013/12/21/ruby-version-policy-changes-with-2-1-0/).
* replacing `http://` or `git://` in repository url by secured `https://`

bundle-audit could run on continuous integration server and fail your build or test run in case
of existing vulnerabilities.

## Step 3: do a licensing audit

Living in the age of open-source software gives us a luxury of using tons of existing libraries and
be productive, be focused on solving business problems. But most of the developers don't think
about legal aspects when they introduce a new gem or a package. And this could cost to the company
a big amount of money.

Just remember: "Unlicensed code is copyrighted code that you do not have the permission to use.".

Recently I found very useful gem (hah, gem for solving issues with gem) called `license_finder`.
The tool is maintained by Pivotal and all dependencies for licenses. It works with Python, Node,
Go, Java as well. Check [the Github repo](https://github.com/pivotal/LicenseFinder).

{{<highlight bash>}}
$ gem install license_finder
$ license_finder
........................................
Dependencies that need approval:
bundler, 1.12.5, MIT
minitest, 5.10.2, MIT
rake, 12.0.0, MIT
simplecov, 0.13.0, MIT
simplecov-html, 0.10.1, MIT
....
{{</highlight>}}

Specify the whitelist of allowed licenses and run `license_finder` again for approving dependencies:

{{<highlight bash>}}
$ license_finder whitelist add MIT
$ license_finder
........................................
{{</highlight>}}

## Step 4: upgrade gems carefully

The basic rule is don't try to upgrade to major versions without reading `CHANGELOG` and being sure
that is no breaking API there.

Go to [RubyGems](https://rubygems.org) and check how old the dependency is.
Even if you wouldn't use new API or features of the gem, upgrade it for the future.

If another dependency is conflicting with new version try to upgrade this dependency as well.
Make notes if the process didn't succeed.

And last, try to use annotation to make people know about the maintainability without looking
on the Internet. I know, it's too verbose, but can be helpful. Some examples from my Gemfile:

* `May 2017` -- release month
* `May 2016 <- TODO: upgrade to 0.9+` -- release month and the possibility for upgrading the gem
* `# FIXME: upgrade to > 3.4.4 breaks permitted params: undefined method "wrap"` -- make the team be aware of upgrade issues
* `# NOTE: Gem "config" must be placed before "aws-sdk" and "typeform"` -- says about the problem with dependencies loading

## Step 5: take care of Gemfile readability

Use consistent formatting, grouping with blocks instead of single line definitions, split all
dependencies into sections:

{{<highlight ruby>}}
## Backgroud jobs, queue -------------------------------------------------

gem "daemons",                   "1.2.4" # Aug 2016
gem "delayed_job",               "4.1.3" # May 2017
gem "delayed_job_active_record", "4.1.2" # May 2017
gem "delayed_job_web",           "1.4"   # May 2017
gem "stomp",                     "1.4.4" # Jun 2017

## Logs, metrics ---------------------------------------------------------

gem "grape_logging",  "1.5.0"  # May 2017
gem "lograge",        "0.5.1"  # May 2017
gem "logstash-event", "1.2.02" # Sep 2013
{{</highlight>}}

Just follow it. Don't put new gems in random places of the file.Find your own way of organizing
dependencies and just follow it. Don't put new gems in random places of the file.

## Step 6: think more when you introduce a new gem

Ask yourself a new question: do you really need a new gem for solving the problem?
Maybe you can use Ruby standard library, Rails features, or existing gem instead.
Yes, it can cost you a bit more time, but wouldn't bloat dependencies.

---

**P.S. I would like to hear how you keep your Gemfile and dependencies maintainable.
Please don't hesitate to write some comments and share your experience.**