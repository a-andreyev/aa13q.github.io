---
layout: post
title: 'Qt qtsystems module: QStorageInfo better partitions UUIDs support (Systems:
  SystemInformation)'
date: '2014-04-06T19:08:00+04:00'
categories: posts
en: true
tags:
- Qt
- qtsystems

source_url: http://bugreports.qt-project.org/browse/QTBUG-38148
tumblr_url: http://aa13q.tumblr.com/post/81886958571/qt-qtsystems-qstrorageinfo-better-uuids-support
---
Qt qtsystems module provides [`QSystemStorage` class][0] at SystemInfo part.

QSystemStorage class have public invokable method:

[`Q_INVOKABLE QString uriForDrive(const QString &drive) const;`][1]

This method works well (don’t forget about sending correct drive strings with backslash in Windows OSes though), when you want to get information, let’s say, for mounting or umounting filesystems.
But you probably could face problem when using this information (uriForDrive) for recognizing removable storage partitions through different operating systems.
<!--more-->

> // For example, I encountered this task year ago: I had cross-platform app, that synchronizes inner databases between running copies, and that databases stored information about already processed storage partitions. So I wanted to have the same identifiers for, let’s say, fat32-formatted removable devices at any operating system to optimize processing.

Different OSes (expect linux-based) uses their own unique identifiers, even though the most popular filesystem formats supports having partition UUID at their superblock (don’t know about all the filesystem formats though):

Windows uses their own GUIDs for mounting:

> [`mountvol`][2] -- console application that could show this GUIDs (without params to watch currently mounted)\\
> [`vol`][3] -- console application for getting filesystem partition serial number (that updates at reformatting and stored at superblock)

Linux uses native filesystem partition serial numbers:

> [`blkid`][4] -- console app that could show this UUIDs (needs root rights)

Mac OS X uses UUIDs, that looks similar to native, but don’t tested it at removable devices yet, not sure:

> [`diskutil`][5] -- console application that could show identifiers that Mac OS X using 

So I suppose it would be nice to implement new `QSystemStorage` method, similar to `uriForDrive(…)`, that returns OS-independent UUID for filesystem partition (volume), that is stored at filesystem superblock.

> // I tried to compile bad hack for my app. Patched qtsystems module [`uriForDrive(…)` windows implementation][6] and it worked (based on [`GetVolumeInformation`][7] function from Windows API. Here is my draft for windows (but, as I said, I guess it should be separated to new method for `QSystemStorage`:

{% gist 5815525 %}

[0]: https://qt.gitorious.org/qt/qtsystems/source/src/systeminfo/qstorageinfo.h "qt / qtsystems @ gitorious"
[1]: https://qt.gitorious.org/qt/qtsystems/source/src/systeminfo/qstorageinfo.h#L76 "line 76 at current QStorageInfo implementation"
[2]: http://technet.microsoft.com/en-us/library/cc772586.aspx "Mountvol command manual @ MS technet"
[3]: http://technet.microsoft.com/en-US/library/cc725860.aspx "Vol command manual @ MS technet"
[4]: http://linux.die.net/man/8/blkid "man blkid @ linux.die.net"
[5]: https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/diskutil.8.html "diskutil manual at developer.apple.com"
[6]: https://qt.gitorious.org/qt/qtsystems/source/src/systeminfo/windows/qstorageinfo_win.cpp#L96 "current QStorageInfoPrivate::uriForDrive Windows implementation @ gitorious"
[7]: http://msdn.microsoft.com/en-us/library/windows/desktop/aa364993.aspx "GetVolumeInformation @ Microsoft Developer Network)"
