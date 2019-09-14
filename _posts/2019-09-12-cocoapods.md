---
layout: post
author: jayahariv
title: cocoapods
---
# cocoapods

- [GH](https://github.com/CocoaPods/CocoaPods)

1. the (cross) dependency resolution,
2. (semantic) version management, and
3. automating the ‘integrating it into Xcode’ parts.
4. fetches source code for all dependencies
5. creates and maintains an Xcode workspace to build your project
6. Uses Semantic Versioning to resolve dependency versions.


## Making a CocoaPod
There are only a few differences between a CocoaPod and a generic open source library. The most important ones, aside from the actual source, are the .podspec and LICENSE.


```
# work on the library from its folder on your system
pod 'Name', :path => '~/code/Pods/'

# test the syntax of your Podfile
pod lib lint

# test pod from GH repo
pod 'NAME', :git => 'https://example.com/URL/to/repo/NAME.git'

pod install
# -- or --
pod update

# use to send your library to the Specs repo.
pod trunk push NAME.podspec

# private repo
pod repo push [repo] NAME.podspec
```

# [cocoapods-under-the-hood](https://www.objc.io/issues/6-build-tools/cocoapods-under-the-hood/)


## Core components
CocoaPods is written in Ruby and actually is made of several Ruby Gems.

### what happens when pod install

> Read Podfile & dependency version conflict resolve

1. CocoaPods goes through and makes a list of all defined.
  - explicit and implicit
  - versions. conflict resolution on dependency versions.

> Load Sources

2. Load the source as per the pod spec file.
  - convert podspec to SHAs
  - store in `~/Library/Caches/CocoaPods`
    - Core GEM is responsible for files here
  - Files are then downloaded to `Pods` directory

> Generate Pods.xcodeproj

3. Pods.xcodeproj is updated (if change present)
  - Xcode GEM is responsible.
  - if no files, then create with some default settings.

> Install libraries

4. Add source files to each library
  - `xcconfig`
  - merged config `private xcconfig`  
  - `prefix.pch`
  - `dummy.m`
5. Add Pod Target.
6. `Pods-Resources.sh`, include the resources from libraries to project.
7. `Pods-environment.h`, macros distinguish between pod components.
8. Acknowledge files, `plist`, `markdown`

> writing to disk

9. all the above work(which is in memory) is written to disk
  - Pods.xcodeproj
  - Podfile.lock, resolved versions of the pods.
  - Manifest.lock, Pods directory is not under versioning, this file helps to achieve same.


## Misc
- Search pods: `pod search couchbaselite-swift --simple`
