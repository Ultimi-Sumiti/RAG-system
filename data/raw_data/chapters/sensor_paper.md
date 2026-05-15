# Sensor Technologies in Mobile Robotics (Paper 1)

## Definition and Classification

-   **Sensor vs. Transducer:** A transducer is a device that converts
    one form of energy into another (e.g., mechanical to electrical). A
    sensor is a receiver transducer that measures a physical parameter
    (like light or sound) and converts it into a signal. An actuator is
    an emitter transducer that injects energy into the environment.

-   **Excitation Source:**

    -   **Passive Sensors:** Do not require an external power supply to
        generate an excitation signal; they only receive energy already
        present (e.g., a camera receiving light, a microphone).

    -   **Active Sensors:** Require power to generate their own
        excitation signal and measure the reaction (e.g., Sonar or LiDAR
        emitting pulses).

-   **Measurement Domain:**

    -   **Proprioceptive:** Measure internal robot states (e.g., battery
        level, wheel speed via encoders).

    -   **Exteroceptive:** Measure the environment and the robot's
        interaction with it (e.g., distance to an obstacle).

-   **Key Performance Metrics:**

    -   **Resolution:** The smallest detectable change in the input
        signal.

    -   **Precision (Repeatability):** The ability to reproduce the same
        result for a steady signal (variance).

    -   **Accuracy:** How close the measurement is to the true value
        (correctness).

    -   **Dynamic Range:** The ratio between the largest and smallest
        measurable values, usually expressed in decibels.

## Basic Components (Transducers)

-   **Force and Deformation:**

    -   **Piezoelectric:** Generates voltage when deformed (reversible).
        Used in accelerometers and sonars.

    -   **Capacitive:** Measures force via changes in capacitance
        between plates. Used in MEMS accelerometers and microphones.

-   **Light:**

    -   **CCD vs. CMOS:** Both are imaging sensors. CCDs shift charges
        to a central amplifier (sequential), while CMOS pixels have
        individual amplifiers (faster, common in consumer electronics).

-   **Magnetic:**

    -   **Hall Effect:** A conductor generates a voltage when traversed
        by a current in a magnetic field. Used in encoders and
        compasses.

## Proprioceptive and Contact Sensors

-   **Bumpers:** Simple contact switches used for safety and collision
    detection.

-   **Encoders:** Measure angular position/velocity of wheels for
    odometry.

    -   **Incremental:** Counts "ticks" (relative motion). Quadrature
        encoders use two signals shifted by $90^{\circ}$ to detect
        rotation direction and increase resolution.

    -   **Absolute:** Provides a unique binary code for every specific
        angle, retaining position even after power loss.

## Global Localization Systems

-   **GNSS (Outdoor):**

    -   **GPS:** Uses trilateration of satellite signals. Standard
        accuracy is $5\text{--}15$m.

    -   **RTK (Real-Time Kinematic):** Improves accuracy to
        centimeter-level by using a base station and measuring carrier
        wave phases.

-   **UWB (Indoor):**

    -   **Ultra-Wideband:** Uses short energy pulses for indoor
        positioning. It uses TDOA (Time Difference of Arrival) or TOF
        (Time of Flight) between a mobile "tag" and fixed "anchors". It
        is more accurate than WiFi/Bluetooth but requires
        infrastructure.

## Inertial and Heading Sensors (IMU)

-   **Accelerometers:** Measure linear acceleration. MEMS versions often
    use capacitive plates on a moving frame.

-   **Gyroscopes:** Measure rotational rate.

    -   **MEMS (Vibrating Structure):** Exploit the Coriolis effect on a
        vibrating mass.

    -   **FOG (Fiber-Optic):** Use the Sagnac effect (light
        interference) for high accuracy but are expensive.

-   **Compasses:**

    -   **Fluxgate:** High accuracy devices using magnetic saturation
        coils.

    -   **Hall Effect:** Common, cheaper implementation using
        perpendicular magnetometers.

## Vision Systems (Cameras)

-   **Digital Cameras:**

    -   **Pinhole/Thin Lens Model:** Describes how 3D points project
        onto a 2D sensor plane. A lens is required to focus light rays.

    -   **Bayer Filter:** A grid of R, G, and B filters over pixels
        allows a single sensor to capture color images via
        interpolation.

-   **Omnidirectional Cameras:** Capture
    $180^{\circ}\text{--}360^{\circ}$ views.

    -   **Catadioptric:** Uses a camera pointing at a curved mirror
        (parabolic/hyperbolic).

    -   **Polydioptric:** Uses an array of multiple overlapping cameras.

-   **Event Cameras:** Asynchronous sensors that only output data
    (events) when pixel brightness changes. They have very high dynamic
    range and low latency, making them ideal for high-speed motion.

## Ranging and Depth Sensors

-   **Sonar (Ultrasonic):** Uses sound waves (TOF) to measure distance.
    Cheap but suffers from wide emission cones (low angular resolution)
    and specular reflections.

-   **LiDAR (Light Detection and Ranging):**

    -   **Continuous Wave (Phase Shift):** Measures phase difference of
        a continuous beam.

    -   **Pulse-based (TOF):** Measures round-trip time of laser pulses.
        Requires nanosecond timing.

    -   **Solid-state LiDAR:** Uses Optical Phased Arrays (OPA) to steer
        beams without moving mechanical parts.

-   **Radar:** Uses radio waves (FMCW). Lower resolution than LiDAR but
    robust to rain, fog, and dust. Can measure velocity directly via the
    Doppler effect.

-   **ToF Cameras:** Illuminate the whole scene with IR light to get a
    depth map in one shot. Prone to "multipath interference" (light
    bouncing off multiple surfaces).

-   **Stereo Vision:**

    -   **Passive:** Uses two cameras (baseline separation) to calculate
        depth via triangulation (finding corresponding points). Fails on
        untextured surfaces.

    -   **Active Stereo:** Projects a pattern (texture) to allow
        matching even on smooth walls.

-   **Structured Light:**

    -   **Spatial Encoding:** Projects a random dot pattern (e.g.,
        Kinect v1). Good for dynamic scenes.

    -   **Temporal Encoding:** Projects a sequence of changing
        lines/fringes. Higher accuracy but requires the scene to be
        static.

-   **RGB-D:** Combines a color camera with a depth sensor (ToF, Stereo,
    or Structured Light) to provide colored point clouds.
