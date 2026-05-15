# Motion Planning

## Motion Planning Fundamentals

-   **Definition:** Unlike Path Planning (pure geometry), Motion
    Planning involves computing a collision-free trajectory ($q_{start}$
    to $q_{goal}$) considering time, dynamics, and kinematics.

-   **Approaches:**

    -   **Combinatorial (Exact):** Decomposes free space ($C_{free}$)
        into cells (e.g., Exact Cell Decomposition, Voronoi Diagrams).
        It guarantees an optimal solution but is computationally heavy.

    -   **Sampling-based:** The standard for high-dimensional spaces. It
        samples random configurations to build a graph/tree.

        -   **PRM (Probabilistic Roadmap):** Builds a map during a
            learning phase, then queries it.

        -   **RRT (Rapidly-exploring Random Tree):** Grows a tree from
            the start toward random points. RRT-Connect grows two trees
            (from start and goal) to meet in the middle.

        These are probabilistically complete (probability of finding a
        solution $\rightarrow 1$ as time $\rightarrow \infty$).

    -   **Artificial Potential Fields (APF):** Treats the robot as a
        particle moving in a force field. The goal applies an attractive
        force, and obstacles apply repulsive forces.

    -   **Issue:** Prone to getting stuck in Local Minima.

    -   **OMPL:** The Open Motion Planning Library is the standard
        backend for planning in ROS (via MoveIt).

## Human-Robot Collaboration (HRC) Planning

-   **The Challenge:** Industrial scenarios are no longer static.
    Planning must account for human presence, safety, and ergonomics.

-   **Dynamic Role Allocation:** In tasks like collaborative
    transportation, the robot switches between Leader (guiding for
    precision) and Follower (compliant to human force).

-   **Policies:** Allocation can be based on force detection (admittance
    control), vision (tracking human skeleton/velocity), or distance
    from the goal.

-   **Online Adaptation:** Offline planners (like RRT) generate a
    nominal path, while online planners (like APF) modify it in
    real-time to avoid moving obstacles (humans) or align with human
    intention.
