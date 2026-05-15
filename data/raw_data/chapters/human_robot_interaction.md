# Human Robot-interaction

## Teleoperation, and Shared Intelligence

### Teleoperation and Telesystems

-   **Definition:** A telesystem involves a human and a robot that are
    physically separated but interacting to accomplish a task. This is
    often necessary because AI is good at symbol manipulation but poor
    at converting raw sensor data into semantic symbols (perception).

-   **Components:** A standard telesystem has seven components: Display
    and Control (at the local/human end), Sensors, Effectors, Power, and
    Control (at the remote/robot end), connected by Communication.

-   **The "Human Out-Of-The-Loop" (OOTL) Problem:** A major risk in
    automation. When a system is highly automated, the human operator
    loses "situational awareness." If the automation fails, the human
    cannot seamlessly take over because they do not understand the
    current state (e.g., the Air France accident where pilots could not
    handle a stall after autopilot disengaged).

### Taxonomy of Control and Interaction

How the human and robot share the workload allows for different control
paradigms:

-   **Direct/Manual Control:** The human closes all loops. They
    interpret the sensor data and send direct motor commands (e.g.,
    wheel speed). The robot has no intelligence.

-   **Supervisory Control:** The human sets high-level goals
    (directives), and the robot executes them autonomously using its own
    local feedback loops.

-   **Traded Control:** The human and robot take turns controlling the
    system. For example, the robot drives autonomously on a highway,
    then says, "You take it from here," handing control back to the
    human for city driving.

-   **Shared Control:** Both human and robot supply control signals
    simultaneously. The robot might handle high-frequency stability
    (e.g., keeping a drone level), while the human handles low-frequency
    guidance (e.g., moving forward).

### The "Horse Metaphor" (H-Metaphor)

To solve the complexity of controlling high-DOF robots (like humanoids
or exoskeletons), researchers use the H-Metaphor:

-   **Concept:** Controlling a robot should be like riding a horse.

-   **Role Allocation:**

    -   **The Rider (Human):** Provides high-level guidance (e.g., "Go
        to that tree," "Stop").

    -   **The Horse (Robot):** Handles the low-level mechanical
        complexity. The rider doesn't tell the horse where to place
        every hoof or how to balance; the horse does that instinctively.

-   **Shared Intelligence:** This moves beyond simple control mixing. If
    the user commands "Go Right," but the robot perceives a cliff, the
    robot (like a horse) should refuse the command or modify it to
    ensure safety, rather than blindly executing it.

### Brain-Machine Interfaces (BMI) and Neuro-Robotics

-   **Definition:** An interface that interprets user intention directly
    from the nervous system (e.g., EEG for brain waves, EMG for muscle
    activity) without physical movement.

-   **Challenges:** BMI signals are noisy, have low information transfer
    rates (few commands possible), and are complex to decode.

-   **Shared Intelligence in BMI:** Because BMI commands are uncertain
    (probabilistic), robots use "Shared Intelligence." Instead of
    blindly executing a noisy brain command, the robot combines the
    User's Probability Distribution (e.g., 60% chance user wants "Left")
    with the Environmental Affordances (e.g., "Left is blocked by a
    wall"). The robot filters the command to execute the most logical
    action.

-   **Application (INTELLEXO Project):**

    -   **Context:** Lower-limb exoskeletons for disabled users.

    -   **Approach:** Instead of learning full walking trajectories, the
        system learns Motor Primitives (basic building blocks of
        movement) from healthy human demonstrations.

    -   **Adaptation:** The exoskeleton uses an RGB-D camera to see the
        environment (e.g., stairs, obstacles) and adapts these
        primitives in real-time to generate collision-free,
        physiologically natural gait trajectories.
