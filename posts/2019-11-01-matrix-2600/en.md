# Matrix IM @ 2600 in russian (english translation)

I've made a presentation "Matrix. Instant Messaging System: Theory and Practice" at monthly [2600 meetings](https://www.2600.com/meetings/mtg.html) at [Saint Petersburg, Russia](http://2600.spb.ru/) on November 1st, 2019.

Disclaimer. It was hard to find the time for preparation. I decided to make a presentation no matter I'm not good enough at the subject. Here's the text retelling of the speech translated to english. Comments from the audience are marked as bold italics. Corrections and clarifications have been made to the text. It's nor a literal, neither as charm as live presentation :) Special thanks to [kitsune](https://matrix.to/#/@kitsune:matrix.org) for the help and additional comments for the publication.

Feel free to [send me](https://matrix.to/#/@aa13q:matrix.org) the comments about the mistakes or typos.

![2600 spb event](https://i.imgur.com/MCqKoCo.jpg)

![Matrix. Instant messaging system: Theory and Practice](https://i.imgur.com/YdrXn2h.png)


— ***Alexey is going to talk about Matrix protocol and messengers***


— I'm going to tell you introduction words about what it is. An try to tell a little bit about my contribution maybe.


Let's start from the beginning. I've taken the previous presentation as an inspiration, where the presenter based the design on a movie. Does anyone know from which movie the frame at the slide is based on?

![](https://i.imgur.com/hM9Ge5F.jpg)


***— Some "Hackers"? :D***

— No, no. End of the eighties. Carpenter is the director of the movie. Okay, it's called ["They Live" ("Aliens among us" in the Russian distribution)](https://www.themoviedb.org/movie/8337-they-live). It's an old movie about some guy looking like a professional wrestler, who is getting the sunglasses and realizes that not all the people are acting good and [the reckless consumption and careerism are bad](https://twitter.com/thehorrormaster/status/816486706186596352). Thank Carpenter for the cool movie!

***— Obey, consume!***

— Yeah! The tagline is something like: "they live until you sleep". So, I'm going to make silly jokes and cultivate the analogies with the subject. Like, we are all using client software, but do not care about the servers in general...

## Introduction

There is [the protocol](https://matrix.org/docs/spec/) and [the implementaion](https://github.com/matrix-org) called [Matrix](https://matrix.org/). Developed by the Matrix Foundation. Why Matrix? Not because of the movie, but because "matrixed together" have mathematical meaning. So, the network should [matrix together](https://twitter.com/matrixdotorg/status/841424770025545730) all the fragmented communications, that we have right now.

[It is](https://www.reddit.com/r/privacy/comments/da219t/im_project_lead_for_matrixorg_the_open_protocol) an open end-to-end encrypted communication layer for the internet for any kind of data. So it's not some kind of internet replacement or IP-stack replacement. IIRC, it's on the layer lower than the application layer and layer higher than...

***— Physical layer :D***

— And should help us made better communications.

## Goals and objectives

The first goal of the project is to fix the fragmentation problem of IP communications. [Current messengers](https://youtu.be/KbWfzyQBWrU) who becomes [mainstream](https://www.youtube.com/watch?v=xzX1mYVtZJg) are all centralized, handcuffed within itself. That's why our communications are fragmented: we could not, for example, say "let's meet" to somebody from the telegram via your skype account. It's the first problem.

***— Jabber transport anyone?***

— Yeah, we will talk [about the Jabber too](https://youtu.be/AlndCl30OyI?t=211) :)

The second task is to make the solution mainstream. It has to be non-marginal and be mainstream enough to compete with WhatsApp, Skype, Slack, etc.


***— Yet another standard... :D***

— I hope after the presentation it would be clear that it's not some regular "yet another standard", as we all love to joke with [xkcd](https://xkcd.com/927/).

Third, the next aim is to give users an option to control their data and third-party providers.

Final task: move from federated to the peer-to-peer solution. Made communications as simple as in real life, when we talk to each other...

***— Without intermediaries.***

— Yep, as we all would love to.

***[kitsune] The longer term goal is not only about that.***

***[kitsune] The longer term goal is to make IM communications free from the vendor lock-in environment similar to e-mail. But even that is just the part of the case :( It's probably better to access the [FAQ](https://matrix.org/faq):***

***"The longer term goal is for Matrix to act as a generic HTTP [not necessarily] messaging and data synchronisation system for the whole web - allowing people, services and devices to easily communicate with each other, empowering users to own and control their data and select the services and vendors they want to use."***

## Relevance

I guess relevance is clear to everyone. Mainstream solutions are still centralized, fragmented and closed. Talking about instant messaging or some video conferences, we don't have a solution similar to email right now, for example. When you aren't tied to some server and able to write to another one.


## Subject and object

Let me emphasize, what is the project is and what isn't. It's standard upon the IP stack and it's not going to be yet another application protocol to replace all the layers of the communications.

## Working hypothesis

What the developers are trying to check in practice is: is it possible to build a more secure and less centralized IM solution over IP communications, convenient enough for mass usage. Classic problem is to find a balance between security and usability.

## Methods and methodology

Que faire? For the current iteration the client, the server, app service, and identity center terms are introduced. Important parts are [the client](https://matrix.org/docs/spec/client_server/latest) and [the server](https://matrix.org/docs/spec/server_server/latest). [Identity servers](https://matrix.org/docs/spec/identity_service/latest) are a bit harder part for me, let's look at them later. The servers could talk to each other and share the history, while the clients could work with any of the servers.

![](https://i.imgur.com/GHfPstn.png)

It worth to mention git analogy probably. When we keep some code via git, even if we remove some repo but didn't removed all the clones, the code is safe. Same here with the messaging history: if one of the servers is removed or unavailable, the access to the history is safe.

***— What about privacy in this case?***

— Yep, it's a problem. They're trying to [solve it](https://youtu.be/-ofZMnKkp_Y). As a minimum, by adding end-to-end encryption. To test, deploy and develop such solutions, a lot of the time (about several years) was spent for an encryption library and security audit. It's called [olm](https://gitlab.matrix.org/matrix-org/olm/).

![](https://i.imgur.com/Dg3Gw0U.png)

[Olm](https://en.wikipedia.org/wiki/Olm) is a salamander from Europe.

***— Like lizard? Looks like axolotl...***

— Yep! Why it looks similar: because they're also using Double Ratchet principles, similar to Signal (Whisper Systems) — the current de-facto standard for the encrypted messaging. Matrix developers wrote alternative implementation and called it olm. But they use it differently: only as initialization part of the more complex scheme. It's called megolm for the group of messages: not only one by one but the groups. By the way, "group" term is primary in the current implementation, while one-by-one communication is only a particular case of group communication. I will mention more about it later.

Talking about all the scary terms like AES, SHA, Curve, Double Ratchet and Ratchet, — they probably deserve an additional story. I'm not ready to talk about them [right now](https://youtu.be/-ofZMnKkp_Y).

Interesting part is [an interaction between Matrix and Whisper Systems](https://news.ycombinator.com/item?id=11725827). I don't remember any additional details, but the Matrix team thought about potential compatibilty if Moxie and OWS agreed to accept it.

Anyway, Matrix developers provided their implementation. They ordered the security audit, spent a lot of time.

***— And money***

— Yes, and money.

***— Are you talking about security audit, similar to any other encryption solutions?***

— [Yes](https://matrix.org/blog/2016/11/21/matrixs-olm-end-to-end-encryption-security-assessment-released-and-implemented-cross-platform-on-riot-at-last). They decided not to dive into standardization via standardization organizations like IETF and IEEE right now because it will take even more time. But they spent the time on the audit first of all.

Interesting moment: it is possible to turn off the end-to-end encryption. It is handy for the massive public groups when it's more important to provide public history, not to follow the encryption part.

***— What's the point with making the encryption optional?***

— It's hard to make e2ee communications convenient in the large public groups, to share the keys. And to support that process among a lot of people.

***— There's also the synchronization problem. XMPP/Jabber had e2ee a long time ago, but it had problems like: you could lose your messages if you are offline. I remember a lot of problems with that.***

— Talking about Jabber, they have the main document and additional extensions. In the end, it became a mess with extensions and client support. Matrix is going to support a unified single document without extensions.

***— Are they using some old existing encryption or some new one?***

— They took existing principles (not ready to talk about it right now, like Double Ratchet, etc) — same are in Signal. But they provided their implementation because they needed to support group messages, not just one-to-one.

Indeed, it hard to provide a convenient secured solution. For example, you have to deal with the key sharing, storage, distribution and do not mess with that. Especially with the UX/UI part.

***— So general key sharing solutions are not working there?***

— They're working. But practice will tell, how to provide better solutions.

## Novelty

Novelty is clear too, I guess. Why Matrix differs from Signal, for example, Matrix is not going to become yet another application protocol, it's an attempt to create an infrastructure to interconnect these protocols. Something like e-mail, but for instant messaging with history, encryption, decentralization, interoperability, for any data.

***— I'm interested in the details. So you are talking about decentralization. Are we going to trust servers or not? Could the server read the collected data?***

— If we are not using end-to-end encryption, we have to trust our home server. And we could deploy our own since it's open-source. If we turned on end-to-end encryption, we don't have to trust any server at all.

***[kitsune] This is not entirely the case. First, the metadata is protected only in terms of integrity. Metadata is not encrypted and accessible throughout the federation. Secondly, the clients are very much forced to trust their servers. Your access token is located on the server. Even more, the server could generate a new device for your account and send messages from your account (although yes, the rest will have to trust this new device first). The server cannot decrypt encrypted content, but stating that the server does not need to be trusted implies something more broad concept.***

But it's just an intermediate iteration. The global aim is to get everyone into peer-to-peer. Firstly, to teach people to deploy their servers, not just to use only one. After that, by combining the server and the client into a new entity on the device (when it will be ready to do it) we will get full peer-to-peer.

***[kitsune] Peer-to-peer is good, but never universal. Typically, P2P brings most of the problems :) I also presented on Saturday, and I came up with these about the individual owners of home servers: suddenly you have an HTTP server in the corner of your room that serves the entire Internet.***

***[kitsune] Some problems are old and long-known. For example, the "alien traffic" problem, when your device accidentally become a convenient transshipment point of the current topology. Many of the peer-to-peer applications are a complete mess for those connected with traffic restrictions.***

***— How could it be peer-to-peer, if we have servers?***

— If you combine the server and the client, there will be no servers anymore in the ordinary sense of the word, it will be a more complex term.

Talking about clients by the way. IIRC, unlike Signal, the client is the actual device at the Matrix. For example, you are talking to someone via his home PC and found a new device of your conversationalist. For example, from an Egypt cafe. You could not trust the exact device and your history will not be shared with the exact device, but will be shared to the trusted account devices.

***— On the one hand, it's a bonus, on the other — additional challenges. You have to deal with all the devices during the synchronization process, share the keys.***

— Yes, there're work and experiments with the UI part: like emojis coding, QR-codes sharing, etc.

## Degree of maturity

![](https://i.imgur.com/3VtCDbS.png)

Current state. As you could imagine by the slide with homeless people from the movie, the degree of maturity is not perfect... :)


***— Well, you are cunning definitely! :D***

— The encryption library is already here and even that is a big deal in general.

***— There's also a lot of nodes, connecting Matrix with the other networks***

— [Bridges](https://matrix.org/bridges/)

***— Yes, bridges! There's a lot of them already: IRC, XMPP, Slack, Telegram and a lot more***

***— I guess it could be a bottleneck in terms of security***

— Not sure about that, we could talk about it later.

Anyway, there're already a [client-server](https://matrix.org/docs/spec/client_server/latest) interaction, [server-server](https://matrix.org/docs/spec/server_server/latest) interaction. There's [python-based server implementaion](https://github.com/matrix-org/synapse). By the way, it was a reference implementation just to check everything. But due to the popularity of the project, it became the battle one.

***— Hah***

— Protocol version 1.0 was released [this year](https://matrix.org/blog/2019/06/11/introducing-matrix-1-0-and-the-matrix-org-foundation). There's a plan to move from python to the [go implementation of the server](https://github.com/matrix-org/dendrite). There's also a [Rust based project](https://www.ruma.io/). They are in the early stages of development. Everyone is using python version right now.

There are some problems with scalability and performance of course. But what we also have is client [SDK for Android](https://github.com/matrix-org/matrix-android-sdk), and [ iOS](https://github.com/matrix-org/matrix-ios-kit). There's also a new one Android client [RiotX has written in Kotlin](https://medium.com/@RiotChat/introducing-the-riotx-beta-for-android-b17952e8f771). There's [Qt SDK too](https://github.com/quotient-im/), I'm contributing to the SDK. There're [Rust](https://github.com/ruma/ruma-client), [JS](https://github.com/matrix-org/matrix-js-sdk) client libraries. [In gerenal](https://matrix.org/docs/projects/sdks/), since we have REST API with JSON it is possible to send the messages even via curl — nothing stops you from that.

***— I have a small question. You said earlier that Matrix is a layer above IP communications. But from the recent words, I could conclude it's even on the top of the HTTP?***

— HTTP transport is using to grow faster. But I have a backup slide with [cool stuff](https://www.youtube.com/watch?v=DZBvy4abB1o) using UDP/CoAP/etc for the low-end devices with a weak channel.

***— So it's a recommendation, not hardcoded into the standard?***

— Looks like yes, but you could check [at the spec](https://matrix.org/docs/spec/client_server/latest#api-standards).

***[kitsune] In fact, it is tied up enough now. Look [here](https://matrix.org/docs/spec/client_server/latest#api-standards) for example. There're enough mentions of the JSON over HTTP(S) through the document.***

***[kitsune] The statement, however, is that other protocols could be supported with minimal changes to the client and server code. And neither JSON nor HTTP is an integral part of Matrix. It just uses them as a substrate. Well, in the meantime, there has not been a single person who really would have to draw up the average agnostic protocols. The PoC for the transfer via another environment was done, the hypothesis was confirmed.***

***[kitsune] Some people tried to say something like COPR is more compact and should be used and that we're unreasonably spending the bandwidth. What PoC showed is that actually TLS is the biggest overhead in terms of the payload side.***

***— Is it some kind of Layer 7? :D***

***[kitsune] As a note, Matrix uses application-level encapsulation. Formally, all these REST calls, no matter which substrate they use, remain at the application level. The OSI model does not fit the hierarchy that appeared on top of HTTP at all :( You know how networkers are.***

***[kitsune] Hypothetically, Matrix could be done upon bare IP by writing its own equivalent of some QUIC.
So we can say that conceptually it really is somewhere between the application and transport layers.
And "accidentally" is implemented in terms of a pure application level at the moment.***

***— Okay more practical question: is it working via web-browser?***

— Yes, [it works](http://riot.im/).

***— Awesome!***

— For me, it would be awesome to have something except the web browser :D That's why I'm contributing to [the Qt based library](https://github.com/quotient-im/).

## The approbation, the demo

Let's look at the demo.

***— While you're preparing, I want to mention that there are some QML-based browsers, that are not using HTML/CSS/JS***

***— Hehehe***

— Cool, but I've heard about similar things earlier, we have to look at it later.

***[kitsune] The first real project for browsers with QML worth mention is called [Qt for WebAssembly](https://doc.qt.io/qt-5/wasm.html), which was created recently :) And it is VERY interesting. I want to find time to try it. The size of the WebAssembly binaries are still frankly scary :( It's no wonder since it's using emscripten right now.***

So, we have a demo on the main page ["How does it work?"](https://matrix.org/).

***— Beautiful!***

***— Is it some kind of multicast?***

***— I was never interested in Matrix previously, I thought it some kind of cult, but I'm very interested right now :D***

— I don't know how should compare it with multicast, not ready to provide the analogy.

***[kitsune] There is no multicast in the Matrix, like, at all. The multicast is not projected on E2EE in any way, by the way :)***

***— How the routing works?***

— It's the next animation of the demo

***— Oh, is it some kind of blockchain? :D***

— There's some comparison with blockchain style, but I don't remember it right now

***— Hold on, it not a blockchain! :D***

***[kitsune] Each room is a so-called directed acyclic graph of Merkle, a subclass of which are Merkle trees, commonly referred to as blockchain :)
Another project that uses directional acyclic Merkle graphs is IPFS.
Etherium, Bitcoin and many others use ordinary Merkle trees.***

— The demo shows how synchronization works

***— So everyone gets all the messages, and someone decides what to answer?***

— All the history is synced via all the servers. If you turn off your home server, it will not ruin the system.

***— Is it safe?***

***— It's encrypted***

***— Is it already encrypted?***

— Depending on the situation and your tasks, you could select end-to-end encryption or trust the home server.

***— Is it possible some third party server I trusted decrypt all my messages when I lost and forgot my keys there, for example?***

***— You don't share the private key with the server***

***— It's end-to-end encryption***

— I will show the [web-client](https://riot.im/), which is, *unfortunately for me*, is the most developed one... :D

***— Is it Riot?***

— Yes, Riot. There's a public Matrix server, but they are going to power it off when everyone will be ready with self-hosted servers. We could look at the Riot here.

There's a list of all the rooms from all the servers. You could filter them with the "communities" list.

There's a notification about my presentation in [russian speaking matrix room](https://matrix.to/#/#ru.matrix:matrix.org).

***— Is it reactions?***

— Yes, there're reactions and replies. There are also widgets supported at the header of the room. For example, some temperature widget.

***— To steal the cookies! :D***

***— Is it possible to execute JS via the requests? :D***

***— I have a question: what about messaging model? Is it more like Telegram with linear answers list or is it more like Slack with the branches of the answers?***

— You don't have to make the clients all the same. Let's take a look at [the spec](https://matrix.org/docs/spec/client_server/latest#rich-replies).

***— We are asking about it because we don't want to get a mess at the support channels when everyone is trying to help to the new member***

— You could implement threads, there's [the details in the spec](https://matrix.org/docs/spec/client_server/latest#forming-relationships-between-events) and additional discussion [at the issues](https://github.com/vector-im/riot-web/issues/2349). I recommend to check it.

***[kitsune] Threads are a sore subject actually. Now the answer mechanism is more or less specified, but the communication and visualization of discussion threads are now in a very embryonic state. But we are working on it.
Key proposal for spec at the adoption stage: [MSC1849](https://github.com/matrix-org/matrix-doc/pull/1849/files)***

***— So, is it like LiveJournal? :D***

***— You said all the servers replicate all the data. What about scalability? Especially in terms of p2p...***

— Time will tell! :D

***[kitsune] This is one of the reasons why p2p is a problematic solution. Matrix actually replicates not all the messages but only needed for the home server and all the clients. It is clear what messages clients want, they running with syncs and asking for history (limited part of the history most of the time). But home servers also need all the info about access control (power levels, joins/leaves, bans...) to check it according to current room rights state. They must pull the whole chain of the so-called authentication events i.e. everything that relates to changes in these rights. And it is a lot in case of the large rooms such as Matrix HQ :)***

***— So are you just taking all the world history to your home server? Everything to everyone automatically?***

***— Well, blockchain is working like that...***

***— Blockchain? Are we going to wait two days for a message?***

— AFAIK, there are solutions like ["share lists"](https://matrix.org/docs/spec/client_server/latest#server-access-control-lists-acls-for-rooms). But I don't know how it's going to be implemented in the future.

***— So it's an obscure aspect?***

— It's the obscure aspect for me :)

***— It's a nonsense in terms of the end-user! I'm not going to use a client with all that checkboxes to follow!***

— You don't have to follow the checkboxes. It's on the shoulders of the home server admins.

***— The point is you could choose between providers and terms of service***

— Look, guys. I've made the ["2600 meetings in Russian" room](https://matrix.to/#/#ru2600:matrix.org). We could use a bridge for the telegram and lure people here. Oh, it's already two persons here!

***— To say the truth, it's kinda slow***

— The public server is slow right now, yes. We should deploy our server and contribute to the code.

***[kitsune] It is slow mainly because the user graph is terribly unbalanced. A bunch of people sitting on one poor [matrix.org](https://matrix.org/) instead of being hosted out on separate servers. When everyone is more or less engaged in their rooms, and only part of the communication really flies through the entire federation.
But here we have what, the price of explosive growth.***

***— Tell me about the main difference between the Matrix and the Jabber?***

— Jabber is not focused on the history sharing, it's about message sending. And Jabber has the minimal protocol and the extensions. Here we have a unified document.

***— I didn't get it***

— You should try it and look inside

***— Is ICQ "fundamental" problem solved? Could I send a 1 Gb file to another person? Is it going to be shared with all the servers?***

— AFAIK, files handled separately, we should  [read the spec](https://matrix.org/docs/spec/client_server/latest#m-room-message-msgtypes) about that.

***[kitsune] Everything is very simple with files: the file is sent to one ("your") home server, and from there it is accessible to everyone else.
The gigabyte does not fly through the federation, of course.
(That is, it flies, but only for those who want to download it, which is actually an even bigger drawback for distributing large files. In general, Matrix is not yet a replacement for torrents for this purpose).***

***— Let's wait until memory became cheaper and the channels became faster :D***

— By the way! There's an [issue about IPFS](https://github.com/matrix-org/matrix-doc/issues/539).

Let's try to connect [b3cksp4c3 hackspace](https://matrix.to/#/#b4ck5p4c3:matrix.org) with telegram too.

## My contribution: GSoC 2019

Let's go back to the slides. I took part in the [Google Summer of Code](https://summerofcode.withgoogle.com/archive/2019/organizations/4888795948777472/). I've contributed to the [Qt client library](https://github.com/quotient-im/libQuotient). There's a library, shared between several Qt clients: [Quaternion](https://github.com/quotient-im/Quaternion) and [Spectral](https://gitlab.com/b0/spectral). It doesn't have E2EE support yet. During the summer, I've managed to implement message receiving, but have to finish message sending and contribute to other parts.

I used an existing library with the Qt wrapper.

***— You have to be careful about existing libraries, especially crypto. For example, be careful with the salt.***

— I've used official [community library from the repo](https://gitlab.matrix.org/matrix-org/olm/). By the way, they share it on the self-hosted Gitlab, not Github just not to deal with the additional bureaucracy.

***— Silly question: is the encryption symmetric or asymmetric?***

— AFAIK, it depends, you have to deal with different types.

***[kitsune]: there is a simple answer: both one and the other :))
For different things, different.
Messages are encrypted with an asymmetric protocol. Files with a symmetric one, for example***

## Conclusion, current challenges

Сurrent challenges. It was mentioned at the beginning of the year and some of them could be solved already. Canonical DMs. Reworking Communities (i.e. groups of rooms). Decentralized accounts (i.e. letting users migrate between or exist on multiple servers). Lots of server performance and scalability improvements. Peer-to-peer Matrix and resistance to metadata analysis.

***— It looks like metadata protection is similar to blockchain principles when you have to use additional tools for that***

— As hook-ups, I will show you some parts of the  [FOSDEM presentation](https://archive.fosdem.org/2019/schedule/speaker/matthew_hodgson/) about 100 bit/sec for low-end devices and VR demo.

***— Okay, we should view the full version***

— I also could tell you about financial details.

***— Booyah***

— It's not an inside, [just public information](https://www.reddit.com/r/privacy/comments/da219t/im_project_lead_for_matrixorg_the_open_protocol/f1nwlra/). Firstly the telecommunication company backed Matrix, then let it go on a free voyage. The commercial company was organized to form the resources. After that, they found [French state contract](https://matrix.org/blog/2018/04/26/matrix-and-riot-confirmed-as-the-basis-for-frances-secure-instant-messenger-app), where a similar system is needed for many departments. After that they found [additional funding](https://matrix.org/blog/2019/10/10/new-vector-raises-8-5-m-to-accelerate-matrix-riot-modular).


It's interesting how [the French project](https://www.youtube.com/watch?v=C2eE7rCUKlE) has to deal with E2EE and antivirus checks. It was solved nicely by checking the files on the clients before and after the transmission with a connected antivirus solution.

This spring they had [internal infrastructure security incident](https://matrix.org/blog/2019/05/08/post-mortem-and-remediations-for-apr-11-security-incident) with kind Github issues about the problem :D After that the clear and detailed report was provided with the notes how to prevent it in the future.

***— Question: who owns most of the current public servers? Is the situation, which some afraid of with the Tor, when most of the nodes could be owned by NSA, is possible here?***

— Security is the question of trust. Use the server you trust. You don't have to trust other servers.

***— We have to read about it more and try it***

— Thank you!

***— [claps]***

## Questions

![](https://i.imgur.com/dPu0sW5.png)

**P.S.: questions after the meeting**

***— I like the [console client](https://github.com/tulir/gomuks) better. In my workflow with a lot of consoles, hotkeys and tiling wm it's the most efficient :)***

***— How much time are you spending on the Qt-client?***

— All the free time in the summer with some timeouts. Right now I don't have time at all, but willing to get back out there.

***— What about identity servers?***

— We have to talk about them separately, I'm not ready to talk about it

***— Cool, the authors are just in time with this idea!***

— I guess the idea is in the airs. It's the question of resources and implementation.

![](https://i.imgur.com/owDnbqG.png)


*The slides:*
+ [gdrive: in english](https://docs.google.com/presentation/d/1VTg13hHCAa1pdFHVh8A_-rtY3qfYTkI_yqaJM-jZwek/edit?usp=sharing)
+ [gdrive: in russian](https://docs.google.com/presentation/d/1HEYBmolqHVAg6xg7nPcrFwxvyGZ7scwpYpti75pMDok/edit?usp=sharing)

*Special thanks to zafod, kitsune and Kaffeine for support and kitsune for great external comments! Schemes from the slides were taken from the original [ara4n](https://matrix.to/#/@matthew:matrix.org) slides, thank him for [them](https://archive.fosdem.org/2016/schedule/event/matrix/) and [the matrix](https://www.youtube.com/watch?v=eA0KnTt4O7E) :)*

*I have come here to show the slides and drink up… and I'm all out of the slides.*
