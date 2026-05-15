# Localization and mapping

## Mapping, Navigation, and Advanced Sensing

### Advanced Range Sensing (Lidar, Radar, GPS)

To build maps, robots need precise distance measurements.

#### Laser Range Finders (Lidar)

-   **Mechanism:** Lidar uses electromagnetic energy (infrared light) to
    measure distance via Phase Shift or Time of Flight (ToF) principles.
    Since light travels extremely fast, phase shift is often used for
    precision in short-range sensors.

-   **2D vs. 3D:** A standard 2D Lidar uses a rotating mirror to steer a
    single beam on a plane. 3D Lidar (often used in autonomous cars)
    uses multiple stacked lasers to scan the environment vertically and
    horizontally, creating a dense point cloud.

-   **Limitations:** Lidar struggles in rain, fog, or dust because the
    light is absorbed or scattered.

#### Radar

Uses radio waves rather than light. While it has lower resolution than
Lidar, it is robust in adverse weather (rain, fog) and is standard in
automotive adaptive cruise control.

#### GPS (Global Positioning System)

-   **Operation:** A system of satellites that transmit synchronized
    time signals. The receiver (which is passive) calculates position
    via trilateration (intersecting spheres of distance from at least
    3-4 satellites).

-   **Limitations:** GPS suffers from "Canyoning" (signals blocked by
    tall buildings), Multipath errors (signals bouncing off walls), and
    fails completely indoors or under dense tree cover (leaves absorb
    frequencies due to water content).

## The Mapping Problem: Metric vs. Topological

To navigate, a robot asks: Where am I? Where am I going? How do I get
there? The answer depends on the map type.

### Metric Maps (Quantitative)

-   **Analogy:** Like a city surveyor's blueprint or an atlas.

-   **Characteristics:** Represents the world using Cartesian
    coordinates, precise distances, and angles. It places the robot in a
    specific $(x, y, \theta)$ location.

-   **Use Case:** Essential for precise obstacle avoidance and local
    motion planning.

### Topological Maps (Qualitative)

-   **Analogy:** Like a subway (metro) map.

-   **Characteristics:** Represents the world as a Graph. Nodes are
    "Distinctive Places" (Landmarks), and edges are the connectivity
    between them. It "forgets" exact distances to focus on relationships
    (e.g., "The kitchen is connected to the hallway").

-   **Use Case:** Efficient for high-level path planning, symbolic
    reasoning, and long-distance travel.

### Hybrid Maps

Most modern robots use a hybrid approach: a Topological map for global
planning (Room A to Room B) and local Metric maps (grids) for moving
safely within the room.

## Topological Navigation and Landmarks

### Landmarks

These are the anchors of topological maps. To be useful, a landmark must
be Distinctive, Unique, and Perceivable from different viewpoints (e.g.,
a specific corner, a door, or a tower).

-   **Natural vs. Artificial:** Roboticists prefer Natural Landmarks
    (features already present, like building corners). Artificial
    Landmarks (QR codes, beacons) make sensing easier but require
    engineering the environment, which is costly and not always
    feasible.

### Distinctive Places & Hill Climbing

To reduce position uncertainty without GPS, robots use Hill Climbing
control laws. Upon detecting a landmark (like a corner), the robot moves
to maximize the "distinctiveness" of that feature (e.g., equidistant
from walls), "resetting" its position error to zero.

### Symbol Grounding

A critical concept in AI. The Topological map deals with abstract
Symbols (Node A, Node B). To move the robot, these symbols must be
"grounded" in the physical world via Control Laws (behaviors like
"follow corridor" or "move through door") linked to the graph edges.

## Configuration Space (C-Space)

To simplify path planning in metric maps, we transform the robot's
physical reality.

-   **Definition:** C-Space reduces the robot to a single point. To do
    this, we "grow" or inflate the obstacles by the robot's radius.

-   **Dimensions:** For a robot moving on a flat plane, C-Space is
    usually 3-Dimensional ($x, y$ position and $\theta$ rotation). The
    robot effectively navigates through a "tunnel" of free
    configurations.

-   **Representations:**

    -   **Occupancy Grids:** The world is discretized into a grid of
        cells (Occupied/Free). Simple but suffers from Digitization Bias
        (if an object covers 1% of a cell, the whole cell is marked
        occupied) and high memory usage.

    -   **Generalized Voronoi Graphs (GVG):** A skeleton map where paths
        are generated equidistant from all obstacles. This maximizes
        safety but isn't always the shortest path.

    -   **Meadow Maps:** Represent free space using convex polygons.

## 3D Mapping Representations

Since the real world is 3D, 2D maps often fail (e.g., a robot seeing
table legs but crashing into the tabletop).

-   **Point Clouds:** Raw, dense collection of points. High precision
    but requires unbounded memory.

-   **3D Voxel Grids:** Extends the 2D grid into 3D cubes (Voxels).
    Constant access time but extremely memory intensive.

-   **Octrees (e.g., OctoMap):** A recursive tree structure. A large
    cubic volume is divided into 8 smaller cubes only if occupied. This
    is highly memory-efficient for 3D mapping.

-   **Elevation Maps (2.5D):** A 2D grid where each cell stores a height
    value. Efficient, but cannot represent overhangs (bridges, tunnels).
    Multi-Level Surface (MLS) maps solve this by allowing multiple
    height patches per cell.

## Simultaneous Localization and Mapping (SLAM)

### The Hierarchy of Localization Problems

Before addressing mapping, one must understand localization---estimating
the robot's state. There are three levels of difficulty in this problem:

-   **Position Tracking:** The initial robot position is known (given by
    an oracle). The robot tracks its position over time by correcting
    errors as it moves. This is a local problem.

-   **Global Localization:** The robot has no prior knowledge of its
    location (e.g., it wakes up in an unknown room). It must determine
    its position from scratch. This is computationally expensive, so
    once localized, the robot usually switches to tracking.

-   **The Kidnapped Robot Problem:** The most challenging scenario. A
    well-localized robot is physically moved to a new location without
    being told (teleported). The robot must realize its current
    localization is wrong and re-localize globally.

### The SLAM Problem Definition

-   **Definition:** SLAM stands for Simultaneous Localization And
    Mapping. It is the process where an autonomous robot moves through
    an unknown environment to incrementally build a consistent map while
    simultaneously estimating its location within that map.

-   **The Chicken-and-Egg Paradox:** A map is required to localize the
    robot, but the robot's accurate location is required to build the
    map. Therefore, both must be estimated jointly.

-   **Applications:** SLAM is fundamental for autonomy in GPS-denied
    environments, such as indoors (vacuum robots), underwater,
    underground (catacombs), or on other planets (Mars rovers).

## The Probabilistic Approach

Modern robotics assumes the Open World paradigm, where sensors and
actuators are noisy and models are imperfect. Instead of exact
positions, we estimate a **Belief**---a probability density function
(PDF) over the state space.

### Bayes Filter

The recursive algorithm used to update belief. It consists of two steps:

1.  **Prediction:** Updates the belief using the Motion Model
    (actuators/odometry). This operation generally increases uncertainty
    (flattens the probability curve).

2.  **Correction (Update):** Updates the belief using the Sensor Model
    (observations). This reduces uncertainty by re-weighting the belief
    based on likelihood.

## The Kalman Filter (KF) and Extended Kalman Filter (EKF)

### Kalman Filter

An analytical solution to the Bayes Filter. It assumes the system is
**Linear** and all distributions are **Gaussian** (defined by a Mean
vector $\mu$ and Covariance matrix $\Sigma$).

-   **Operation:** It predicts the new state using matrix multiplication
    and corrects it using the Kalman Gain ($K$). $K$ acts as a weighting
    factor: if the sensor is precise (low uncertainty), $K$ is high, and
    the system trusts the measurement more than the motion prediction.

### Extended Kalman Filter (EKF)

Real-world robot motion (trigonometry) and sensors are **Non-Linear**.
The EKF solves this by Linearizing the functions around the current
estimate using a first-order Taylor expansion (computing Jacobian
matrices $G$ and $H$).

-   **Limitations:** EKF assumes the belief is a single Gaussian
    (Unimodal). It cannot represent ambiguity (e.g., "I am either in
    Room A or Room B").

## Particle Filters

-   **Concept:** A non-parametric implementation of the Bayes Filter.
    Instead of math equations, it represents belief using a set of
    samples (particles). Each particle represents a hypothesis of the
    robot's state.

-   **Density:** The density of particles in a region corresponds to the
    probability of the robot being there. This allows the filter to
    represent Multi-modal distributions (multiple peaks/hypotheses).

-   **Algorithm:**

    -   **Sampling:** Generate particles based on the motion model (add
        noise to each particle).

    -   **Weighting:** Assign a weight to each particle based on how
        well it matches the sensor reading (Sensor Model).

    -   **Resampling:** Use a "Roulette Wheel" approach to duplicate
        high-weight particles and delete low-weight ones, concentrating
        the belief.

## Solving the SLAM Problem

There are three main paradigms for solving SLAM:

### EKF SLAM

Solves Online SLAM (estimates only current pose + map). The state vector
includes the robot pose ($x, y, \theta$) and the coordinates of all
landmarks ($x_i, y_i$).

-   **Issues:** The Covariance Matrix grows quadratically with the
    number of landmarks ($N^2$), making it computationally expensive for
    large maps. It also requires perfect Data Association (knowing
    exactly which landmark you are looking at) to avoid divergence.

### Particle Filter SLAM (FastSLAM)

Solves Full SLAM (entire trajectory). Each particle represents a full
trajectory hypothesis. It is very effective for grid-based maps and
handles ambiguities well.

### Graph-based SLAM

The current state-of-the-art.

-   **Structure:** It models the problem as a Factor Graph. Nodes
    represent robot poses and landmarks. Edges represent "springs" or
    constraints derived from observations and motion.

-   **Optimization:** Solving SLAM becomes a Least Squares optimization
    problem (Maximum Likelihood Estimation). The goal is to find the
    configuration of nodes that minimizes the "tension" in all springs
    (minimizes the error between expected and actual measurements).

-   **Loop Closure:** A critical concept in Graph SLAM. When a robot
    recognizes a previously visited location, it adds a "Loop Closure"
    constraint, which drastically reduces accumulated drift and corrects
    the entire map.
