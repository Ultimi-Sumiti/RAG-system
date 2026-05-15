# Geometric Motion Planning and Algorithms (Paper 2)

## The Geometric Path Planning Problem

-   **Definition (The Piano Mover's Problem):** The fundamental goal is
    to find a continuous collision-free path for a robot (rigid body or
    articulated links) from an initial configuration ($q_I$) to a goal
    configuration ($q_G$) amidst static obstacles.

-   **Configuration Space ($C$-Space):** To simplify planning, the
    robot's complex geometry is mapped to a single point in a
    high-dimensional space called the Configuration Space ($C$).

    -   **Dimensions:** The dimension of $C$ equals the robot's degrees
        of freedom (DOF). For example, a rigid body in 2D has a 3D
        $C$-space ($x, y, \theta$), while a rigid body in 3D has a 6D
        $C$-space (translation + rotation).

    -   **Obstacles ($C_{obs}$):** The set of all configurations where
        the robot intersects with workspace obstacles.

    -   **Free Space ($C_{free}$):** The set of configurations where the
        robot does not collide ($C \setminus C_{obs}$). The planner must
        find a path entirely within $C_{free}$.

    -   **Topology:** The $C$-space is often a manifold. For instance, a
        2-joint arm has a torus topology ($S^1 \times S^1$), and a 3D
        rotating body uses $SO(3)$, often represented by unit
        quaternions ($RP^3$) to avoid singularities.

## Complexity and Completeness

-   **Computational Hardness:** The basic motion planning problem is
    PSPACE-hard, meaning the computational effort grows exponentially
    with the number of degrees of freedom.

-   **The "Black Box" Approach:** Because calculating $C_{obs}$
    explicitly is computationally expensive, modern algorithms use a
    collision detector as a "black box." The planner proposes a specific
    configuration ($q$), and the detector simply returns "True" or
    "False" regarding collision.

-   **Completeness Types:**

    -   **Complete:** An algorithm that finds a solution if one exists,
        or reports failure in finite time if none exists (e.g., Canny's
        roadmap algorithm). These are often too slow for
        high-dimensional problems.

    -   **Resolution Complete:** Guaranteed to find a solution if the
        resolution of the grid/discretization is fine enough.

    -   **Probabilistic Completeness:** The standard for sampling-based
        planners. It guarantees that if a solution exists, the
        probability of finding it approaches $1.0$ as the number of
        samples approaches infinity. However, it cannot definitively
        report that no solution exists.

## Sampling-Based Planning

These methods avoid exact geometric modeling by sampling random points
in $C$-space to build a connectivity graph.

-   **Multi-Query Planners (PRM - Probabilistic Roadmap):** Designed for
    static environments where the map is built once and queried multiple
    times.

    -   **Construction:** Samples are drawn from $C_{free}$ and added as
        vertices. The algorithm attempts to connect nearby vertices with
        simple paths (local planner). If successful, an edge is added.

    -   **Result:** A roadmap graph $G$ that captures the connectivity
        of $C_{free}$. Queries are solved by connecting start/goal
        configurations to the roadmap and performing a graph search.

-   **Single-Query Planners (RRT - Rapidly-exploring Random Trees):**
    Designed to solve a specific ($q_I, q_G$) query immediately without
    pre-computation.

    -   **Algorithm:** The tree grows from $q_I$. In each step, a random
        sample ($\alpha(i)$) is chosen. The nearest node in the existing
        tree is extended toward this sample.

    -   **Voronoi Bias:** RRTs naturally explore unexplored space
        because the probability of selecting a node for expansion is
        proportional to the volume of its Voronoi region (the empty
        space around it).

    -   **Bidirectional Search:** Efficiency is improved by growing two
        trees (one from start, one from goal) and attempting to connect
        them.

## Combinatorial and Potential Field Approaches

These are alternative paradigms effective for lower-dimensional problems
but less scalable than sampling methods.

-   **Combinatorial Roadmaps:**

    -   **Maximum Clearance:** Retracts paths to be as far from
        obstacles as possible (Voronoi diagrams).

    -   **Shortest Path (Visibility Graph):** Connects reflex vertices
        of obstacles. Paths touch obstacle corners.

    -   **Cell Decomposition:** Breaks $C_{free}$ into simple geometric
        shapes (e.g., trapezoids). A path is found by moving between
        adjacent cells.

-   **Potential Fields:**

    -   **Concept:** The robot is treated as a particle moving in a
        differentiable field. The goal applies an attractive force, and
        obstacles apply repulsive forces.

    -   **Gradient Descent:** The robot "rolls downhill" toward the
        goal.

    -   **Local Minima Problem:** The robot may get stuck in a "basin"
        where the net force is zero but it has not reached the goal
        (e.g., a U-shaped obstacle). Navigation functions can solve this
        but are hard to construct.

## Planning with Differential Constraints

Real robots have limits on velocity and acceleration (dynamics) or
steering constraints (nonholonomic).

-   **Terminology:**

    -   **Nonholonomic:** Constraints on velocity that cannot be
        integrated into position constraints (e.g., a car cannot slide
        sideways).

    -   **Kinodynamic:** Problems involving both kinematic (velocity)
        and dynamic (force/acceleration) constraints.

    -   **Phase Space ($X$):** The search happens in a space combining
        configuration and velocity ($q, \dot{q}$).

-   **Reachability Trees:** Instead of sampling points in space, the
    planner samples control inputs (actions) over time intervals.
    Integrating these actions generates a tree of reachable
    trajectories.

-   **Decoupled Approach:** A common simplification is to plan a
    geometric path first (ignoring dynamics), then compute a velocity
    profile (time scaling) to satisfy constraints along that path.

## Extensions and Variations

-   **Time-Varying Environments:** If obstacles move, the state space
    becomes $C \times T$ (Configuration + Time). Time must strictly
    increase along the path.

-   **Multiple Robots:**

    -   **Centralized:** Treat all $m$ robots as one giant system with
        dimension $N = \sum dim(C_i)$. This is complete but
        computationally expensive.

    -   **Decoupled:** Plan for each robot individually, then coordinate
        their timing to avoid collisions. This is faster but sacrifices
        completeness.

-   **Closed Kinematic Chains:** Robots with loops (e.g., two arms
    holding one object) are constrained to a lower-dimensional manifold.
    Standard sampling fails because random samples rarely satisfy the
    loop closure constraint constraint.

-   **Algebraic Geometry (CAD):** Cylindrical Algebraic Decomposition
    provides a theoretical upper bound for solving motion planning
    exactly using polynomial models. It is doubly exponential in
    complexity and rarely used in practice, but ensures exact
    completeness.
