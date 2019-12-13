---
layout: post
author: jayahariv
title: activity tracing
---
# Activity Tracing

## References
- https://developer.apple.com/videos/play/wwdc2016/721/
- https://www.objc.io/issues/19-debugging/activity-tracing/


Activity tracing has three different parts to it: activities, breadcrumbs, and trace messages.
(i) Activities allow you to trace the crashing code back to its originating event in a cross-queue and cross-process manner.
(ii) With breadcrumbs, you can leave a trail of meaningful events across activities leading up to a crash.
(iii) Trace messages allow you to add further detail to the current activity. All this information will show up in the crash report in case anything goes wrong.

- Activity tracing is integrated into AppKit and UIKit. (as per 2014 WWDC)
- iOS 10.0, macOS 10.12 -> it is available in os.
