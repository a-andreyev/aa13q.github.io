---
layout: post
title: "Sailfish Xiaomi kenzo port: aarch64 kernel with armv7hl rootfs"
date: '2017-05-20T17:27:00+3:00'
categories: posts
lang: en
url: jolla-libzypp
tags:
- Sailfish
- SailfishOS
- PackageKit
- pkcon
- Xiaomi
- kenzo

---

# Problem

I've bought Xiaomi kenzo (Redmi Note 3) nextday after I've found [XDA theme](https://forum.xda-developers.com/redmi-note-3/development/rom-sailfish-os-2-0-5-6-kenzo-t3571258) about the port and donated to [Akhil (porter)](https://github.com/akhilkalwakurthy) to support him.

Akhil said he lost his xiaomi and stayed with some lumia, so his builds are requiring heavy testing (he just building it).

I created [merproject wiki page](https://wiki.merproject.org/wiki/Adaptations/libhybris/Install_SailfishOS_for_kenzo) to provide current status information.

I've never tried to port SFOS by myself yet, but I've found error with this port when using `pkcon` or `qt creator Sailfish sdk`:

    Error: /home/nemo/RPMS/harbour-foobar-0.1-1.armv7hl.rpm has wrong architecture: armv7hl

But zypper worked fine (zypper, Jolla Store, WareHouse)

# Researching

I've searched for code:

`https://git.merproject.org/mer-core/PackageKit/blob/master/PackageKit/backends/zypp/pk-backend-zypp.cpp` for the pkcon backend `backend_install_files_thread ` method:

```
    ...
    // check if the file has an acceptable architecture 
    if (rpmHeader->tag_arch() != ZConfig::defaultSystemArchitecture () &&
        rpmHeader->tag_arch() != Arch_noarch && 
        ! system_and_package_are_x86 (rpmHeader->tag_arch ())) {
            zypp_backend_finished_error (job, PK_ERROR_ENUM_LOCAL_INSTALL_FAILED,
            "%s has wrong architecture: %s", full_paths[i],
            rpmHeader->tag_arch ().asString (). c_str());
            return;
        }
    ...

```

where `system_and_package_are_x86` is some very stange method (because there are better separated compability methods in PackageKit afaik),
but we are on ARM,
so we are more interested in `ZConfig::defaultSystemArchitecture ()`:

it is located in `libzypp` library, which are actually is outdated a little bit if you look at the port:

`$ zypper if libzypp`:

```
    ...
    Version: 12.2.0+git5...
    ...
```

so I've searched for `12.2.0+git5` tag and [found it](https://git.merproject.org/mer-core/libzypp/blob/master/libzypp/zypp/ZConfig.cc), there's a `defaultSystemArchitecture()` method, which is requesting `_autodetectSystemArchitecture()` method with the part for ARM:

```
    ...
    Arch architecture( buf.machine );       MIL << "Uname architecture is '" << buf.machine << "'" << endl;
    ...
    else if ( architecture == Arch_armv7l || architecture == Arch_armv6l )   {
        std::ifstream platform( "/etc/rpm/platform" );
        if (platform) 	{
            for( iostr::EachLine in( platform ); in; in.next() ) {
                if ( str::hasPrefix( *in, "armv7hl-" ) ) {
                    architecture = Arch_armv7hl;
                    WAR << "/etc/rpm/platform contains armv7hl-: architecture upgraded to '" <<
                        architecture << "'" << endl;
                        break;
                    }
                if ( str::hasPrefix( *in, "armv6hl-" ) ) {
                    ...
    ...
```

my `/etc/rpm/platform` contains apropriate architecture info:

```
    armv7hl-meego-linux
```

but
`uname -m` is returning:

```
    aarch64
```

I guess it's because my kernel is `aarch64`, but rootfs is `armv7hl`. And I'm not sure why [some guys from redhat bugzilla declaring they are incompatible](https://bugzilla.redhat.com/show_bug.cgi?id=1326312) (maybe I'm missing something)

anyway, current libzypp is outdated, so there's no headers for `aarch64`.

I've tried to build it from master merproject core branch, but updated `boost-devel` and `libsolv-devel` are required for that too.

So I've decided to patch it according to [this example from vanilla libzypp](https://github.com/openSUSE/libzypp/commit/15845713eb5fbe5650e3dc232a495978c5a6dcea) manually with [quilt](http://www.shakthimaan.com/downloads/glv/quilt-tutorial/quilt-doc.pdf)

As you can see, there's no autodetect support for aarch64 even for [vanilla git opensuse branch of libzypp](https://github.com/openSUSE/libzypp/blob/master/zypp/ZConfig.cc).

Don't know why yet, but will try to figure it out.

## Solving

So I've decided to add additional arch check manually for now at `ZConfig.cc`:
```
    ...
    else if ( architecture == Arch_armv7l || architecture == Arch_armv6l || architecture == Arch_aarm64)
    ...
```

and added mentioned headers.

I've tried to build it manually via `mer-sdk`:
```
    ...
    # as user mersdk
    ssh -p 2222 -i ~/SailfishOS/vmshare/ssh/private_keys/engine/mersdk mersdk@localhost
    # Phone
    sb2 -t SailfishOS-armv7hl -R -m sdk-install
    ...
    git clone ...
    git submodule update --init --recursive
    ...
```
but it asked a lot of time with my laptop. So I've [forked current legacy repo](https://git.merproject.org/aa13q/libzypp/commits/12.2.0+git8) and used [my obs account](https://build.merproject.org/package/show/home:aa13q:kenzo/libzypp) to build it.

Anyway, everything worked!

I've installed rpm with zypper and pkcon started working.

Now I'm able to use qt creator SDK without any problems with RPM deployment! :)

# Future work

I'm archlinux user, so this environmet is not familiar for me yet.

And maybe I'm missing something general about ARM architecture.

[Going to ask Andrew Branson about current status](https://git.merproject.org/mer-core/libzypp/merge_requests/15) and will try to contribute to apropriate solution.
