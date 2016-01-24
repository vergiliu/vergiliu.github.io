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
- Performance - time to process a single trasaction
- Throughput - number of transactions the system can process in a given timespan
- Capacity - maxim throughput the system can sustain with acceptable response time for a given request
- it's essential to identify correctly which one is the main focus point
- important to consider them upfront, in the early phases of the project as they are more difficult to tackle later on
- "Premature optimization is the root of all evil" - D. Knuth
- try to avoid the tendency to start with optimizing, and concentrate on writing "clean" code
- important ability during testing is to be able to simulate realistic use scenarios
- capacity tests should be able to run in parallel with different scenarios
- isolate environments as far as possible
- be careful about virtualizing
- be wary about assuming (linear) scaling in case performance testing environment is not same as production
	- most of the times, it is difficult to get same prod. environment just for capacity testing
- Automatic capacity testing
	- tests should be short, repeatable, robust and test real-world scenarios
	- we can try to scale up acceptance scenarios which are already well defined/undestood and real
	- recommendation is to avoid capacity testing through UI and focus more on API/low-level API
	- for high performance apps, the tests themselves must be certified in some way that they apply
	the counted on load to the environment and that they are not the actual bottleneck
- overall it might be a good idea to separate capacity tests out of the pipeline if integrating them is a too-complex task
- (optionally) have a possibility to roll-back
	- backup data before beginning, and practice restore/rollback
	- Blue-Green deployments
		- have 2 similar prod deployments, install on one test on the other then switch
			- or have apps installed on same env twice and make the switch
		- possible database issues, make read only before switching
	- Canary release
		- deploy a new version of one application in prod
		- find possible problems with that application
		- roll back is easy and simple - as we only have 1 app
		- can be used for A/B testing
		- complicated database upgrades
- emergency fixes should go through same processes as regular updates
	- otherwise almost impossible to reproduce on next iteration depending on changes done
	- sometimes easier to just roll back
- "Deployment Is the Whole Team’s Responsibility"

Ch 10 - Deploying and Releasing Applications
- a deployment strategy is beneficial if agreed upon at the beginning of the project
- deployment should be done automatically from day 1, same tools/scripts should be used for all envs.
- possibility of viewing which build is in which stage (of the pipeline)
- we should have smoke tests after deployment to make sure the deployment is running, or be able to diagnose it quickly otherwise
- we should have a way to select any version that has passed the previous "gate" successfully
- promoting configurations as well as binaries
on page 266

Ch 11 - Managing Infra and Environments
- state of infra should be known through monitoring, and should be specified through version controlled config.
- we should catch environment problems early in the initial stages
- by striving to release as often as possible we will get little change between releases
- identifying the procedures to release (CAB, ITIL documents, etc) should be part of dev team release plan
- ops teams have visibility into environment using monitoring (Nagios, OpenNMS, HP OpsMgr)
- use the technologies the Ops Teams is most familiar with (Shell, PowerShell)
	- if same process is used to deploy all envs. it is important to be familiar to all teams concerned
- keep all configurations of the infra in version control
- lock down your environments in the same way
- same changes (requests & approvals) should be in place for UAT and Prod
	- we should have traceability for all changes, including testing of the updates
- prefer automation over documentation
	- provisioning of new hardware resources should be automated
- small example of having puppet deploy postfix email server
- config of middleware comprises: binaries, config, and data
- "no technology can be considered enterprise ready if it can't be deployed and configured automatically"
page 299
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
Ch 13 - Managing Components and Dependencies
415
Ch 14 - Advanced version control
441


Rating unrated/5

| Apple      | [iTunes] |
| Google     | [Play Store] |
| Goodreads  | [Goodreads] |
| Amazon     | [Books] |

[iTunes]: https://itunes.apple.com/us/book/
[Goodreads]: https://www.goodreads.com/book/show/
[Play Store]: https://play.google.com/store/books/details/
[Books]: http://www.amazon.com/
