---
layout: post
title: "Qt Quick Controls 2 Silica Theme"
date: '2018-03-13T09:15:00+3:00'
categories: posts
lang: en
comments: true
url: qqc2-silica-theme
tags:
- Qt
- Qt Quick Controls
- Sailfish
- Silica
---

# Problem

[Sailfish Silica](https://sailfishos.org/develop/docs/silica/) controls were developed when there's no such thing as Qt Quick Controls (neither [first](https://doc.qt.io/qt-5.10/qtquickcontrols-index.html) not [second](https://doc.qt.io/qt-5.10/qtquickcontrols2-index.html)).

[Qt Quick Controls public introduction](https://www.youtube.com/watch?v=_6_F6Kpjd-Q) -- July 2013.
[Jolla announced it's Sailfish OS and UI at Slush 2012](https://www.youtube.com/watch?v=bdLUJZR078k
) -- November 2012.

So right now we have separated technologies for the Sailfish OS controls and *desktop/other-Qt-supported-platform* controls,
that are duplicating their functions.
It's harder to maintain a cross-platform app for Sailfish OS too.
Аnd improvements from Qt team working on general controls are not getting to Sailfish due to that.

# Researching

## Introduction

I really wanted to do it since I've bought my first Jolla phone several years ago. I've asked this question at Russian-speaking community meetups and chats. I've received feedback like:

*"Hey, you can do it, but you don't need to. Sailfish have some unique UI/UX parts".*

*"All the cross-platform teams I know are sharing C++ business logic and implementing native look-and-feel on every platform separately"*

It's true and I respect this opinion. And it held me for a while... :(

## Real-life examples

But when I'm writing my application, I still do need to rewrite GUI completely.

For example, I'm writing some [banking application with balance and transaction list](https://github.com/a-andreyev/harbour-cosmonaut). Have to rewrite ListViews, Buttons and other components from Silica to Qt Quick Controls if I want to run it on Linux/KDE for example. But with Android, I could reuse Qt Quick Controls with native look and feel.

Another example -- [multimedia IPTV application](https://github.com/a-andreyev/harbour-chastv). I want to build it for my DIY-smartTV (arm device + CRT TV :) -- no way, have to rewrite UI part completely repeating similar parts.

It not very hard to write several versions. But it's harder to maintain them.

There's also similar questions at TJC [[1](https://together.jolla.com/question/71520/request-qtquick-controls/),[2](https://together.jolla.com/question/98438/develop-application-using-standard-qt-modules/)] and recent [Collaboration Meeting Question](http://merproject.org/meetings/mer-meeting/2018/mer-meeting.2018-03-08-09.03.log.html) from marmistrz.

## Related work

M4rtinK have to provide [additional layer](https://github.com/M4rtinK/universal-components) his [modrana](https://github.com/M4rtinK/modrana). It is an option, but I guess if we could implement Qt Quick Controls 2 Silica style there will be no need for an additional layer.

Qt Quick Controls 2 (QQC2) already have [Default, Fusion, Imagine, Material and Universal styles](https://doc.qt.io/qt-5.10/qtquickcontrols2-styles.html). It will be definitely possible to provide Silica Theme and I've received positive comments about my attempts from Joona Petrell at Jolla via email.

My friend eetu, locusf and neochapay are implementing [Qt Quick Nemo Controls](https://github.com/nemomobile/qtquickcontrols-nemo). As far as I know, it's based on `QtQuick.Controls 1.0` and not based on gestures. (`TODO:` ask eetu and Sergey (neochapay) more about that). They working hard to provide a completely open-source solution and it's great. I'm more into silica theme and gestures and QQC2. Qt Quick Controls 2 were rewritten in C++ ([video with details from QtWS16](https://www.youtube.com/watch?v=WIRRoPxIerc)).

Recently we had a conversation with Kaffeine (Alexander) at Russian-speaking chat and he is supporting QQC2 ideas. He provided additional information:

*Current Sailfish app requires around 1500 ms to start: 700 ms is for compiling at first start without invoker) and another 700 ms is for js-based PageStack and ApplicationWindow. Qt Quick based app requires 50 ms.*

*And we could move the Sailfish-specific part to Qt.SailfishExtras. [Example for material style](https://code.woboq.org/qt5/qtquickcontrols2/src/imports/controls/material/).*

*QML is for the declarative part, Quick -- for UI definition. Move complex code to C++. And, yes, PageStack.js — is a complex code. [Example bench from KDAB](https://www.kdab.com/analyzing-performance-qtquick-applications/). [Another publication at woboq](https://woboq.com/blog/qml-vs-cpp-for-application-startup-time.html).*

So `grep -r "duration: 600" /usr/lib/qt5/qml/Sailfish/` and you could find hardcoded animation duration probably for that reasons :)

There's also [Kirigami project from KDE](https://techbase.kde.org/Kirigami) and I'm following their progress but I'm more into Silica. Kirigami is also using Qt Quick Controls 2 codebase.

## Progress

[Played with QQC2 last year](https://www.youtube.com/watch?v=mWgjSnNZxK0).

Still, a lot of work needs to be done. There are also some changes made in mainline LTS 5.9 comparing to current Jolla Qt 5.6, that we have to take into account (at least make additional researching)


## Additional/optional tasks

+ If we are gonna move to Qt Quick Controls 2, it will be easy to provide kind of "inverted" Silica Theme, where we have a bright background and dark fonts but with fancy Silica shaders :) Since we are still refactoring Theme object, we could not hardcode on a dark theme only.

+ It would be super-awesome also to change default auto-generated theme main color based on wallpaper color to an opposite (in terms of hue in HLS) because it will be much more contrast and readable (always have to change auto-generated color to the opposite at my themes).


# P.S.

I do really want to help to implement QQC2 on my favorite mobile OS. Sailfish Silica UI is awesome due to:
+ Attempts to split sensor input and display output via gestures and PulleyMenus. It's so important, obvious and handy for me that I don't even know why should I explain that. I want to thank and shake a hand to everyone who worked on it at Nokia and Jolla (and not only for that)
+ Shared Theme for the same look of applications
+ Fancy blur shaders! :)
