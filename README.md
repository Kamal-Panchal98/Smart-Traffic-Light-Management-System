# Smart Traffic Light Management System

An intelligent traffic signal optimization system that uses **Reinforcement Learning** and **SUMO (Simulation of Urban MObility)** to dynamically manage traffic signals and reduce vehicle waiting time.

The project simulates urban traffic networks, trains a reinforcement learning model on predefined traffic scenarios, and compares traffic congestion before and after optimization.

---

## Project Overview

Traditional traffic signals operate using fixed timing patterns that may not adapt effectively to changing traffic conditions.

This project explores a dynamic traffic-control approach where a reinforcement learning model determines which direction should receive a green signal based on the current number of vehicles waiting at each intersection.

The primary objective is to reduce **aggregate vehicle waiting time** and improve traffic flow across the simulated road network.

---

## How the System Works

Consider a city grid containing multiple traffic nodes such as:

- `n1`
- `n2`
- `n3`
- `n4`

At each node:

1. Traffic approaches from four directions.
2. The system tracks the number of vehicles waiting on each side.
3. The reinforcement learning model receives the traffic state as input.
4. The model selects which direction should receive the green signal.
5. A minimum signal duration prevents unrealistic rapid switching.
6. The simulation records vehicle waiting time and traffic performance.

The number of traffic nodes can vary depending on the road network being simulated.

---

## Training Approach

![Training Loop](https://github.com/Elcampeoncr7/Smart-Traffic-Light-Management-System/assets/71449727/01f34cda-6e57-4472-a214-a9776ed9c376)

The model is trained using multiple predefined traffic events representing different vehicle movement patterns.

Using fixed pseudo-random traffic scenarios makes experiments reproducible and allows model performance to be compared consistently across training iterations.

### Model Input

The model receives the number of vehicles waiting on each side of a traffic intersection.

### Model Output

The model determines which direction should receive the green signal.

Training across multiple traffic scenarios helps the model learn how to respond to different congestion patterns.

---

## Simulation Environment

The project uses **SUMO (Simulation of Urban MObility)** to create and simulate realistic road networks, traffic flows, intersections, and vehicle movements.

SUMO provides the simulation environment used to evaluate the traffic-control strategy before and after reinforcement learning.

### San Jose Downtown Simulation Map

![San Jose Downtown Map](/Smart%20Traffic%20Signal/maps/San_Jose_Downtown_Map.jpg)

### Training Performance

![Epoch vs Time](/Smart%20Traffic%20Signal/Visualization/time_vs_epochs.png)

---

## Simulation Comparison

### Traffic Simulation Without Trained Model

https://github.com/Elcampeoncr7/Smart-Traffic-Light-Management-System/assets/71449727/267482d7-63bf-4c41-9221-7f713b5a64a5

### Traffic Simulation With Trained Model

https://github.com/Elcampeoncr7/Smart-Traffic-Light-Management-System/assets/71449727/90c981f4-052b-4edb-9feb-f8684f6a1503

---

## Technology Stack

- Python
- Reinforcement Learning
- SUMO
- Traffic Simulation
- Data Analysis
- Tableau

---

## Repository Structure

```text
Smart Traffic Signal/
├── Demo Videos/
│   ├── After Training.mp4
│   └── Before Training.mp4
│
├── Output_data_files/
│   ├── Traffic Congestion after training...
│   ├── Traffic Congestion before training...
│   ├── trained_model.jpg
│   ├── tripinfo_original.xml
│   ├── tripinfo.xml
│   └── without_training.jpg
│
├── SUMO Simulation after Training the data/
│   └── train.py
│
├── SUMO Simulation before Training the data/
│   └── without_training.py
│
├── Trained model/
│   └── JaiShreeRam_50.bin
│
├── Visualization/
│   ├── SUMO-pjct_2.twbx
│   └── time_vs_epochs.png
│
├── data/
│   └── Training and Testing data/
│       ├── San Jose Downtown OSM file...
│       ├── Sjdt.net.xml
│       ├── Sjdt.rou.alt.xml
│       ├── Sjdt.rou.xml
│       ├── Sjdt.sumocfg
│       ├── randomTrips.py
│       └── trips.trips.xml
│
└── maps/
    └── San_Jose_Downtown_Map.jpg

### Key Files

- **`train.py`** — trains and runs the reinforcement-learning traffic signal simulation.
- **`without_training.py`** — runs the baseline SUMO simulation for comparison.
- **`Sjdt.sumocfg`** — SUMO simulation configuration.
- **`randomTrips.py`** — generates traffic routes for the SUMO network.
- **`time_vs_epochs.png`** — training-performance visualization.
- **`SUMO-pjct_2.twbx`** — Tableau workbook for traffic-analysis visualization.    

# Running the Project

## Prerequisites

Before running the project:

1. Clone or download this repository.
2. Install Python and the required project dependencies.
3. Install **SUMO GUI**.

SUMO installation instructions:

https://sumo.dlr.de/docs/Downloads.php

---

## Step 1 — Create the Traffic Network

Use SUMO's `netedit` tool to create a road network.

Save the network file inside the `maps` directory.

Example:

```text
network.net.xml
```

Generate traffic routes using:

```bash
python randomTrips.py -n network.net.xml -r routes.rou.xml -e 500
```

This generates:

```text
routes.rou.xml
```

for 500 simulation steps.

---

## Step 2 — Configure the Simulation

Specify the network and route files in the SUMO configuration file:

```xml
<input>
    <net-file value="maps/city1.net.xml"/>
    <route-files value="maps/city1.rou.xml"/>
</input>
```

---

## Step 3 — Train the Model

Run:

```bash
python train.py --train -e 50 -m model_name -s 500
```

### Arguments

- `--train` — enables training mode
- `-e` — number of training epochs
- `-m` — model name
- `-s` — number of simulation steps

Example:

```bash
python train.py --train -e 50 -m traffic_model -s 500
```

After training, performance visualizations are generated and saved in the project's visualization directory.

---

## Step 4 — Run the Trained Model

Run:

```bash
python train.py -m model_name -s 500
```

This launches the SUMO GUI and runs the simulation using the trained model.

For a fair comparison, use the same number of simulation steps during both training and testing.

The simulation reports the total vehicle waiting time after completion.

![Trained Model Output](/Smart%20Traffic%20Signal/Output_data_files/trained_model.jpg)

---

## Step 5 — Run the Baseline Simulation

To compare the trained model against the original traffic-control strategy:

```bash
python without_training.py
```

The SUMO GUI will launch and report the total waiting time for the baseline simulation.

![Baseline Simulation Output](/Smart%20Traffic%20Signal/Output_data_files/without_training.jpg)

---

## Results

### Traffic Congestion Before Training

![Traffic Congestion Before Training](https://github.com/Elcampeoncr7/Smart-Traffic-Light-Management-System/assets/71449727/c2666b0c-4cd9-461d-98d8-755380b33ad4)

### Traffic Congestion After Training

![Traffic Congestion After Training](https://github.com/Elcampeoncr7/Smart-Traffic-Light-Management-System/assets/71449727/4ce08c76-a86e-4b99-96fe-349096ac783f)

The trained and baseline simulations can be compared using vehicle waiting time and observed congestion patterns.

Exact performance improvements should be evaluated using the output generated by each simulation run.

---

## Tableau Dashboard

A Tableau dashboard was also created to visualize simulation results and traffic-performance data.

[View Tableau Dashboard](https://public.tableau.com/app/profile/maria.poulose/viz/SUMO-pjct_2/Dashboard1?publish=yes)

---

## Future Improvements

Potential extensions include:

- Multi-intersection traffic coordination
- Additional traffic-network configurations
- More complex traffic scenarios
- Alternative reinforcement learning strategies
- Real-time traffic-data integration
- Additional performance metrics and dashboards

---

## Academic Context

This project was developed as an academic technical project and is maintained on GitHub as part of my engineering portfolio.
