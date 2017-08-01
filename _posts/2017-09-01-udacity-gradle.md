---
layout: post
title: "Udacity - Building w/ Gradle"
date: 2017-08-31 22:22:22
tags: [video, reviews, 2017]
rating: na
---
- Project
  - can be made up of one or more Tasks
  - Tasks
    - contain Actions
    - can depend on other Tasks `task A(dependsOn 'B')`
    - can define Inputs/Outputs
    - new tasks can be defined w/ `tasks NAME {}`
    - we can define `defaultTasks` which are triggered when we run gradle

- typically we use a gradle wrapper
- Gradle daemon can be stopped w/ `gradle --stop`
- there is a configuration phase and an execution phase
