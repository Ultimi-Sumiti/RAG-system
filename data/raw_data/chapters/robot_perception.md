# Robot Perception

## Robot Perception and Sensors

### The Challenge of Perception

#### The Paradox of Intelligence

In robotics, "easy problems are hard and hard problems are easy".
Abstract tasks like playing chess (closed world, complete information)
are computationally easy, whereas perception tasks that humans perform
unconsciously (like seeing and walking) are extremely difficult for
robots.

#### Open vs. Closed Worlds

Perception is difficult because the real world is an "open world" with
dynamic events, occlusions, and incomplete information, unlike the
static, fully defined "closed world" of a chessboard.

#### Current Limitations

While technology has advanced in "seeing" (detecting objects via Deep
Learning), robots still struggle with "feeling" or understanding the
context and semantics of a situation (e.g., recognizing a dangerous
traffic situation versus a safe one).

## The Perception Pipeline: From Data to Semantics

The process of robot perception involves compressing raw data into
abstract concepts:

-   **Raw Data:** The lowest level consists of numbers (vectors,
    matrices) coming directly from sensors, such as pixel arrays or
    range readings.

-   **Features:** Algorithms extract patterns from raw data, such as
    lines, corners, or colors. This often requires imposing a model
    (e.g., fitting a line to a set of points).

-   **Objects:** By combining features, the robot identifies entities
    (e.g., recognizing a door or a human).

-   **Semantics and Context:** The highest level involves reasoning
    about the relationships between objects (e.g., a door connects two
    rooms) and understanding the situation or place.

-   **Sensor Fusion:** This refers to combining data from multiple
    sensors (often multimodal, like cameras and lasers) to create a more
    reliable "percept" or world model.

## Sensor Taxonomy

Sensors are classified based on what they measure and how they interact
with the environment:

### Proprioceptive vs. Exteroceptive

-   **Proprioceptive:** Measures the internal state of the robot (e.g.,
    battery level, wheel encoders, joint angles, internal temperature).

-   **Exteroceptive:** Measures the external environment (e.g., cameras,
    laser scanners, distance sensors).

-   **Exproprioceptive:** Measures the relationship between the robot's
    body and the environment (e.g., a camera on an arm guiding a gripper
    toward an object).

### Active vs. Passive

-   **Active:** The sensor emits energy into the environment and
    measures the reaction (e.g., sonar, laser range finders, active
    optical beacons).

-   **Passive:** The sensor receives energy already present in the
    environment (e.g., cameras, microphones, tactile sensors).

**Note on GPS:** While the GPS system (satellites) is active, the
receiver on the robot is typically considered a passive sensor because
it only receives signals.

## Characterizing Sensor Performance

Real-world measurements are always error-prone and uncertain.

### Key Metrics

-   **Dynamic Range:** The ratio between the minimum and maximum
    measurable values, usually expressed in decibels.

-   **Resolution:** The minimum difference between two values that the
    sensor can detect.

-   **Bandwidth:** The speed or frequency at which the sensor can
    provide updated readings.

-   **Sensitivity:** The ratio of output change to input change. Sensors
    can suffer from cross-sensitivity, where they react to unintended
    environmental changes (e.g., a light sensor affected by
    temperature).

### Accuracy vs. Repeatability

-   **Accuracy:** How close a measurement is to the true value
    (reference standard).

-   **Repeatability (Precision):** The ability to reproduce the same
    result under the same conditions multiple times.

**The Target Analogy:** High accuracy means hitting the center of the
target; high repeatability means all shots land close together, even if
they are far from the center.

### Industrial Application

Industrial robots typically have high repeatability but low accuracy.
They are programmed by "teaching" (saving a specific posture) rather
than by geometric coordinates, ensuring they return to the exact same
spot even if the coordinate calculation is imperfect.

## Motion and Heading Sensors (Dead Reckoning)

Dead reckoning is the process of estimating position by integrating
internal motion measurements over time, though it is prone to
accumulating drift.

-   **Wheel Encoders:** Proprioceptive sensors attached to motor axes.
    They use optical gratings to count transitions and calculate wheel
    rotation speed and distance. Quadrature encoders can also detect
    direction.

-   **Compass:** Uses the earth's magnetic field to determine absolute
    heading. It is often unreliable indoors due to interference from
    power lines and metallic structures.

-   **Gyroscopes:** Measure angular velocity to estimate heading
    changes.

    -   **Mechanical:** Rely on a spinning mass maintaining its axis of
        rotation (momentum). They suffer from drift over time.

    -   **Optical:** Use the Sagnac effect with laser beams traveling in
        opposite directions. They are more precise than mechanical ones
        but still drift.

-   **Accelerometers:** Measure proper acceleration using a spring-mass
    system or MEMS technology (capacitive changes). They are used to
    detect tilt (gravity) or dynamic motion.

-   **Inertial Measurement Unit (IMU):** A closed system combining
    accelerometers, gyroscopes, and magnetometers to estimate
    orientation and motion. Data is often fused using a Kalman Filter.

## Tactile and Contact Sensors

-   **Bumpers:** Simple, passive switches that detect physical contact.
    They are cheap but provide poor localization of the object.

-   **Whiskers:** Bio-inspired sensors that can provide information
    about proximity and object texture.

-   **Robot Skin (RoboSkin):** Advanced capacitive sensors organized in
    triangular modules that cover the robot's body. These allow
    collaborative robots ("cobots") to detect exactly where and how hard
    they are touched, ensuring safety during human-robot interaction.

## Technology Drivers

New sensor technologies often drive revolutions in robotics
capabilities:

-   **Kinect:** Introduced cheap RGB-D (color + depth) sensing, solving
    the difficulty of measuring the third dimension.

-   **Laser Time-of-Flight:** Previously expensive ($\approx$ 50,000
    euros), these sensors are now affordable enough for household
    vacuums, providing reliable distance measurements compared to noisy
    sonar.

-   **Soft Actuation:** Emerging technologies in soft grippers and
    artificial muscles are opening new applications.

## Advanced Proprioception and Exteroceptive Sensing

### Inertial Measurement and Dead Reckoning

-   **The IMU (Inertial Measurement Unit):** This single sensor combines
    three gyroscopes, three magnetometers, and three accelerometers to
    estimate the robot's motion in 6 degrees of freedom (3 rotational
    and 3 positional axes).

-   **Dead Reckoning:** This is the process of estimating position
    solely by integrating internal measurements (speed, heading,
    acceleration) without external references, similar to navigating a
    ship at sea with only a compass.

-   **Drift and Uncertainty:** Dead reckoning is prone to accumulated
    errors over time (drift). For example, a car navigation system in a
    tunnel uses dead reckoning when GPS is lost; it estimates position
    based on speed and heading, but errors grow until the car exits the
    tunnel and receives a corrective external signal.

-   **Sensor Fusion:** To combat noise and uncertainty, robots use
    sensor fusion. This involves combining redundant data (to correct
    noise) or complementary data (different distinct features) to avoid
    "false positives" (detecting non-existent obstacles) or "false
    negatives" (missing real obstacles).

### Contact and Tactile Sensors

These are passive sensors that detect interaction only upon physical
touch, covering the closest range of perception.

-   **Bumpers and Whiskers:**

    -   **Whiskers:** Simple switches triggered by moving a wire.

    -   **Bumpers:** Used in vacuum robots, these typically have two
        switches. Triggering both indicates a frontal collision, while
        triggering one indicates a side collision, allowing the robot to
        navigate corners.

-   **Tactile Sensors:** Based on capacitors embedded in a soft
    substrate. When pressed, the substrate deforms, changing the
    capacitance to detect touch.

-   **Robot Skin (Artificial Skin):** To cover large areas (like a
    humanoid or industrial arm), sensors are organized into a
    tessellation of triangular modules (e.g., Sai skin). Each module
    processes data locally and sends it via a bus. Advanced skins can
    integrate temperature, proximity, and acceleration sensors to mimic
    human sensitivity.

-   **Safety in Collaborative Robots:** Tactile and proximity sensors
    are critical for "cobots." Instead of relying on measuring motor
    current (torque) to detect a collision---which requires a forceful
    impact---skin sensors allow the robot to detect contact immediately
    or even pre-collision, enabling faster but safe operation.

### Proximity Sensors (Infrared)

-   **Principle:** These active sensors emit near-infrared light (LED)
    and measure the reflection to detect objects within a short range
    ($1\text{--}20$ cm).

    \[Image of infrared proximity sensor diagram\]

-   **Limitations:**

    -   **Absorption:** Dark or black objects absorb infrared light,
        yielding no reflection.

    -   **Cross-talk:** They can be blinded by sunlight (which contains
        IR) or interference from other sensors.

    -   **Specular Reflection:** On mirror-like surfaces, light reflects
        away from the sensor rather than bouncing back. This creates a
        "blind spot" where the robot thinks the path is clear,
        potentially leading to collisions.

-   **Spectrum Distinction:** It is important to distinguish Near
    Infrared (used for proximity and night vision) from Thermal Infrared
    (Far Infrared), which is used to detect heat/temperature.

### Triangulation and Structured Light

-   **Triangulation:** Distance is measured by projecting a collimated
    beam (laser point or line) and observing where it falls on a camera
    sensor. The position of the dot on the image plane correlates
    geometrically to the distance of the object.

-   **Structured Light:** Instead of a single point, a complex pattern
    (lines or grids) is projected. By analyzing the deformation of this
    pattern on surfaces, the robot can reconstruct the 3D shape of the
    environment.

-   **3D Cameras:** Devices like the Microsoft Kinect (v1) use an
    infrared projector and camera to create a dense depth map (RGB-D) of
    the scene using this principle.

### Time of Flight (ToF) Sensors

These sensors measure distance by calculating the time ($t$) it takes
for a signal to travel to an object and return.
$$d = \frac{c \cdot t}{2}$$

-   **Speed of Propagation:**

    -   **Sound (Sonar):** Travels at $\approx 300$ m/s ($0.3$ m/ms).
        Easy to measure.

    -   **Light (Lidar/ToF Cameras):** Travels at $\approx 300,000$ km/s
        ($0.3$ m/ns). Requires nanosecond-precision timing.

-   **Ultrasonic Sensors (Sonar):** Cheap and effective for short ranges
    (e.g., car parking sensors).

    -   **Problems:** They have a wide emission cone
        ($\sim 30^{\circ}$), leading to poor angular resolution. They
        also suffer from multipath errors (sound bouncing off multiple
        walls before returning) and specular reflections (sound bouncing
        off smooth walls away from the sensor).

-   **GPS:** Operates on the ToF principle using radio signals from
    satellites. It requires extremely precise time synchronization
    (atomic clocks) but fails indoors or in tunnels (multipath
    problems).
