# Hierarchical Decision and Motion Planning for Autonomous Driving (CARLA)

## Description
This project implements a hierarchical planning system for autonomous driving in the CARLA simulator. It integrates behavioral decision-making and motion planning to generate safe, smooth, and feasible vehicle trajectories under different driving scenarios.

## Motivation / Problem Statement
Autonomous vehicles must make high-level driving decisions while simultaneously generating low-level motion plans that are safe, comfortable, and dynamically feasible. This project addresses that problem by combining a rule-based behavioral planner with a trajectory and velocity planning module in a simulated environment.

## High-Level Approach
The system follows a traditional hierarchical planning architecture:

1. **Behavior Planner**
   - Implemented as a Finite State Machine (FSM)
   - Determines high-level driving intent based on the environment

2. **Motion Planner**
   - Generates candidate geometric paths using cubic spirals
   - Evaluates paths using collision checking and a cost function

3. **Velocity Profile Generator**
   - Assigns speed profiles along the selected path
   - Ensures smooth acceleration, cruising, and deceleration while respecting constraints

## Behavioral Planning (FSM)
The FSM handles high-level driving decisions using the following states:
- **Follow Lane**: Drive along the lane center at a cruising speed
- **Follow Vehicle**: Adjust speed to maintain a safe distance from a lead vehicle
- **Decelerate to Stop**: Gradually slow down when approaching a stop condition
- **Stopped**: Remain stationary until it is safe to proceed

State transitions are triggered by conditions such as proximity to junctions, stop lines, and other vehicles.

## Path and Trajectory Generation
- Paths are generated using **cubic spirals**, ensuring continuous curvature and heading
- Goal points are defined with local offsets ahead of the vehicle to improve adaptability
- Multiple candidate spirals are evaluated
- Collision checking is performed by sampling points along each path
- A cost function selects the safest and most suitable trajectory based on:
  - Lateral offset
  - Heading error
  - Curvature smoothness
  - Collision risk

## Velocity Profile Generation
Velocity profiles are generated according to the current behavioral state:
- **Follow Lane**: Maintain a target cruising speed
- **Decelerate to Stop**: Compute stopping distance using kinematic equations and reduce speed smoothly to zero
- **Follow Vehicle**: Adjust speed to preserve a safe following distance

Profiles are updated in real time to ensure physically plausible and comfortable motion.

## Project Structure
```

project/
├── carla/                  # CARLA Python API
├── planning/
│   ├── velocity_profile.py # Velocity profile generation
│   ├── utils.py            # Helper functions
│   ├── structures.py       # State and maneuver definitions
│   └── ...
└── run_planner.py          # Entry point for running the planner

````

## Requirements and Setup
- CARLA Simulator
- Python 3
- Required Python package:

### Running the Project

1. Start the CARLA simulator:

   ```bash
   ./CarlaUE4.sh
   ```
2. Ensure the CARLA Python API is added to `sys.path`
3. Run the planner:

   ```bash
   python run_planner.py
   ```

## Testing

Unit tests are provided to validate individual components such as cost functions and spiral generation.

```bash
pytest tests -p no:warnings -s
```

## Results and Outcomes

* Successful integration of behavioral planning, path generation, and velocity profiling
* Smooth state transitions and realistic vehicle motion in simulation
* Safe path selection through collision-aware trajectory evaluation

## Limitations and Assumptions

* Behavioral logic is rule-based and may struggle in ambiguous scenarios
* Cubic spiral generation is computationally more expensive than simpler methods
* Designed for structured urban driving scenarios within simulation

## References

* CARLA Autonomous Driving Simulator
* Finite State Machines for behavioral planning
* Cubic spiral trajectory generation for autonomous vehicles
