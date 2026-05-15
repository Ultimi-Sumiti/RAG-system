# Software Architectures for robotics and robotics paradigms

## Robot Architectures

### The Hierarchical Paradigm (Classic AI)

-   **The Cycle:** This paradigm, dominant from the late 60s to the 80s,
    operates on a strict serial sequence: SENSE $\rightarrow$ PLAN
    $\rightarrow$ ACT.

-   **Characteristics:** It focuses heavily on "planning." The robot
    perceives the world to update an internal global model, reasons upon
    that model to generate a plan, and then executes it.

-   **Examples:**

    -   **Shakey (1967):** The first AI robot, developed by SRI. It used
        the STRIPS algorithm to plan actions based on a logical
        description of the world (predicates like `INROOM` or
        `PUSHABLE`).

    -   **The Monkey and Banana Problem:** A classic test case where a
        planner must deduce how to move a box to reach a banana. It
        highlights the reliance on a static, fully described world.

-   **Limitations (Why it failed):**

    -   **The Closed World Assumption:** It assumes the internal model
        is perfect and contains everything relevant. It cannot handle
        new, undefined objects (e.g., a human entering a "blocks
        world").

    -   **The Frame Problem:** It is computationally impossible to
        mathematically describe how every single action affects every
        aspect of the world state.

    -   **Processing Bottleneck:** The cycle is too slow for real-time
        reaction (e.g., balancing a pendulum), as the robot must
        complete the sensing and planning phases before moving.

### The Reactive Paradigm (Behavior-Based Robotics)

-   **The Revolution:** Arising in the mid-80s (notably by Rodney
    Brooks), this paradigm removes the "Plan" block and the global world
    model. It connects perception directly to action: SENSE
    $\rightarrow$ ACT.

-   **Key Philosophy:**

    -   "The world is its own best model": Instead of storing a map, the
        robot senses the environment in real-time.

    -   "Intelligence is in the eye of the observer": Complex behavior
        (like ants foraging) emerges from simple rules interacting with
        the environment, not from complex internal reasoning.

-   **Bio-Inspiration:**

    -   **Grey Walter's Tortoises (Machina Speculatrix):** Early analog
        robots (1950s) that exhibited complex behaviors (seeking light,
        recharging) using simple vacuum tube circuits.

    -   **Braitenberg Vehicles:** Theoretical agents that produce
        complex behaviors (aggression, cowardice, love) simply by
        crossing or uncrossing wires between sensors and motors.

-   **Subsumption Architecture:** Proposed by Brooks, this organizes
    behaviors into layers. Lower layers (e.g., "Avoid Objects") operate
    as fast reflexes. Higher layers (e.g., "Wander" or "Explore") can
    subsume (inhibit or suppress) the lower ones to achieve complex
    tasks. It uses Augmented Finite State Machines (AFSM) for
    implementation.

-   **Arbitration:** When multiple behaviors (e.g., "Go to Goal" vs.
    "Avoid Obstacle") trigger simultaneously, an arbiter decides the
    action using strategies like Winner-Take-All (priority-based),
    Vector Summation (cooperative), or Voting.

-   **Emergent Behavior:** Functionalities that are not explicitly coded
    but appear through interaction. For example, robots in Robocup
    passing the ball not because of a "pass" code, but because of
    dynamic role allocation logic.

### The Hybrid Paradigm

-   **Synthesis:** Neither Hierarchical (too slow) nor Reactive (no
    global awareness) is perfect. Modern robotics uses a Hybrid
    Paradigm.

-   **Structure:** It keeps reactive behaviors at the bottom for
    real-time safety and control (close to hardware) but adds a
    deliberative layer on top for long-term planning and world modeling.

-   **Operation:** The planner operates asynchronously, configuring or
    activating specific behaviors which then run reactively.

### The Hybrid Paradigm

-   **The Problem:** Purely Deliberative systems (Sense-Plan-Act) are
    too slow and struggle with the "Frame Problem" (modeling a changing
    world). Purely Reactive systems (Sense-Act) lack memory, planning,
    and global goal handling.

-   **The Solution:** The Hybrid Architecture combines both approaches
    to maximize their strengths. It organizes the robot into interacting
    layers with different time scales and levels of abstraction.

-   **Layered Structure:**

    -   **Deliberative Layer (Upper):** Handles high-level goals,
        mission planning, and maintaining a global world model. It
        operates on a slow time scale (Past, Present, Future).

    -   **Reactive Layer (Lower):** Handles low-level sensing and acting
        (behaviors) for real-time safety and reflex actions. It operates
        on a very fast time scale (Present only).

    -   **Sequencing/Interface Layer (Middle):** This is the critical
        bridge. It acts as a manager that selects which behaviors to use
        based on the high-level plan (e.g., "activate Follow Hallway
        behavior") and monitors progress to report back to the planner.
