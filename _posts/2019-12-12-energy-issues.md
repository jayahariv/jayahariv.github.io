---
layout: post
author: jayahariv
title: energy efficiency
---
# Energy Efficiency
Energy = power * time;

## References
- https://developer.apple.com/videos/play/wwdc2015/708/

- in iOS and mac, we could see the most energy used apps and check it.

## Reduce Energy
- do it never/do it less
- do it a better time
- do it efficiently

## debugging workflow
1. general debugger
2. focused debugger
3. repeat above steps

## iOS
1. Location - time
2. Network -  time
3. Background - use UIBackground task

## General Guidelines
- background: use the UIBckground tak begin and end when its done.
- network: sync on demand, or through push notification.

## Tools
### Energy Gauges
- high level of the energy usage
- component wise, time series or go to profile via instrument

### Instruments
- root cause analysis
- in depth profiling
- untethered profiling run on device.
