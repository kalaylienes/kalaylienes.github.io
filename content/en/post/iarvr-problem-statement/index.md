+++
title = "IARVR 02 - Problem Statement - TUDa 25/26"
date = "2026-03-14"
type = "post"
draft = false
author = ["kalaylienes"]
description = "Problem statement for a physics-driven VR locomotion and interaction system based on indirect control."
categories = ["IARVR25/26"]
tags = ["iarvr", "arvr", "vr", "locomotion", "interaction", "physics", "debugging"]
series = ["IARVR Final Report"]
stage = "budding"
translationKey = "iarvr-problem-statement"
coffee = 1
weight = 2
+++

## Stability Problems in Horse Locomotion

One of the first challenges appeared after activating the horse animations. In early versions of the implementation, the horse would sometimes run away from underneath the player as soon as the animation system was enabled. This happened because the player rig and the horse model were not properly synchronized.

As a result, the player could suddenly lose the riding position while the horse continued moving forward. This completely broke the illusion of riding and made the locomotion system unusable.

Solving this issue required stabilizing the relationship between the player rig and the horse model so that both remained aligned during movement and animation playback.

{{< vimeo-embed
    id="1161275932"
    title="Horse stability issue prototype video"
    caption="Early instability where the horse animation caused the player rig to detach from the mount point."
>}}

## Jump Detection Issues

The jumping mechanic was implemented using a downward swing of the left controller. The idea was to detect a strong downward movement and interpret it as a jump gesture.

However, in practice this approach produced unintended behavior. Because the jump detection relied on velocity values, sudden spikes in controller movement could trigger jumps even when the player did not intend to jump.

These spikes often occurred when the player interacted with large objects in the environment, such as checkpoint structures or transitions between different surfaces. In some cases the system interpreted these small collisions as a jump gesture.

After investigating the problem, it became clear that collider interactions in the environment were affecting the controller velocity readings. Converting these colliders into trigger volumes helped reduce the problem significantly.

For the demo version of the project, jumping was also restricted to the final straight section of the course to avoid unexpected behavior in earlier sections of the environment.

{{< project-figure
    src="iarvr/problem-statement/iarvr-problem-04-collider-problem.png"
    alt="Scene screenshot showing checkpoint colliders that interfered with jump gesture detection."
    caption="Checkpoint structures originally used physical colliders, which caused unintended velocity spikes in the jump detection system."
>}}

## Physics Challenges in Object Interaction

The object interaction phase introduced a different set of challenges. The player needs to manipulate a T-shaped object using a hook attached to the rod. Instead of grabbing the object directly, the player attaches the hook to the object and lifts it through the rope system.

Early versions of this system suffered from unstable physics behavior. When gravity was disabled, the object often behaved unrealistically and could easily drift away or move unpredictably. When gravity was enabled, the opposite problem occurred: the object could fall too quickly or move outside the intended interaction space.

Balancing these behaviors required several adjustments to the rigidbody configuration and interaction logic. The goal was to maintain a sense of physical behavior while still keeping the object controllable for the interaction task.

{{< project-figure
    src="iarvr/problem-statement/iarvr-problem-05-object-gravity-problem.png"
    alt="Early object interaction test showing unstable behavior caused by gravity and rigidbody settings."
    caption="Early object interaction tests where enabling or disabling gravity produced unstable behavior."
>}}
