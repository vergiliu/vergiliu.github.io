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
- fail fast, notify asap, limited scope affected by the change
	- pretested commit
- break on compilation, tests, error in env. - fix immediately by team
- separate env. specific config from the build scripts
- devs have sense of ownership
- maybe use a build master (owner) for large teams
- artifact repo: binary creation should be repeatable
- testing pyramid: unit, service, UI
	- UI at acceptance level, don't touch DB, use dependency injection,
	- acceptance tests after commit stage tests
	- use stubbing for large scale components or systems, for components prefer mocking
		- for mocks we can verify how the code behaved; can also be used to isolate 3rd party code
	- minimize state
	- "fake time" use replacement classes for working with time/date
	- try to keep commit stage time to 5-10 minutes (upper bound)
		- split into parts and parallelize if not possible
- on every change: build, run tests, produce metrics; must check-in often

Ch8 - Automated Acceptance Testing
- acceptance tests that verifies the acceptance criteria of a story
	- validating business acceptance criteria
	- business facing
	- some scenarios difficult to test otherwise
	- used when making very big changes to the application
- create acceptance test suites
	- to be maintainable they have to be layered
	(- use iterative processes?)
	- BDD approach to testing gives us executable specifications and generated documentation
	- window driver pattern
- we should use the app API to set up the initial data (state)
- try to make tests atomic, or cleaning up after themselves
- never add testing interfaces to production deployed components
- handling asyncrony - try to wrap it in synchronous calls
- test environment should mimic external systems
	- minimize coupling between tested system and external systems
	- delivery team should be resposible for acceptance tests
	- if they are not fixed asap tests will deliver little to no value
- first chance to check if automated deployment works
- tend to your acc testing over time
- parallelize, virtualize, use grid (for Selenium)
Ch 9 - Non Functional Requirements
Ch 10 - Deploying and Releasing Applications
Ch 11 - Managing Infra and Environments
page 325

Ch 12 - Manging Data
- have automated database migration in place (flyway, liquibase)
- roll forward and roll-back* (not really necessary lately)
- fwd/backward compatibility between layers (app -> db)
- unit tests must not run against database or in memory db / must be isolated
- coupling between tests/data: test isolation, test adapting, test sequence
	- for 1st we can use partitioning of data as well
	- effective tests are not data driven
- data in acceptance tests
	- more complex inherently, reference data can be kept as db dumps
	- use APIs to put system in correct state
- capacity tests
	- automate generation of data
	- use acceptance tools for data and scale it up
	- for exploratory testing using production data is not necessarily needed
- (reproduction scenarios for issues - prod db is necessary)
344


Rating unrated/5

| Apple      | [iTunes] |
| Google     | [Play Store] |
| Goodreads  | [Goodreads] |
| Amazon     | [Books] |

[iTunes]: https://itunes.apple.com/us/book/
[Goodreads]: https://www.goodreads.com/book/show/
[Play Store]: https://play.google.com/store/books/details/
[Books]: http://www.amazon.com/
