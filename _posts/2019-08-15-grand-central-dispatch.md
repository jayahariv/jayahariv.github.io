---
layout: post
author: jayahariv
title: bonjour
---

[WWDC](https://developer.apple.com/videos/play/wwdc2017/706/)

## Parallelism vs Concurrency
- Parallelism more cores are good.
- Concurrency with single core

### Parallelism
System frameworks

- Accelerate built in - adv. core image.
- Metal & CoreML  - powerful GPU.

#### with GCD
- DispatchQueue.concurrentPerform
- optimize for you.
- load balances with the cores available

- objc dispatch_apply(DISPATCH_APPLY_AUTO)


### Concurrency
- subsystem(UI, DB, Networking)
- context switch between subsystem
- visualize it : System Trace in Instruments

- too much context switch is bad.
  - others in the line
  - to gather the new context, and save history of previous context also is going to perform.

- Excessive context switching
  - repeatedly waiting for exclusive access resources
  - repeatedly switching between independent operations
  - repeatedly bouncing an operations between threads

#### Lock contention
- stair case issue
- unfair lock, but starvation can happen.
- check app ussing the instruments.

#### Lock ownership
- run time knows about the next context switch and thread to execute.
- help resolve priority inversion(high priority waiter vs low priority owner).
- primitives with single ownership does lock ownership.
  - serial queue
  - dispatch work item - wait
  - os unfair lock
  - pthread_mutex, NSLock
- No owner - run time does not know
  - semaphore
  - dispatch group
  - pthread_cond, NSCondition
  - Queue suspension
- Primitives with Multiple owners
  - private concurrent queues
  - pthread_rwlock

Optimizing the lock contention
- inefficient behavior are emergent properties
- visualize apps behavior with instruments
- use right lock


# Dispatch(from documentation)
- GCD, operating at the system level, can better accommodate the needs of all running applications, matching them to the available system resources in a balanced fashion.
- When you build your app using the Objective-C compiler, all dispatch objects are Objective-C objects.


## Queues and tasks
1. dispatch_get_main_queue
2. dispatch_get_global_queue: (system-defined global concurrent queue with QoS)
3. Dispatch Queue: (serially/concurrently => main/background thread)
4. Dispatch Work Item: (completion handle or execution dependencies)
5. Dispatch Group:(as a single unit)

## System Event Monitoring
1. Dispatch Source:
2. Dispatch I/O
3. Dispatch Data

### Dispatch Queue
  - Except for the dispatch queue representing your app's main thread, the system makes no guarantees about which thread it uses to execute a task.
  - Attempting to synchronously execute a work item on the main queue results in deadlock.
  - Dispatch queues provide minimal support for autoreleased objects by default.
