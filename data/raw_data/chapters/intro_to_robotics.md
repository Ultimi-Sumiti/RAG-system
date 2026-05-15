# Introduction to robotics

## The Evolution of Robotics and ROS 2 Framework

### The ROS 2 Framework

#### Purpose and Definition

ROS (Robot Operating System) is not an actual operating system but a
framework or middleware that sits on top of an OS (usually Linux). Its
primary goal is to facilitate the building of robot software using
reusable modules, preventing developers from having to "reinvent the
wheel" for every new robot.

#### Modularity

Success in robotics software relies on modularity. This allows
developers to use code written by others (e.g., using a localization
module from another lab) while focusing on their specific application.

#### Key Components

-   **Nodes:** These are the atomic units of processing. A node is a
    process that performs computation, such as image processing or wheel
    control. Ideally, each node should handle a single specific
    functionality.

-   **Topics:** These act as communication channels or a "whiteboard"
    where nodes exchange information. A node can publish messages to a
    topic or subscribe to read them. Topics are strictly for
    communication, not for configuration.

#### Tools and Structure

-   **Workspaces:** Directories used to organize and build ROS packages.

-   **ROS 2 Doctor:** A command-line tool (`ros2 doctor`) used to check
    if the environment is correctly set up and to diagnose issues.

## The Four Industrial Revolutions

The history of robotics is tied to the evolution of industrial
production:

-   **Industry 1.0 (Mechanization):** Began in the late 1700s,
    characterized by the use of steam and water power to activate
    mechanical machines.

-   **Industry 2.0 (Mass Production):** Focused on work organization and
    the introduction of electricity. A key innovation was the assembly
    line (inspired by slaughterhouses), which increased standardization
    and productivity by having workers perform repeated specific tasks.

-   **Industry 3.0 (Automation):** Introduced electronics, PLCs
    (Programmable Logic Controllers), and software. This era saw the
    rise of traditional industrial robots used for repetitive, dirty, or
    dangerous jobs (like painting cars to avoid toxic fumes). These
    robots follow fixed programs and require fences for safety.

-   **Industry 4.0 (The Smart Factory):** Represents the shift from mass
    production to custom-based production. It relies on cyber-physical
    systems, machine learning, big data, and cloud technology to create
    interoperable, reprogrammable systems that can adapt to different
    product needs (e.g., different car configurations) without halting
    production.

## The Failure of "Dark Factories" and the Need for Humans

### The Concept

With Industry 4.0, some companies (like Tesla and Amazon) attempted to
create "dark factories"---fully automated facilities that require no
humans, and therefore no lighting or heating.

### The Limitation

These attempts often failed because the real world is too complex and
dynamic for current robotics technology. Robots struggle with unmodeled
events (e.g., a foreign object on the floor) or mechanical failures that
require human dexterity and reasoning to fix.

### The Solution

The industry realized that collaborative robotics is more effective.
Humans are needed for supervision, high-level reasoning, and dexterity,
while robots handle heavy lifting and repetitive tasks.

## Industry 5.0 and Collaborative Robotics

### Industry 5.0

A new paradigm proposed (especially by the EU) to complement Industry
4.0. It moves beyond efficiency to focus on sustainability, resilience,
and a human-centric approach. It emphasizes the well-being of the
worker, partly to attract employees in Western countries where the
workforce is aging and shrinking.

### Collaborative Robots (Cobots)

Unlike Industry 3.0 robots, cobots are designed to work alongside humans
without protective fences.

-   **Safety:** They are safe by design, equipped with sensors to stop
    upon contact or detection of humans, unlike older robots that would
    continue moving regardless of obstacles.

-   **Benefits:** They reduce the factory footprint (no cages needed)
    and are cost-effective, making them ideal for Small and Medium
    Enterprises (SMEs).

-   **Interaction:** Advanced collaborative robots may use displays to
    indicate their future actions (e.g., gaze direction) to help humans
    predict their movements.

## Historical Lineage: Tools vs. Agents

Robotics evolved along two distinct parallel tracks that are now
converging:

-   **The "Tool" Track (Industrial):** Driven by the nuclear industry's
    need to handle radioactive materials remotely. This focused on
    precision and teleoperation, leading to the Unimate (1959), the
    first industrial robot, which was used in foundries.

-   **The "Agent" Track (Intelligent):** Driven by the space race and AI
    research (e.g., DARPA, NASA). This focused on autonomy and
    reasoning. A key milestone was Shakey (late 1960s), the first mobile
    robot able to perceive its environment, reason, and plan actions to
    achieve a goal.

**Convergence:** Modern robotics combines these tracks. Today's
collaborative robots merge the reliability and strength of industrial
"tools" with the sensing and adaptability of intelligent "agents".

## History and Market Realities

-   **First Commercial Robot:** The Unimate (1959, founded 1956) was the
    first industrial robot, not the Roomba or machines by Turing.

-   **The Roomba Success Story:** Early vacuum robots failed because
    engineers tried to make them perfect (expensive sensors, mapping)
    costing $\approx \$2,000$. iRobot designed the Roomba based on what
    people would spend on an impulse ($\$250$). It used random motion
    and simple behaviors rather than complex planning, proving that
    "good enough" autonomy can be commercially superior.

-   **Artificial Intelligence Term:** The term was coined by John
    McCarthy in 1956, not Turing.

## Autonomy, Intelligence, and Locomotion

### Autonomy vs. Automation

There is a distinct difference between automation and autonomy in
robotics, defined by how the system interacts with its environment and
data.

#### Automation (The Tool)

-   **Definition:** Automation refers to physically situated tools
    designed to perform highly repetitive, pre-planned actions.

-   **Characteristics:** It relies on signals (e.g., a sensor reading a
    voltage) rather than symbols. It operates under the **Closed World
    Assumption**, meaning the environment is perfectly modeled, nothing
    relevant changes, and there are no surprises.

-   **Decision Making:** Decisions are deterministic and handled at the
    signal level (e.g., if voltage is high, stop).

-   **Goal:** To create stable control loops for well-defined tasks.

#### Autonomy (The Agent)

-   **Definition:** Autonomy is the state of being "self-governing". In
    engineering, this does not mean "free will" or moral independence,
    but the mechanical ability to regulate oneself using feedback loops.

-   **Historical Example:** James Watt's Flyball Governor is an early
    example of autonomy. It mechanically regulated steam flow to keep
    engine speed constant without human intervention, effectively
    adapting to changes in load.

-   **Characteristics:** It operates under the **Open World
    Assumption**, where the model of the world is partial, and the robot
    must adapt to dynamic, unmodeled events (e.g., a rock on the path).

-   **Decision Making:** It uses symbols (abstract representations like
    "cat" or "door") and reasoning to generate new plans rather than
    just executing pre-written ones.

#### The "Monkey and Banana" Problem

This classic AI problem illustrates the failure of closed-world logic in
the real world. A planner might deduce how to reach bananas using a
cart, but in the real world (Open World), unmodeled constraints (like
the bananas being tied to the cart) can cause the plan to fail. This
necessitates perception and adaptation.

## Cognitive Architectures

How a robot organizes its "brain" to connect sensing to acting:

-   **Deliberative Architecture (Sense-Plan-Act):** The robot perceives
    the world, builds a model, plans an action, and then acts. This is
    slow and thoughtful (like a chess player).

-   **Reactive Architecture (Sense-Act):** A direct link between
    stimulus and response, similar to reflexes in animals (e.g., a
    cockroach running from light). It is fast but has no memory or
    long-term planning.

-   **Hybrid Architecture:** The standard for modern robotics. It
    combines a Deliberative layer (upper brain) for long-term planning
    with a Reactive layer (spinal cord/lower brain) for immediate safety
    and reflexes.
