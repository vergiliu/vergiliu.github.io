---
layout: post
title: "Continuous Delivery"
date: 2015-12-31 23:45:00
categories: [books, video, reviews, tutorials, articles]
tags: [books, video, reviews, 2015]
---

Ch5 - Deployment pipeline
- Process can be seen as a value stream map -> fundamentlly an automated software delivery process
- Build source only once
- Keep configuration(s) separately from code
- deploy using the same process in all environments
- if any part of the pipeline fails, stop the line
- my pet peeve, the pipeline, is actually referring to the CPU pipeline, which is somewhat different but hey not anybody can like it, right?
- we should handle as much as possible in an automated fashion, esp. wrt to the host and environment configuration
- it should as simple as pushing a button to release a specific version of the software
- a reduction in risk is only related to the actual degree that a process is tested/rehearsed
- either roll back to a known good version or deploy everything new
- to automate a deployment pipeline
	- automate build and deployment: use same process everywhere, but different configurations
	- automate unit tests and code analysis
	- automate acceptance tests: try to automate non functional tests into the pipeline
	- automate releases
- the pipeline should record and time all changes so it should be easy to identify the bottlenecks and where it can be improved
	- best example: time to release a single line of code change into production
	- should help you determine cycle-time
	- some metrics can help you determine the issues: test coverage, defects #, # builds / day, build duration
	- get build and build steps metrics and check which ones can be improved

Ch6 - Build and deploy scripting
- quickly going over the most common build tools: make, ant, msbuild, maven, rake, buildr
- use same scripts to deploy to every environment
- build traceability in binaries, VCSs
- automation should be the only way to deply software
Ch7 - Commit stage


Rating unrated/5

| Apple      | [iTunes] |
| Google     | [Play Store] |
| Goodreads  | [Goodreads] |
| Amazon     | [Books] |

[iTunes]: https://itunes.apple.com/us/book/
[Goodreads]: https://www.goodreads.com/book/show/
[Play Store]: https://play.google.com/store/books/details/
[Books]: http://www.amazon.com/
