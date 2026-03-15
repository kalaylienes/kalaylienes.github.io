+++
title = "IVAR 01 - Introduction & Motivation - TUDa 25/26"
date = "2026-03-14"
type = "post"
draft = false
author = ["kalaylienes"]
description = "Introduction and motivation for an indirect horse locomotion and hook interaction project in VR."
categories = ["IARVR25/26"]
tags = ["iarvr", "arvr", "vr", "locomotion", "interaction", "motivation"]
series = ["IARVR Final Report"]
stage = "budding"
translationKey = "iarvr-introduction-motivation"
coffee = 1
weight = 1
+++

## Project Overview

This project was developed as part of the Introduction to AR/VR course. The goal of the assignment was to design and implement a locomotion technique and an interaction technique in a virtual reality environment.

Instead of implementing a conventional locomotion system based on joystick input, the idea was to explore a more indirect interaction style where the player influences movement through another object. The result is a locomotion system inspired by the well-known "carrot on a stick" metaphor.

In this system, the player rides a horse and controls its movement indirectly by positioning a carrot attached to a fishing rod. The horse reacts to the carrot's position and distance, which determines both the direction and speed of movement. This approach allowed the locomotion mechanic to feel more playful and slightly less predictable compared to traditional direct control methods.

{{< project-figure
    src="iarvr/introduction-motivation/iarvr-introduction-01-unity-scene.png"
    alt="Early prototype scene showing the horse, carrot, and rod system."
    caption="Early prototype of the carrot-based locomotion system in Unity."
>}}

The second part of the project focuses on object interaction. Instead of using the standard VR grabbing technique, the interaction system introduces an indirect manipulation method using a hook and a revolver. The player first enters the interaction mode by shooting a target. In this mode the carrot is replaced by a hook, which can be used to lift a T-shaped object. The orientation of the object can then be adjusted by shooting it with the revolver.

Overall, the project combines a physics-based locomotion mechanic with a simple but unconventional object interaction system.

The Unity project repository is available here: [Carrot-on-a-stick_IVAR2526](https://github.com/kalaylienes/Carrot-on-a-stick_IARVR2526).

## Design Motivation

A key motivation behind the locomotion system was to avoid the typical VR movement approach where the player directly controls movement using a joystick. While such systems are practical, they often feel detached from the virtual world.

The carrot on a stick metaphor offered a simple alternative. Instead of directly controlling the horse, the player influences the horse's behavior by guiding the carrot in front of it. The horse then reacts to this stimulus, creating a form of indirect locomotion.

{{< project-figure
    src="iarvr/introduction-motivation/iarvr-introduction-02-start-position.png"
    alt="Start area screenshot showing the player mounted on the horse at the beginning of the course."
    caption="The start area of the project, where the player begins the horse-based locomotion sequence."
>}}

This idea allowed the locomotion mechanic to emerge from the relationship between multiple objects: the horse, the carrot, the rod, and the rope connecting them. The goal was not to create a perfectly realistic simulation but rather a playful interaction that still feels understandable and controllable.

During development, several adjustments were made to stabilize the system and make it usable in practice. Small gameplay additions such as wind channels were also introduced later to make the locomotion experience slightly more challenging.

## Project Overview Slides

The slide deck prepared for the presentation can be reviewed below for a compact project overview.

<div class="project-embed">
  <iframe src="https://gamma.app/embed/guy67s2ni7f50pv" allow="fullscreen" title="IVAR 2025/26"></iframe>
</div>
