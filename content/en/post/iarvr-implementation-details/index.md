+++
title = "IVAR 04 - Implementation Details - TUDa 25/26"
date = "2026-03-15"
type = "post"
draft = false
author = ["kalaylienes"]
description = "Implementation details for the indirect horse locomotion and hook-based interaction systems in the IVAR project."
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

The following code shows the essential part of the mount system, where the player rig is locked to the mount point on the horse.

```csharp
if (riderRig)
{
    riderRig.position = mountPoint.position;
    if (lockRotation && !lockOnlyYaw)
        riderRig.rotation = mountPoint.rotation;
}
```

This approach proved to be much more reliable during gameplay.

{{< project-figure
    src="iarvr/implementation-details/iarvr-implementation-01-mount-point-hierarchy.png"
    alt="Unity hierarchy screenshot showing the horse object, the MountPoint child, and the VR rig alignment."
    caption="The VR rig follows a MountPoint attached to the horse to avoid conflicts with the animation system."
>}}

## Carrot and Rope System

The carrot used to guide the horse is attached to a fishing rod held in the player's right hand. To visually represent the connection between the rod and the carrot, a rope-like element is rendered between two attachment points.

One attachment point is placed at the tip of the rod, while the second one is located on the carrot object. A line renderer is used to visually connect these two points, creating the impression of a rope.

Because the carrot also influences the locomotion system, its movement had to remain readable and somewhat stable. Additional adjustments were therefore introduced to limit excessive swinging and make the system easier to control.

These changes were primarily intended to improve gameplay stability rather than to simulate a physically accurate rope system.

This snippet illustrates how the rope system continuously measures the distance between the rod tip and the carrot attachment point.

```csharp
Vector3 attachPos = GetAttachWorldPos();
Vector3 fromTip = attachPos - rodPosCached;
float dist = fromTip.magnitude;
if (dist < 0.0001f) return;

Vector3 dir = fromTip / dist;

float freeLen = restLength + slackExtra;
float hardMax = freeLen + maxExtraStretch;
```

{{< project-figure
    src="iarvr/implementation-details/iarvr-implementation-02-mounted-position.png"
    alt="Mounted gameplay setup showing the player rig positioned on the horse during traversal."
    caption="The mounted position is stabilized so the rider stays aligned with the horse during movement."
>}}

## Jump Detection Implementation

The jumping mechanic was implemented by detecting a downward swing motion of the left controller. The system calculates the movement velocity of the controller and triggers a jump when the downward velocity exceeds a certain threshold.

The jump gesture was implemented by checking the downward velocity and travel distance of the left hand.

```csharp
Vector3 pos = leftHand.position;
Vector3 vel = (pos - prevPos) / Mathf.Max(Time.deltaTime, 0.0001f);

float downVel = -vel.y;
float downDist = Mathf.Max(0f, prevPos.y - pos.y);

if (Time.time - lastJumpTime >= jumpCooldown)
{
    if (downVel >= minDownVelocity && downDist >= minDownDistance)
    {
        horse.RequestJump();
        lastJumpTime = Time.time;
    }
}
```

While this method worked well in controlled tests, it initially produced unintended jumps when the player interacted with large environmental colliders. Objects such as checkpoints or ground transitions could cause short spikes in controller velocity.

To reduce these unwanted triggers, several checkpoint colliders were converted into trigger volumes so they would no longer influence the physics interactions of the player rig.

For the demo version of the project, the jumping mechanic was also restricted to the final straight section of the parkour course. This ensured that the locomotion system remained stable during the main traversal sections.

To keep the demo stable, jumping was restricted to a dedicated area using a collider-based zone check.

```csharp
bool insideZone = true;
if (requireInsideZone)
{
    if (allowedJumpZone == null) insideZone = false;
    else insideZone = allowedJumpZone.bounds.Contains(horse.transform.position);
}
```

## Hook Interaction System

The object interaction phase introduces a hook system that allows the player to manipulate a T-shaped object indirectly.

Instead of grabbing the object directly with the VR controller, the player approaches the object with the hook attached to the rod. When the hook touches the object and the trigger is pressed, the system creates a connection between the hook and the object using physics components.

The hook attaches to the object by creating a configurable joint at the contact point.

```csharp
joint = targetRb.gameObject.AddComponent<ConfigurableJoint>();
joint.connectedBody = hookRb;

joint.autoConfigureConnectedAnchor = false;
joint.anchor = targetRb.transform.InverseTransformPoint(contactWorld);
joint.connectedAnchor = hookRb.transform.InverseTransformPoint(hookTipTrigger.transform.position);

joint.xMotion = ConfigurableJointMotion.Locked;
joint.yMotion = ConfigurableJointMotion.Locked;
joint.zMotion = ConfigurableJointMotion.Locked;
```

The object can then be lifted and moved through the rope system.

This approach allows the object to react to physics while still remaining controllable during the interaction task.

{{< project-figure
    src="iarvr/implementation-details/iarvr-implementation-03-hook.png"
    alt="Gameplay or Unity screenshot showing the hook attaching to the T-shaped object."
    caption="The hook connects to the object through a physics-based attachment."
>}}

## Object Rotation Through Shooting

To adjust the orientation of the T-shaped object, a shooting mechanic was introduced using the revolver.

When the player fires the revolver, a raycast is emitted from the weapon. If the ray hits the interaction object, the system determines which side of the object was hit. Based on this information, the object rotates by a small predefined angle.

The revolver uses a raycast and a short tracer line to visualize shooting and detect hits.

```csharp
Ray ray = new Ray(muzzle.position, muzzle.forward);

bool hitSomething = Physics.Raycast(ray, out RaycastHit hit, range, hitMask, QueryTriggerInteraction.Collide);

Vector3 endPoint = hitSomething ? hit.point : (muzzle.position + muzzle.forward * range);

EnsureTracer();
DrawTracer(muzzle.position, endPoint);
```

This part handles interaction targets and also forwards hit information to the object-rotation system.

```csharp
if (hitSomething)
{
    var target = hit.collider.GetComponentInParent<ShootTarget>();
    if (target != null)
    {
        target.Shot();
    }

    var rot = hit.collider.GetComponentInParent<ObjectTRotateOnShot>();
    if (rot != null)
        rot.RotateFromHit(hit.point);
}
```

This step-based rotation method provides a simple way for the player to gradually align the object with the target silhouette.

The object determines whether the left or right side was hit and updates its target rotation accordingly.

```csharp
Vector3 localHit = transform.InverseTransformPoint(hitPointWorld);

float dir = (localHit.x >= 0f) ? 1f : -1f;

desiredYaw = Mathf.Repeat(desiredYaw + dir * stepDegrees, 360f);
hasTarget = true;
```

The actual rotation is applied gradually so the object turns in controlled steps instead of spinning continuously.

```csharp
float current = transform.eulerAngles.y;
float next = Mathf.MoveTowardsAngle(current, desiredYaw, maxYawSpeed * Time.deltaTime);

transform.rotation = Quaternion.Euler(0f, next, 0f);

if (Mathf.Abs(Mathf.DeltaAngle(next, desiredYaw)) < 0.1f)
{
    transform.rotation = Quaternion.Euler(0f, desiredYaw, 0f);
    hasTarget = false;
}
```

{{< project-figure
    src="iarvr/implementation-details/iarvr-implementation-04-object-interaction.png"
    alt="Screenshot showing the object interaction area during manipulation and alignment."
    caption="The object interaction phase keeps the physics-driven object controllable inside the task space."
>}}

## Stabilizing the Object After Release

Another challenge appeared when releasing the object from the hook. If the object remained fully controlled by physics, it could immediately fall or rotate unpredictably.

To solve this problem, the object's physics behavior is adjusted once it is released. The rigidbody velocities are reset and gravity can be disabled so that the object remains in the position where it was placed.

When the object is released, its motion is reset and physics is partially disabled so it stays in place.

```csharp
if (freezeOnRelease && latchedRb)
{
    latchedRb.linearVelocity = Vector3.zero;
    latchedRb.angularVelocity = Vector3.zero;

    latchedRb.useGravity = false;

    if (freezeUseKinematic)
        latchedRb.isKinematic = true;
}
```

This allows the player to carefully position the object without it drifting away after being released.
