---
title: "npm as a topic"
draft: false
---

  - it's npm not NPM or Node Package Manager
  - npm doesn't have a lockfile:
    - it did, they called it shrinkwrap.json, it wasn't required
    - they do as of npm 3, it's called package-lock.json, it's required
  - npm nests folders
    - only if there are multiple versions of the same package
    - you can still enforce a top level version by specifying your own version
    - the nesting scheme is due to how *node* works, not *npm* (and it wont change)
    - this is the nature of javascript: packages aren't global
  - yarn is not faster in all cases
  - yarn is straight up broken
  - fuck [npm|yarn] install
