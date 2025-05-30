---
layout: post
title:  "Swerve Drive Reverse Kinematics"
date:   2023-9-30 20:59:54 -0800
categories: Robotics Engineering Math
seo_title:  "Swerve Drive Reverse Kinematics"
excerpt: "Explanations for the math used for robotics swerve drive."
---

In 2021, my robotics team adopted an independent wheel steering or
"swerve" drivetrain. That year, I was a freshman tasked with programming
the swerve drive. Many of the available libraries and guides were either
completely incorrect or felt too complicated (especially for a novice).

Since then, I have written my own library,
[Quail](https://github.com/mineinjava/quail).

https://github.com/mineinjava/quail

I finally got around to writing some explanations for the math used in
Quail. Here is the start: the basic math needed for movement.

See [this Desmos graph](https://www.desmos.com/geometry/ys8clq7yev) for
an interactive visualization

Translating a swerve robot is trivial: point all the wheels in one
direction and drive.

![All modules going one direction]({{ site.baseurl }}/images/DesmosTranslation.png)

Rotation is also trivial. Point all the wheels in a direction
perpendicular to the robot center (or the center of rotation), and
drive.

![All modules going different directions]({{ site.baseurl }}/images/DesmosRotation.png)

Moving while rotating is a bit more complicated.

From a desired robot movement (velocity), we can calculate the module 
state (position and rotation) for each module.

To do so, we begin with the translation component of the desired robot
movement, vector _R<sub>G</sub>_ (movement of robot relative to the
ground). We rotate the vector for field-oriented control.

```java
    moveVector = moveVector.rotate(gyroOffset, false);
```

For translation, each module state, _m<sub>i</sub>_, is equal to the
robot's movement relative to the ground (_m<sub>i<sub>G</sub></sub>_)
plus the module's movement relative to the robot of rotation
(_m<sub>i<sub>R</sub></sub>_).

_m<sub>i</sub>_ = _m<sub>i<sub>G</sub></sub>_ + _m<sub>i<sub>R</sub></sub>_

It should then follow that _m<sub>i<sub>R</sub></sub>_ can be calculated
easily:

- its angle must be perpendicular to the module's position vector
- its magnitude is equal to the robot's rotation speed times the
  module's distance from the center of rotation.

```java
      Vec2d moduleOffCenterVector = module.position.subtract(centerPoint);
      Vec2d moduleRotationVector = moduleOffCenterVector.rotate(Math.PI / 2, false);
      moduleRotationVector.scale(rotationSpeed);
```

Here's what that looks like:

![Movement and rotation]({{ site.baseurl }}/images/DesmosCombined.png)

And here's the one-liner to calculate them with
[Quail](https://github.com/mineinjava/quail):

```java
SwerveDrive.calculateMoveAngles(moveVector, rotationSpeed, gyroOffset);
```

I have continued to work on the code in collaboration with a friend in
Arizona. I estimate that about two dozen teams used Quail in the 2024
season.
