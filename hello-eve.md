Hey Eve!

It's been a long time since I sent you any mail, but I think this is a good one. I loved legos as a child and my parents managed to get me quite a few sets. I remember playing with one of my childhood friends and inventing with him a complex role-palying strategy game involving them. In the package you're getting this from are two beginner lego sets. I hope you find them enjoyable, because I enjoyed getting them for you!

I participated in a day long hackathon called [OpenSourceNOLA][OSN]. Before I get into the prizes that I won I'd like to tell you about what open source means for me. You see I've always been rather poor and my family is no different. When I had you with your mother we didn't have much money and I was working for $8 an hour at RadioShack and I hated it. When I started programming I didn't have the resources for big computers or operating systems. Open source made a lot of tools free and available to me.

I guess you probably also don't know what a [Hackathon][HACKATHON] is and I can tell you about that too. Hackathons are essentially get-togethers for computer-oriented people. Many of the people who go to these events are software developers like me. Some people are designers, graphic artists, or people who have ideas and are essentially community managers. We all form teams and work on common goals.

The OpenSourceNOLA hackathon is a combination of these two concepts. Technically minded people get together in order to work on free and openly available software that might benefit others. The group I was a part of consisted of [Melvin Arndold][MELVIN] and I, two experienced developers attempting to play with some new tools. He had the idea to build a unique social network over a rather untapped band of connection: Bluetooth.

I suppose I could explain [Bluetooth][BLUETOOTH], but frankly it's still complicated to me. The short of it is that Bluetooth is a specification and device type for two nodes (devices) to communicate over. In this application we built the Bluetooth service would be our [link layer protocol][LLP]. Similar to [Ethernet][ETHERNET], [RJ45][RJ45], or [USB][USB] connections. This is further complicated and definitely beyond my ability to explain, but I'm sure you'll figure it out.

A person named [n8fr8][N8FR8] thought up the idea to use the name of your Bluetooth device as the message you want to send others. This particular piece was named **wind** and would be the foundation of our application. In technical terms this is what you'd consider a [internet layer protocol][ILP], much like [TCP/IP][TCPIP]! We used his idea to build our own implementation in [meteor.js][METEOR] using [Cordova][CORDOVA] to deploy to an android phone. These are more things that you can look up or maybe I'll just explain them later.

So far I've been talking about the *hard* things to understand, but since we've got that out of the way it's time to talk about the *easy* things. On top of all these layers we wrote a very simple application. The prototype ended up being called windjs and we compiled it for android. It was incredibly enriching to dive into not only Bluetooth specifications, but also experience Meteor and Cordova.

First we needed to find out how to set the Bluetooth device name. This proved amusingly difficult since none of the libraries assumed we'd want to do this! Apparently most Bluetooth tools don't *care* about the name of the device. It's a shortcut for humans to understand what is talking to what. Our first pull request was to the BlueToothSerial library for Cordova, giving us API access to the name field.

After forking the repository and making our change we moved on to the daunting task of starting a new meteor project with Cordova in mind. I have to admin Eve that I was surprised at how flipping easy it was to do this integration. For all the dislike I have of isomorphic applications this was painless. The prototype application near instantly deployed to the connected phone and we were on our way.

Simply allowing the user to set their name was the first step.


[OSN]: http://www.example.com
[HACKATHON]: http://www.example.com
[MELVIN]: http://www.example.com
[BLUETOOTH]: http://www.example.com
[LLP]: http://www.example.com
[ETHERNET]: http://www.example.com
[RJ45]: http://www.example.com
[USB]: http://www.example.com
[N8FR8]: http://www.example.com
[ILP]: http://www.example.com
[TCPIP]: http://www.example.com
[METEOR]: http://www.example.com
[CORDOVA]: http://www.example.com