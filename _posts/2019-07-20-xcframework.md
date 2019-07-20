---
layout: post
author: jayahariv
title: XCFramework
---

# XCFramework
XCFramework make it possible to bundle a binary framework or library for multiple platforms —including iOS devices, iOS simulators, and Mac Catalyst — into a single distributable .xcframework bundle that your developers can use within their own applications

An .xcframework bundle can be added to an Xcode target’s Link Libraries phase and Xcode uses the right platform’s version of the included framework or library at build time.

Creation of frameworks is supported from the command line using xcodebuild -create-xcframework.

Frameworks or libraries bundled in an XCFramework should be built with the Build Libraries for Distribution build setting set to YES

## how to

1. Make sure Build Libraries for Distribution build setting set to YES.
2. Xcode build
```
xcodebuild archive
	-scheme <SCHEME>
	-destination <DESTINATION> -destination <DESTINATION2>
	-configuration <CONFIGURATION>
	-archivePath <PATH>
	SKIP_INSTALL=NO
```
  * Skip Install build setting, and set it to No.

2. Xcode create xcframework
```
xcodebuild -create-xcframework
	-output <OUTPUT_PATH>
	-framework <PLATFORM_1_ARCHIVE>
	-framework <PLATFORM_2_ARCHIVE>
	...
```

## Swift Module Interface

When the Swift compiler goes to import a module, it looks for a file called the Compiled Module for that library. This Compiled Module Format is a binary format that basically contains internal compiler data structures. And since they're just internal data structures, they're subject to change with every version of the Swift Compiler.

In order to solve this version lock, Xcode 11 introduces a new format for Swift Modules, called Swift Module Interfaces.

And just like the Compiled Module Format, they list out all the public APIs of a module, but in a textual form that behaves more like source code.
And since they behave like source code, then future versions of the Swift Compiler will be able to import module interfaces created with older versions.

 And when you enable Build Libraries for Distribution, you're telling the compiler to generate one of these stable interfaces whenever it builds your framework.

### Inside module interface
1. Section of meta data.
	* version of the compiler that produced this interface
	* subset of command line flags that the Swift Compiler needs to import this as a module.
2. all the modules that this framework imports
3. the types that are part of the interface.
	1. the public name property is included in the interface, but the private current location property is not.
	2. the public initializer and the fly method are included in the interface
	3. the class has a de-initializer in the interface. If this format is supposed to be stable across versions of the compiler, then the compiler should not make any assumptions about the underlying source code.
4. Enum
	1. the first thing to see is that both cases of the enum are included.
	2. in the interface, there's an explicit conformance to both Hashable and Equitable. in the spirit of being explicit, and not making assumptions, it's included in the Module Interface

## Semantic Versioning
- The smallest component is the Patch Version, and represents when you make bug fixes, or implementation changes to your framework that shouldn't affect your clients.
  - No change to module interface without impacting the behavior of the class.
- The middle component is for backwards compatible editions, new APIs, or new capabilities.
  - New public API is added.
	- Added a new case to an enum
	- Made struct a Hashable.
	- New stored property to the struct.
- And the Major component is for any breaking changes that you have to make, whether that's source breaking, binary breaking, or semantics breaking in a way where clients will have to rebuild, and possibly redo some of their client code, in order to adopt the new version of the framework.
  - adding a new parameter to the function, or signature of the function.
	- a function is uniquely identified by its name, and its parameters. Both the argument labels, and the types.


## Investigate further
* You can also have a variant for Mac apps that use AppKit, and a variant for Mac apps that use UIKit.
* And not only can you bundle up frameworks, but you can also use XCFrameworks to bundle up static libraries, and their corresponding headers.
