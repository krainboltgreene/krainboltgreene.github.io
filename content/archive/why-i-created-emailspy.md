---
title: "Why i created emailspy"
draft: false
---

**The Setup** It's Wednesday night and I'm working on a client's project. I'm making the app use S3 to store assets instead of being compiled by me and on Heroku. I notice that the production emails are wonky, specifically with the images and styles. I grab `letter_opener` and run development.

**Suddenly, an error!** I check out the backtrace and see that it's actually something wrong in `letter_opener`. I grab the meaningful lines and start googling. First result is an issue on the `letter_opener` repository. Ryan Bates says it's been fixed (No word on why it was released in the first place). Dig in and find out that it's been fixed on Github, but not released to Rubygems.

So now I have 3 options:

  1. Wait for Ryan Bates to release v0.0.3 which doesn't seem to be happening too soon.
  2. Point my Gemfile at the git repository for the fix, but who knows what else is broken? Tests *clearly* aren't being run.
  3. Fork the repository & release the fixed version.

I'm already leaning towards #3 because frankly *I need the fix right now* to continue working.

**Getting to work** I look at the repository again, how much work could it be? Turns out there's not that much code. There was also a lot of room for code quality improvement. I check out the pull request list, there are 13 outstanding requests. Then I check the issue list: 27 open!

I consider the situation some more and realize I also don't like the style provided. For me it was a pretty easy choice:

  * Ryan Bates didn't seem like he'd be able to do anything quickly even if I forked + made a pull request
  * I can't force him to release a new version of the gem, and he wasn't going to use proper SEMVER anyhow (latest was 0.0.2)
  * Experience tells me that any style changes I wanted would be turned into a bike shed war. I didn't have time for that.
  * It's not like `letter_opener` was complex and would need refactoring anyhow.

So I grabbed my terminal ran `bundle gem email_spy` and proceeded to take code from `letter_opener`. The code was then rewritten piece by piece to be segmented and easier to test. I also changed the naming conventions, broke up large chunks of code, and fixed a few bugs along the way. In addition I completely rewrote the stylesheet and the markup of the template.

Since I was satisfied with the public API I shipped 1.0.0. The differences were clear to me: Mine worked and Ryan Bate's didn't. Now I was looking at those pull requests of which only 5 seemed finished. I went in and looked at the code each of the features were pretty small but really useful.

I then went to my gem and wrote the code to implement those features. Changed the stylesheet to make those new fields show up. Added the markup and conditionals for the sometimes-showing-fields. I also fixed a few UI issues like plain text showing up first over the HTML. Fixed a few bugs that showed up on Windows. I added travis-ci support for continuous testing. By the end of it I had shipped v1.6.0 and it was well beyond `letter_opener`.

I decided to share my work on r/ruby and was met with some bullying an questions. A few were angry that I didn't fork and thus "lost" the git repository's history. I'm not sure how I feel about it. One commenter suggested that including the previous code's license wasn't enough. Another suggested I was "stealing" (MIT Licensed, Open Source) code.

Is `email_spy` still using `letter_opener`'s code? Debatable I think. That said in the readme I include Ryan Bates in the credits. I included `letter_opener`'s license in the repository. I also contacted Mr. Bates and pointed him at the repository letting him know what was going on.
