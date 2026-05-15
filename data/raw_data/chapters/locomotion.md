# Locomotion

## Fundamentals of Locomotion

Locomotion is the physical interaction between a vehicle and its
environment to move from place to place.

### Nature vs. Technology

Nature never developed the wheel or the fully rotating joint.
Roboticists often use bio-inspiration (mimicking geckos, fish, birds),
but direct imitation is difficult because robots use gears and motors,
not muscles.

### Efficiency of Motion

-   **Rolling:** (hard wheels on hard surfaces, like trains) is the most
    energy-efficient form of motion.

-   **Walking:** Surprisingly efficient because it is mechanically
    similar to rolling (the center of mass moves in a way that mimics a
    rimless wheel).

-   **Inefficient Modes:** Sliding, crawling, and running require
    significantly more power.

## Legged Locomotion and Stability

### Why Legs?

Unlike wheels, legs can overcome obstacles higher than their radius,
step over gaps, and keep the payload (body) level while climbing slopes.

### Stability Types

-   **Static Stability:** The center of gravity stays within the polygon
    of support (requires at least 3 legs, or large feet).

-   **Dynamic Stability:** The robot is constantly "falling" and
    catching itself (like a human walking). This allows for 2-legged
    walking but requires active control.

### Passive Dynamic Walkers

A class of robots that can walk down a slope without any motors or
control, solely powered by gravity and inertia (converting potential to
kinetic energy). This proves that walking can be naturally efficient.

### Humanoid Challenges

-   **Energy Cost:** Robots like Honda's ASIMO are inefficient (high
    Cost of Transport) because they walk with bent knees to maintain
    stability, requiring constant motor torque.

-   **ZMP (Zero Moment Point):** A control algorithm used to ensure
    stability by keeping the moment forces at the foot contact point at
    zero.

-   **State of the Art:** While Boston Dynamics shows impressive
    agility, many humanoid demonstrations are still tele-operated or
    highly rehearsed (open loop), lacking true reliable autonomy for
    real-world deployment.

## Wheeled Locomotion and Kinematics

### Fundamentals of Wheeled Locomotion

While legged locomotion is bio-inspired, wheeled locomotion is the
dominant form for mobile robots due to energy efficiency on hard
surfaces. However, every wheel added to a robot imposes constraints on
its motion.

#### Degrees of Freedom (DOF)

-   **Standard Wheel:** Has two degrees of freedom. It can rotate (spin)
    around its horizontal axis and pivot around the point of contact
    with the ground. However, it generally does not allow sideways
    sliding (lateral motion) due to friction.

-   **Castor Wheel:** Consists of a wheel with a vertical pivoting axis
    offset from the wheel's center. It effectively has three degrees of
    freedom (spinning, pivoting offset, and rotation around the contact
    point), allowing it to align with the direction of motion.

-   **Swedish (Mecanum) Wheel:** Features rollers set at an angle
    (usually $45^{\circ}$) around the main wheel perimeter. It has three
    degrees of freedom because the rollers allow the wheel to move
    sideways (laterally) without sliding friction, enabling
    omnidirectional movement.

-   **Spherical Wheel:** A ball held in a socket. It has three degrees
    of freedom and poses no constraints on the robot's motion, moving
    freely in any direction. However, they are technically difficult to
    implement because dirt easily jams the mechanism.

## Robot Configurations and Maneuverability

Robot mobility is defined by the arrangement of these wheels. A robot's
design is always a trade-off between **maneuverability** (ability to
move freely) and **controllability** (ease of controlling that
movement).

-   **Differential Drive:** The most common indoor robot configuration
    (e.g., Roomba). It uses two independently driven wheels and one or
    more passive caster wheels for stability.

    -   **Motion:** It steers by varying the speed of the two drive
        wheels. If both move at the same speed, it goes straight; if
        they move in opposite directions, it turns on the spot (zero
        turning radius).

-   **Synchro Drive:** Uses three or more wheels that are mechanically
    linked. One motor controls the spin of all wheels, and a second
    motor controls the steering of all wheels simultaneously.

    -   **Limitation:** The robot body cannot change its orientation
        (heading). It can move in any direction ($x, y$), but the robot
        always faces the same way.

-   **Ackermann Steering (Car-like):** Uses two rear drive wheels and
    two front steerable wheels. It has high stability but low
    maneuverability (non-holonomic). It cannot move sideways or turn on
    the spot, requiring complex maneuvers (like parallel parking) to
    reach specific poses.

-   **Omnidirectional Robots:** Use Swedish or spherical wheels to move
    instantaneously in any direction ($x, y$) and rotate ($\theta$)
    simultaneously. While highly maneuverable, they are complex to
    control and suffer from uneven vibration and traction issues.

## Stability and Environment Interaction

-   **Static Stability:** To be statically stable without active
    control, a robot needs at least three contact points, and the center
    of mass must fall within the polygon formed by these points.

-   **Suspension:** With three wheels, contact is guaranteed. With four
    or more wheels, suspension is required to ensure all wheels maintain
    ground contact, especially on uneven terrain.

-   **Passive Adaptation (The Shrimp Robot):** Complex navigation does
    not always require sensors. The "Shrimp" robot uses a mechanical
    design with a bogie system and spring-loaded wheels to passively
    climb obstacles up to two times its wheel radius without needing to
    "perceive" the obstacle or calculate a path.
