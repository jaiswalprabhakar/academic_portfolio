---
layout: post
title: The Robot That Swam Like a Cuttlefish
date: 2026-07-16 00:00:00-0000
description: Revisiting an underwater robot I built years ago, and why I never wrote about it until now
tags: robotics projects
categories: journal
giscus_comments: true
related_posts: false
---

I've built a lot of things over the years that never made it anywhere near a portfolio, a write-up, or even a proper folder on my laptop. This is the first of a few posts where I'm going back through old projects and finally giving them the write-up they deserved at the time.

This one goes back to my second year at NIT Surat, when a team of five of us — Himanshu, Atul, Naman, Dhruv, and me — decided to build an underwater robot for GUJCOST Robofest 2.0, a state-level robot-making competition backed by Gujarat's Science, Technology and Innovation fund. None of us had built anything underwater before. That was, honestly, part of the appeal.

## Why a cuttlefish?

Most underwater robots people build for competitions like this default to a propeller — it's simple, well understood, and there are a hundred tutorials for it. We wanted to try something else. Cuttlefish don't have propellers. They swim by rippling a long fin that runs along the length of their body, pushing water backward in a continuous wave. It's quiet, efficient, and — frankly — looked like a much more interesting engineering problem than bolting a motor to a propeller.

So that became the plan: a shaft made of small interlocking links, threaded through slotted rods, connected to a flexible fin on each side. Drive the shaft, and the fin ripples. Change the relative rotation between the left and right side, and the robot turns.

## The parts that actually worked

We designed everything in SolidWorks first, ran it through ANSYS for structural analysis, and 3D printed the whole chassis in PLA — partly because it was what we had access to, partly because it's forgiving if your first print doesn't quite fit.

For control, we went with an Arduino Nano reading signals from a FlySky RC transmitter over the iBus protocol. Getting the receiver talking cleanly to the Arduino, then mapping that into usable servo positions, was its own small project:

```cpp
int readChannel(byte channelInput, int minLimit, int maxLimit, int defaultValue) {
  uint16_t ch = ibus.readChannel(channelInput);
  if (ch < 100) return defaultValue;
  return map(ch, 1000, 2000, minLimit, maxLimit);
}
```

Before we ever put a fin in water, we spent a lot of evenings just watching four servos sweep back and forth in sync, tuning delay values until the motion actually looked like a wave instead of four motors doing their own thing.

## The buoyancy problem nobody warns you about

Making something move forward and turn is, it turns out, the easy part. Making it go up and down without capsizing or sinking is where real submarines earn their reputation. We used the same principle they do: balance buoyant force against gravity. Water-filled syringes acted as ballast tanks, and a linear actuator pushed or released water to shift the robot's weight — take on water, sink a little; push it out, rise.

## Waterproofing, the unglamorous part

No one warns you that half of building an underwater robot is just keeping water *out* of things that were never designed to be wet. We ended up filling our servos with light-viscosity mineral oil — a trick borrowed from underwater RC hobbyist forums rather than any textbook — sealing the rest with superglue and O-rings. It's not elegant. It works.

## Looking back

We didn't set out to build something with obvious real-world use, but by the end it was easy to see the shape of where this kind of robot actually matters — cheap underwater exploration, checking ship hulls, sampling water for microplastics, observing marine life without disturbing it much. Small, quiet, undulating robots turn out to be a genuinely useful idea, not just a fun one.

Looking back at this now, years later, from a very different phase of my engineering life — I'm glad I finally wrote it down properly instead of letting it stay a folder full of CAD files and forgotten Arduino sketches.