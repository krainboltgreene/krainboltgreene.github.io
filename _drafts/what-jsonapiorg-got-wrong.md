---
title: "What JSON:API got wrong"
---

  - Error pointers should have been JSONPaths.
  - Errors shouldn't have included status or codes.
  - Relationship endpoints were a bad idea.
  - Enforce single level apis.
  - Join resources!
  - Errors should have just had slugs and maybe messages
  - Best practice blog

There are a lot of reasons to build a server API that vomits JSON.
As the backend to your iOS app (I've done that).
For the front end that your designer built (I've done that).
To act as a distributed server for your *other* Rails application (Yup, been there).
But mostly I find because it's *fun* to make server APIs.

There are a few things we need to go over before we can dive into this topic:

  1. I don't specify JSON because it's some divine format. Use any format that you like, as long as makes things easier.
  2. Worry about performance second. Regular programming rabbit holes are obvious compared to API rabbit holes.
  3. You could use any framework obviously, but Rails makes a lot of choices for me that I would have made myself. It's like having half the work done before I get to the table.

With those done I'd like to start the discussion with the building of the initial repository.
If you've already created a regular Rails app go ahead and throw it away.
Trust me on this, you'll save so much time in the long run.
Regular Rails apps encourage non-API thinking.

  1. A Token is two parts: A Key and a Secret.
  2. A Token Key is public, and shouldn't be able to harm anything if leaked.
  3. A Token Secret should be disposable.

Obviously there's a little more to tokens than that, but those are important concepts
