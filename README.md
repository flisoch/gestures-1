
# Table of Contents

1.  [Motivation](#orgf77ed45)
2.  [Features](#org91966b3)
3.  [Dependencies](#orged805d9)
4.  [Alternatives](#orgf5416b5)
5.  [TODOS](#org282071f)
    1.  [add customization <code>[0/4]</code>](#orgcd20eb6)
    2.  [enrich readme](#orgc8c836b)
    3.  [Write script to fulfill dependencies automatically](#orgd669072)
    4.  [Implement C++ version](#org5dc67e9)

![img](gestures.gif "demonstrating fluid gestures, five finger gestures, tap gestures and touchscreen gestures")


<a id="orgf77ed45"></a>

# Motivation

-   I wanted an application that will allow me to fluidly express my intent through the touchpad and touchscreen.
-   I felt shackled by one-shot gestures from [libinput-gestures](https://github.com/bulletmark/libinput-gestures) but from it I found the utility of eight-directional gestures.
-   I felt [fusuma](https://github.com/iberianpig/fusuma) was lacking Although it was more fluid.
-   I really missed keeping my fingers moving across the touchpad to switch applications.
-   I wanted tap gestures.
-   I wanted five finger gestures.
-   I wanted usable gestures for my touchscreen.

So I wrote this.


<a id="org91966b3"></a>

# Features

-   **vanilla gestures:** features present in [libinput-gestures](https://github.com/bulletmark/libinput-gestures) and [fusuma](https://github.com/iberianpig/fusuma).
-   **5 finger gestures:** 

This means that you can place 5 fingers on the touchpad and that is recognized as another class of gestures. BTN<sub>TOOL</sub><sub>QUINTTAP</sub> must supported. This feature must supported by your driver + touchpad.
check by running `evtest /dev/input/$(cat /proc/bus/input/devices | grep -iA 5 'touchpad' |grep -oP 'event[0-9]+') | grep BTN_TOOL_QUINTTAP`. if it prints a line containing `"BTN_TOOL_QUINTTAP"`, your touchpad+driver support it.

-   **tap gestures:** 

This means that tapping the touchpad with 4 or 5 fingers is recognized as a gesture.

-   **Touchscreen gestures:** 

Extend gestures to touchscreen.

-   **Fluid gestures:** 

Allow the user to do different things without raising their fingers from the touchpad. This takes advantage of the fact that gesture execution is separated into 3 parts, start, update, and end, by doing complementary actions in each.

For example, assume the shortcut to switch windows is "CTRL + ALT" + direction, where direction is "LEFT", "RIGHT", "UP", "DOWN".

to switch windows fluidly, "CTRL + ALT" are held down programmatically on a start gesture event, then depending on which direction the user is swiping while fingers are still on the touchpad, dynamically generate direction commands. Then when the user raises their fingers, which is an end gesture event, the "CTRL + ALT" are released programmatically.


<a id="orged805d9"></a>

# Dependencies

-   **python dependencies:** subprocess, pathlib, shlex, threading, queue, time, os, sys, math
-   **\*nix dependencies:** cat, grep
-   **dependencies:** stdbuf, evtest
    -   **evtest:** will maybe replaced by evemu-record in the future.
    -   **daemonize:** using `& disown` should work as well but this is a sure way to detach and run this on a global scale.
-   **default dependencies (if running default configuration):** -   **evemu:** need evemu-do (alternative to xdotool that I wrote) in $PATH.


<a id="orgf5416b5"></a>

# Alternatives

-   **[libinput-gestures](https://github.com/bulletmark/libinput-gestures):** -   what I used to use.
    -   Works well, just that the gestures are one-shot, meaning that the command attached to a gesture is executed only once per full swipe.
    -   depends on libinput.
-   **[fusuma](https://github.com/iberianpig/fusuma):** -   Although it doesn't have one-shot limitation, it didn't support commands to run when the gesture begins and ends. This is useful for use-cases like switching applications which require alt-down to be pressed.
    -   didn't support eight-directional gestures.


<a id="org282071f"></a>

# TODOS



<a id="orgcd20eb6"></a>

## TODO add customization <code>[0/4]</code>

-   [ ] enable customization by refactoring code.
-   [ ] use [libinput-gestures ](https://github.com/bulletmark/libinput-gestures)config file syntax.
-   [ ] use [fusuma](https://github.com/iberianpig/fusuma) config file syntax.


<a id="orgc8c836b"></a>

## TODO enrich readme


<a id="orgd669072"></a>

## TODO Write script to fulfill dependencies automatically


<a id="org5dc67e9"></a>

## TODO Implement C++ version

