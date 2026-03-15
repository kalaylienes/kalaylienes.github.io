+++
title = "IARVR 04 - Implementation Details - TUDa 25/26"
date = "2026-03-15"
type = "post"
draft = false
author = ["kalaylienes"]
description = "Implementation details for the indirect horse locomotion and hook-based interaction systems in the IARVR project."
categories = ["IARVR25/26"]
tags = ["iarvr", "arvr", "vr", "implementation", "locomotion", "interaction", "unity", "physics"]
series = ["IARVR Final Report"]
stage = "budding"
translationKey = "iarvr-implementation-details"
coffee = 1
weight = 4
+++

## Mounting the Player on the Horse

One of the first technical challenges was attaching the VR player rig to the horse model in a stable way. The initial idea was to directly parent the VR rig to the horse object so that the player would move together with the horse.

However, the horse asset was part of a prefab hierarchy, and modifying the structure directly caused repeated issues in Unity. In several cases the rig position behaved unpredictably once animations were enabled.

To avoid modifying the prefab structure, a simpler workaround was implemented. A new child object called `MountPoint` was added to the horse model. The VR rig then followed this point instead of being directly attached to the animated object. This provided a stable reference position for the player and avoided conflicts with the horse animation system.

This approach proved to be much more reliable during gameplay.

{{< project-figure
    src="iarvr/implementation-details/iarvr-implementation-01-horse-mountpoint-hierarchy.png"
    alt="Unity hierarchy screenshot showing the horse object, the MountPoint child, and the VR rig alignment."
    caption="The VR rig follows a MountPoint attached to the horse to avoid conflicts with the animation system."
>}}

## Carrot and Rope System

The carrot used to guide the horse is attached to a fishing rod held in the player's right hand. To visually represent the connection between the rod and the carrot, a rope-like element is rendered between two attachment points.

One attachment point is placed at the tip of the rod, while the second one is located on the carrot object. A line renderer is used to visually connect these two points, creating the impression of a rope.

Because the carrot also influences the locomotion system, its movement had to remain readable and somewhat stable. Additional adjustments were therefore introduced to limit excessive swinging and make the system easier to control.

These changes were primarily intended to improve gameplay stability rather than to simulate a physically accurate rope system.

{{< project-figure
    src="iarvr/implementation-details/iarvr-implementation-02-rod-tip-carrot-rope.png"
    alt="Unity screenshot showing the rod tip, carrot attachment, and line renderer rope between them."
    caption="A line renderer visually connects the rod and the carrot."
>}}

## Jump Detection Implementation

The jumping mechanic was implemented by detecting a downward swing motion of the left controller. The system calculates the movement velocity of the controller and triggers a jump when the downward velocity exceeds a certain threshold.

While this method worked well in controlled tests, it initially produced unintended jumps when the player interacted with large environmental colliders. Objects such as checkpoints or ground transitions could cause short spikes in controller velocity.

To reduce these unwanted triggers, several checkpoint colliders were converted into trigger volumes so they would no longer influence the physics interactions of the player rig.

For the demo version of the project, the jumping mechanic was also restricted to the final straight section of the parkour course. This ensured that the locomotion system remained stable during the main traversal sections.

## Hook Interaction System

The object interaction phase introduces a hook system that allows the player to manipulate a T-shaped object indirectly.

Instead of grabbing the object directly with the VR controller, the player approaches the object with the hook attached to the rod. When the hook touches the object and the trigger is pressed, the system creates a connection between the hook and the object using physics components.

The object can then be lifted and moved through the rope system.

This approach allows the object to react to physics while still remaining controllable during the interaction task.

{{< project-figure
    src="iarvr/implementation-details/iarvr-implementation-03-hook-attaching-object.png"
    alt="Gameplay or Unity screenshot showing the hook attaching to the T-shaped object."
    caption="The hook connects to the object through a physics-based attachment."
>}}

## Object Rotation Through Shooting

To adjust the orientation of the T-shaped object, a shooting mechanic was introduced using the revolver.

When the player fires the revolver, a raycast is emitted from the weapon. If the ray hits the interaction object, the system determines which side of the object was hit. Based on this information, the object rotates by a small predefined angle.

This step-based rotation method provides a simple way for the player to gradually align the object with the target silhouette.

{{< project-figure
    src="iarvr/implementation-details/iarvr-implementation-04-revolver-shooting-object.png"
    alt="Screenshot showing the revolver aiming at the interaction object to rotate it step by step."
    caption="The revolver uses raycasting to rotate the interaction object step by step."
>}}

## Stabilizing the Object After Release

Another challenge appeared when releasing the object from the hook. If the object remained fully controlled by physics, it could immediately fall or rotate unpredictably.

To solve this problem, the object's physics behavior is adjusted once it is released. The rigidbody velocities are reset and gravity can be disabled so that the object remains in the position where it was placed.

This allows the player to carefully position the object without it drifting away after being released.
