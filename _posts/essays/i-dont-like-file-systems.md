---
title: "I don't like file systems"
---

[I've always been someone who is critical of how computer filesystems are shaping up these days][0]. I think my biggest contention is that these [developers][1] can't come up with a memorable name at all. Most people who care about this topic will shrug that off as a small thing (If they don't suggest it's a feature).

Which is fine by me because I have a ton of ammo left to throw at these things. Filesystems are a very integral part of the operating system. Not only is it a technical aspect, but also a user experience situation. This might seem like an odd thought but consider this:

  * Opening a window on your desktop?
  * Fixing a crash?
  * Installing a program?
  * Listening to music on your media player?
  * Copying a movie to disk?
  * Transferring files to your company?
  * Sending an email with attachments?

All of these require using the filesystem as represented by the file manager software, or worse the command line (*UGH!*). Yet filesystems are rarely (If ever) focused on how users deal with them. Instead they spend hundreds of hours improving how the computer handles fuck ups. God forbid they make these things friendly or intuitive, because then they wouldn't get to work on problems like these:

> **[Break 32,000 subdirectory limit][2]**
>
> In ext3 the number of subdirectories that a directory can contain is limited to 32,000. This limit has been raised to 64,000 in ext4, and with the "dir_nlink" feature it can go beyond this (although it will stop increasing the link count on the parent). To allow for continued performance given the possibility of much larger directories, Htree indexes (a specialized version of a B-tree) are turned on by default in ext4. This feature is implemented in Linux kernel 2.6.23. Htree is also available in ext3 when the dir_index feature is enabled.

Or this one:

> **[Large file system][3]**

> The ext4 filesystem can support volumes with sizes up to 1 exabyte and files with sizes up to 16 terabytes.

Now my personal view is much more in line with [Ignore The Code][4]'s author, [Lukas Mathis][5]. For instance he quoted [Towards Semantic File System Interfaces][6] from Sebastian Faubel and Christian Kuschel:

> Conventional file system interfaces require users to interact with a hierarchical classification of their files (taxonomy). In this sense, the user of a hierarchical file system is confronted with most of the disciplines in knowledge management, which includes creating and maintaining a consequent vocabulary and naming conventions in order to be able to remember where to look for files in the hierarchy. This is opposed to the natural approach of memory, knowing what to look for.

Not enough for you? Check out this other quote from that post:

> Whilst most advanced computer users have occasionally had to struggle through the hierarchy on their hard disk, this inconvenience does not seem to warrant redesigning how files and folders are stored and retrieved. However, anyone who has studied how application users store files realises that the file system is quite a large barrier to all but the most advanced users. The interface designers for Windows 95 realised that most users did not understand the folder/file hierarchy and those that did had a hard time making it work for their filing needs (Sullivan 1996).

> Furthermore, it is not at all clear that a strict hierarchical classification of files is always appropriate (Dourish 2000). Symbolic links, aliases, shortcuts and other patches have been added to most operating systems in an effort to allow users to make the same files visible from multiple places in the file hierarchy. (Of course, these mechanisms work differently in different operating systems). This kludge clearly points to some more systemic problem.

> Gary Marsden, [Improving the Usability of the Hierarchical File System][7]

Filesystems don't fail just at organization and usage they also fail at display! Did you know that most major filesystems are case sensitive? Of course you did, because you've repeatedly seen errors as you type "cd /repository/Ruby/scripts/" instead of "cd /repository/ruby/scripts/". How many times does the average user really need to have to different files with different case?

In any case I sit here waiting for a file system that focuses not on hierarchy, but tags and metadata. Also, a cool name maybe? Might be asking too much from these people.

[0]: http://twitter.com/krainboltgreene
[1]: http://en.wikipedia.org/wiki/List_of_file_systems "How uncreative can you be?"
[2]: http://en.wikipedia.org/wiki/Ext4#Features
[3]: http://en.wikipedia.org/wiki/Ext4#Features
[4]: http://ignorethecode.net/blog/2009/10/11/flatland/ "This guy is great, keep an eye on him!"
[5]: https://twitter.com/LKM
[6]: http://www.organise-fw.org/media/documents/paper_iswc2008.pdf
[7]: http://pubs.cs.uct.ac.za/archive/00000225/01/sacj-dave.pdf


I'm working hard to make my way in the tech world. I keep up with current trends and ideas. I've focused myself on the software side of the world even though I'm hardware savvy. I've also decided to be a programmer over everything else. Even more so I've decided to be an web developer in the programming community. The bigger problems I've had is that I don't yet have the proper tools to really get out there **and** I'm not exactly in Startup/Tech Central (ie, everywhere but Bay Area and Silicon Valley).

In other news I'm going through what I call my yearly Ubuntu Period (classy, I know). It's a bi-yearly event where I get so damn frustrated with the million small paper cuts from Ubuntu + [GNOME][0]/[KDE][1]/[Xfce][2]. Mostly usability issues, but also design aesthetics and UI/UX fuck ups. It boggles me how much these people seem to miss. Is it general apathy toward usability or ignorance? Or maybe they don't have the resources. In any case it's during these Ubuntu Hate-fests that I renounce Ubuntu forever and try to build my own linux from scratch.

**Hint**: I usually just go back to Ubuntu.

Getting an OS together that's both clean and easy is fucking balls hard... For Linux distro-admins apparently. Apple does a great job at giving you the very basics with the iPhone and then giving you the freedom and (powerful) ability to get anything else you need. Why can't Ubuntu be set up more like that? Here's a quick step by step that should be followed:

1. Release a structured and activity/social friendly Repository site. Where documentation, good design, and constant updates are easy and approachable.
2. Develop an easy to use and intuitive application downloading program. PRO-TIP: Software Center was a fuck up and Add/Remove is a disgrace. Start over with the good ideas, drop the bad.
3. Create a minimal OS that only provides: A basic install guide (Loading the default choices), and an advanced *"Chose which ones you want installed"* categorical install guide.
4. Ship, Social Network, PR.
5. Write a CC-like FOSS license (Yes, I know about GPL and MIT. They aren't cool enough for regular people to care about.)
6. Promote a $1.00 application market so that developers flock to the community.

**This** is how it should be done and from here on out if you're smart, Linux. The gray beards and the gurus & grognards won't live forever, and considering flame wars their blood pressure must be horrible. Eventually you'll need more developers from the young and new. These guys aren't going to be interested unless it's iPhone Easy.

[0]: http://www.gnome.org
[1]: http://www.kde.org
[2]: http://www.xfce.org
