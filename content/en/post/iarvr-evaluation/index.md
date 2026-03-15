+++
title = "IARVR 05 - Evaluation - TUDa 25/26"
date = "2026-03-15"
type = "post"
draft = false
author = ["kalaylienes"]
description = "Evaluation of the indirect horse locomotion and hook-based interaction systems in the IARVR project."
categories = ["IARVR25/26"]
tags = ["iarvr", "arvr", "vr", "evaluation", "playtesting", "locomotion", "interaction"]
series = ["IARVR Final Report"]
stage = "budding"
translationKey = "iarvr-evaluation"
coffee = 1
weight = 5
+++

## Evaluation Approach

The implemented locomotion and interaction systems were tested primarily through repeated gameplay sessions during development as well as short playtests with other people. The goal of the evaluation was not to conduct a formal user study, but rather to observe whether the mechanics were understandable, controllable, and enjoyable to use.

## Locomotion Feedback

During testing, the locomotion system proved to be intuitive after a short adaptation period. Most players quickly understood that the horse reacts to the carrot's position and distance. Steering the horse by moving the carrot left or right felt natural once the relationship between the carrot and the horse movement became clear.

Several testers mentioned that the overall experience felt playful and entertaining. In particular, the added sound effects for the horse movement, revolver shots, and interaction events were perceived as very enjoyable and helped make the experience feel more responsive and engaging. These sounds were implemented using freely available sound samples.

{{< project-figure
    src="iarvr/evaluation/iarvr-evaluation-01-parkour-gameplay.png"
    alt="Gameplay screenshot showing the player moving through the parkour section using carrot-based horse locomotion."
    caption="Gameplay during the parkour section using the carrot-based locomotion system."
>}}

## Motion Discomfort and Challenge

The wind channels introduced an additional challenge to the locomotion system. These zones temporarily disturb the carrot's position and force the player to react more actively. While this mechanic successfully made the gameplay more challenging, several testers reported that entering a wind channel unexpectedly caused a short moment of discomfort or slight motion sickness. This reaction was not entirely surprising, as sudden changes in movement direction are known to be challenging in VR environments.

A similar effect was occasionally reported during jump sequences, where the sudden vertical movement could also trigger mild motion discomfort for some players.

## Interaction Feedback

The interaction task was generally perceived as fun but also somewhat challenging. Players needed a few attempts to correctly lift and align the T-shaped object using the hook system and the revolver rotation mechanic. However, once the interaction logic became clear, most players were able to complete the task successfully.

{{< project-figure
    src="iarvr/evaluation/iarvr-evaluation-02-interaction-task.png"
    alt="Gameplay screenshot showing the player placing the T-shaped object into the matching silhouette."
    caption="The interaction task where the player places the T-shaped object into the matching silhouette."
>}}

## Summary

Overall, the feedback received during testing was largely positive. The locomotion system was considered enjoyable and understandable, while the interaction puzzle added an additional layer of engagement. Some elements, such as the wind channels and jump moments, introduced small motion discomfort for some players, but they also contributed to the challenge of the experience.
