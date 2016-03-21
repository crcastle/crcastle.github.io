---
layout: post
title:  Collaborative Code Conference
categories: collaborative coding, heroku, webrtc, twilio, operational transformation, gulf, codemirror, interview, remote programming
---
 <h3 style="text-align: center;"><em>"I just want a simple way to do a video call<br>and a shared code editor!"</em></h3><br>

Recently I've been interviewing for Developer Relations roles.  As a technical role, part of the interview process includes evaluation of my technical abilities.  Or to put it more simply, writing code while a stranger peers over my shoulder.  That normally happens one of two ways.  If it's an in-person interview, I'm writing code on a whiteboard.  If it's over the phone, I'm writing code in a shared Google Doc.  Neither of these are how a developer normally writes code.  Ever.[^1]  There are tons of opinions out there expounding how technical interviews are broken, so I won't go down that rabbit hole.  But I figured I could leverage my experience to create a simple tool to solve a specific problem.  And here it is.

![Screenshot](/img/code-editor-screenshot.png)

Emphasis on the simple.  My aim wasn't to create a full-blown interview workflow tool or try to replicate all the functionality of a real <abbr title="Integrated Development Environment">IDE</abbr>.  I just wanted a way to code with someone remotely and see and hear them too.

As I talked to people about this, it became clear that it could be used for more than a technical interview: remote pair programming, helping a friend debug a problem, teaching or tutoring a beginner programmer.  [What else](https://twitter.com/intent/tweet?text=I'm using a collaborative code editor for... (@crc))?

#### Something like this doesn't exist already?
Surprisingly, no!  At least I didn't find anything free or nearly free that provides a video conference and a shared code editor and is open source.  Let me know ({% include icon-twitter.html username="crc" %}) if I'm wrong!

In my research I found lots of video conference tools -- some free, some not.  [Google Hangouts](https://hangouts.google.com), [Uber Conference](https://www.uberconference.com), [talky](https://talky.io), [Skype](http://www.skype.com), and many others provide video conferencing and/or screen sharing.  For shared code editing, there are also lots of options: [collabedit](http://collabedit.com), [kobra](https://kobra.io/#/), [Space](https://github.com/chaoscollective/Space_Editor), [Firepad](https://firepad.io/#1) (which serves as the underlying collaborative editor for some of these other ones), [Koding](http://www.koding.com), [CoderPad](https://coderpad.io), [Codebunk](https://codebunk.com), [codeassium](https://codassium.com), [Squad](https://squadedit.com), [Codeanywhere](https://codeanywhere.com), and [codeshare](https://codeshare.io).

None of these met my criteria: 1) combine video conference and collaborative code editing, 2) free, and 3) open source.  [Codeshare](https://codeshare.io) comes the closest but doesn't look to be open source.

#### Try it out
Want to see it in action?  There are two ways to try it out.  

Spin up your own private instance of the app for free.  Clicking the "Deploy to Heroku" button below will deploy the app for free.  You'll need to create Heroku and Twilio accounts if you don't already have them.  You'll also need to generate a few Twilio configuration tokens.  The URLs where you'll get these tokens will be shown to you.  It's really simple.

<div style="text-align: center;"><a href="https://heroku.com/deploy?template=https://github.com/crcastle/collaborative-code-conference/tree/master"><img src="https://www.herokucdn.com/deploy/button.svg"></a></div><br>

Or check out a [live demo](https://codie.herokuapp.com).  But note that the video/audio might now work if you or your coding partner are behind a <abbr title="Network Address Translation">NAT</abbr> gateway.  To get around NAT, Twilio provides a [TURN](https://en.wikipedia.org/wiki/Traversal_Using_Relays_around_NAT) service that relays the audio and video.  I've disabled this for the demo app because it [costs money](https://www.twilio.com/stun-turn/pricing) and could generate a huge bill for me if hundreds of people start using it!  Please be nice and don't put anything on here that's private or you can't lose.

#### How does it work?
From a high level there are two main pieces to the app: shared code editing and audio/video conferencing.  Here's a diagram showing how the different pieces communicate.

<!-- [ARCHITECTURAL DIAGRAM] -->

The audio/video conferencing uses [WebRTC](https://webrtc.org) to establish a peer-to-peer connection between your browser and your coding partner's browser.  [WebRTC support in the major browsers](http://caniuse.com/webrtc) is getting better and better.  Peer-to-peer audio and video is fully implemented and stable in Chrome and Firefox.  And while there haven't been any announcements, it looks like Apple is working on adding WebRTC to Safari (hopefully including iOS Safari) in 2016.

The code editor is simply a giant `<textarea>` with some nice styling and functionality added by [CodeMirror](http://codemirror.net).  As you type, text changes are wrapped in changeset objects and shared with other browsers viewing the same document.  You can think of a changeset like a Git commit.  It represents a diff from one document state to another (and a pointer to its parent changeset).  Right now the changesets are sent over a [WebSocket](https://en.wikipedia.org/wiki/WebSocket) to the server and then broadcast out.  A future iteration of the Collaborative Code Conference might use a WebRTC data channel to share changesets obviating the need for an application server beyond a static file server.

#### Questions?
Tweet at me, {% include icon-twitter.html username="crc" %}, if you have questions or comments about this!

<br><br>

<!-- Here are some interesting technologies this project uses:

* [Heroku](https://www.heroku.com) (including [Deploy to Heroku button](https://blog.heroku.com/archives/2014/8/7/heroku-button))
* [Operational Transformation](http://operational-transformation.github.io), which powers Google Docs and Etherpad
* [CodeMirror](http://codemirror.net)'s web-based editor
* [Gulf](https://github.com/marcelklehr/gulf), a transport-agnostic Operational Transformation control layer
* [Twilio Programmable Video](https://www.twilio.com/video) and [WebRTC](https://webrtc.org) -->

[^1]: Developer tooling, coding environment, process are all things developers spend **days** customizing so they can be as efficient as possible.  Some might argue we spend too long tweaking our development environment.
