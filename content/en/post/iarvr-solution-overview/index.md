+++
title = "IARVR 03 - Solution Overview - TUDa 25/26"
date = "2026-03-15"
type = "post"
draft = false
author = ["kalaylienes"]
description = "Solution overview for the indirect horse locomotion and interaction systems in the IARVR project."
categories = ["IARVR25/26"]
tags = ["iarvr", "arvr", "vr", "locomotion", "interaction", "solution", "wind"]
series = ["IARVR Final Report"]
stage = "budding"
translationKey = "iarvr-solution-overview"
coffee = 1
weight = 3
+++

## Horse Locomotion Concept

The locomotion system is based on an indirect control mechanism inspired by the carrot on a stick metaphor. Instead of directly controlling the horse through joystick input, the player influences the horse's movement by positioning a carrot attached to a fishing rod.

The carrot is held in the player's right hand and connected to the rod with a rope-like visual element. The horse continuously reacts to the carrot's position and distance. When the carrot is moved further away from the horse, the horse begins to move forward. Steering is achieved by positioning the carrot to the left or right of the horse's forward direction.

This approach creates a locomotion system where movement emerges from the relationship between multiple objects rather than from direct player input.

{{< project-figure
    src="iarvr/solution-overview/iarvr-solution-01-carrot-rod-rope.png"
    alt="Screenshot showing the carrot, rod, and rope setup used for indirect horse locomotion."
    caption="Locomotion is controlled indirectly by positioning a carrot attached to the rod."
>}}

## Rope and Carrot Stabilization

During early tests the carrot behaved too unpredictably. Because the object was attached to the rod and influenced by physics, it often swung excessively and made the locomotion system difficult to control.

To improve stability, additional adjustments were introduced to limit the carrot's movement and make its behavior easier to predict. These adjustments were not meant to simulate a physically accurate rope system but rather to create a stable and readable interaction.

Once the carrot movement became more stable, the horse locomotion system also became significantly easier to control.

## Wind Channels for Additional Challenge

After the locomotion system became functional, feedback from Prof. Dr. Jan Gugenheimer suggested that the parkour section could benefit from additional difficulty. While the locomotion mechanic worked reliably, the environment itself did not challenge the player enough.

To address this, wind channels were added to specific sections of the course. These zones apply directional forces that influence the carrot's position and movement. Because the horse reacts to the carrot, these disturbances indirectly affect the horse's speed and steering.

The goal of the wind channels was not to fundamentally change the locomotion mechanic but to introduce moments where the player must react and adjust the carrot's position more carefully.

{{< project-figure
    src="iarvr/solution-overview/iarvr-solution-02-wind-channel.png"
    alt="Screenshot of a wind channel affecting the carrot and making horse locomotion harder to control."
    caption="Wind channels introduce disturbances that slightly alter the carrot's position and make the locomotion more challenging."
>}}

## Interaction Mode

The second part of the project focuses on object interaction. Instead of allowing the player to directly grab objects, the interaction system introduces a separate interaction mode.

The player enters this mode by equipping a revolver using the left controller and shooting a Start target. Once the interaction mode is activated, the carrot is visually replaced by a hook while the rope system remains active.

Using the hook, the player can attach to a T-shaped object located in the interaction area. The object can then be lifted and repositioned by moving the controller.

{{< project-figure
    src="iarvr/solution-overview/iarvr-solution-03-hook-interaction.png"
    alt="Interaction mode screenshot showing the hook attached to the T-shaped object."
    caption="In interaction mode, the carrot is replaced by a hook that allows indirect manipulation of the T-shaped object."
>}}

To adjust the orientation of the object, the revolver can be used again. Shooting one side of the object rotates it step by step, allowing the player to align it with the target silhouette.

Once the object has been placed correctly, the player can shoot the Start target again, which now functions as a Done button. This exits the interaction mode and restores the locomotion setup.

{{< project-figure
    src="iarvr/solution-overview/iarvr-solution-04-object-rotation.png"
    alt="Screenshot showing object rotation by shooting one side of the T-shaped object with the revolver."
    caption="The orientation of the object can be adjusted by shooting it with the revolver."
>}}
