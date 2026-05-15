# Kinematics of mobile robots

## Kinematics and Coordinate Frames

To control a robot, we must map its internal movements to the real
world.

### Reference Frames

-   **World Frame ($W$):** Fixed to the environment (e.g., a room).

-   **Robot Frame ($R$):** Attached to the moving robot.

The goal of kinematics is to calculate the transformation between $W$
and $R$---mapping the robot's local perception to the global map.

### Motion Constraints (Holonomic vs. Non-Holonomic)

A rigid body on a plane has 3 DOF (translation in $x, y$ and rotation
$\theta$).

-   **Holonomic:** A robot that can control all 3 DOF simultaneously
    (e.g., an omnidirectional robot can move sideways instantly).

-   **Non-Holonomic:** A robot that cannot instantaneously move in
    certain directions. A car is non-holonomic; it has 3 DOF in the
    world but only 2 controllable DOF (throttle and steering),
    preventing it from moving sideways without maneuvering.

### Dead Reckoning

This is the process of estimating the robot's current position based
solely on internal sensors (like wheel encoders) without external
references (like GPS). It is prone to accumulating errors (drift) over
time.

## Differential Drive Math

For a differential drive robot with wheel width $W$, we calculate the
path based on the velocities of the right ($v_R$) and left ($v_L$)
wheels:

-   **Straight Line:** $v_L = v_R$ (Radius $R = \infty$).

-   **Rotation on Spot:** $v_L = -v_R$.

-   **Curved Motion:** The robot moves around a point called the **ICC
    (Instantaneous Center of Curvature)**. The radius of this turn $R$
    is determined by the difference in wheel speeds:
    $$R = \frac{W(v_R + v_L)}{2(v_R - v_L)}$$
