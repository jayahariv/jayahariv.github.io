---
layout: post
author: jayahariv
title: xcframework
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

## Indirection
1. function call to a framework
  - at runtime is that the client is going to have to ask which method is the `fly` method? And the framework will respond, it's the `<second>` one.
  - it's basically the same way that Objective-C does message dispatch, doing it in a call from one Library to another
  - Swift only does it when you're crossing this client framework boundary
2. use a struct or enum from the framework.
  - the client can't assume that it knows how big the enum is going to be in memory. the client to ask the framework how big is it? And the framework responds, it's just one byte.
	- one of the new cases added in the future might have associated values. client will also ask the framework to cleanup the enum value when it's done with it, and the framework will do so.

## Additional performance
There's three ways to do that: inlinable functions, frozen enums, and frozen structs.
1. inlinable function
	```
	@inlinable
	func someFunctionName() {
		// do some action inside this
		return self.capacity < totalCapacity
	}

	@usableFromInline
	internal var capacity: Int
	```

  - So, as a rule of thumb, if you're a framework author who has made a function inlinable, make sure not to change the output or observable behavior.
	- It's okay to add a better algorithm, or some additional fast pads, but if you change the observable behavior of the function, then you could end up with these really subtle problems that are only visible at runtime, and possibly only under certain inputs.
2. Swift enum
	 So by marking this enum with the frozen attribute, then I as the framework author can promise that there are no new cases added in future releases of the framework.
	 ```
	 @frozen public enum SomEnumName {
		 case someCaseNumber1
		 case someCaseNumber2
	 }
	 ```
	 - The clients are able to assume that this enum won't have any additional cases and won't require any cleanup.
	 - adding a new case to a frozen enum is both source and binary breaking and requires incrementing the Major Version and asking all clients to recompile.

3. Swift Struct
  By default, a Struct in a binary framework can have new stored properties added, or the existing ones reordered without any trouble, but that does result in that same sort of handshake and extra communication between the client and the framework.
	```
	@frozen public struct SomeStructName {
		public var someProp
	}
	```
	-  And the other thing that this does is require that the stored properties all have types that are public, or usable from inline.


## More info needed
* You can also have a variant for Mac apps that use AppKit, and a variant for Mac apps that use UIKit.
* And not only can you bundle up frameworks, but you can also use XCFrameworks to bundle up static libraries, and their corresponding headers.
