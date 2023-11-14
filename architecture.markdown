---
layout: page
title: Android Architecture
permalink: /architecture/
---

# Android Architecture

The Android system is essentially a Linux computer with a Java API (Application Programming Interface) layer on top. Think about it, you are walking around with a tiny computer in your pocket.

That being said, the major components of the Android platform are,

* Linux Kernel
* HAL
* Native C/C++ Libraries
* Android Runtime
* Android Framework
* Apps

At the core of the Android system is a modified **Linux Kernel**. It runs and manages core systems such as the mic, the camera, the radio module etc.

On top of the Linux Kernel is the **HAL** or **Hardware Abstraction Layer**. The HAL provides consistency for the upper layers. Each handset or device might have subtle differences in how it handles the core systems provided by the Linux Kernel. The HAL hides these differences and exposes a uniform interface to the Native C/C++ Libraries and the Android Runtime above it.

The **Native C/C++ Libraries** as the name suggests provides libraries that help in using the systems provided by the HAL. These libraries a written in the C/C++ programming language. Your apps can access the libraries directly, bypassing the Android runtime if need be. In fact, most high powered mobile games access the native libraries directly to offer high quality game graphics.

The **Android Runtime** also sits on top of the HAL and has access to the systems provided by the HAL. Android apps usually run in the Android Runtime.

We then have the **Android Java API Framework**. It provides Java 'hooks' to the native libraries and Android Runtime. It provides the 'building blocks' that you will use to create your apps.

![Android Stack](../images/android_stack.png)