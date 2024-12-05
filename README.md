<div align="center">
  <img src="assets/banner-dark.png#gh-dark-mode-only" alt="Banner" style="width: 600px; height: auto;">
  <img src="assets/banner-light.png#gh-light-mode-only" alt="Banner" style="width: 600px; height: auto;">
</div>

---

### Overview

This repository implements a simulation of an **M/M/S pure loss queueing model**, a fundamental model in queueing theory used to study systems with limited server capacity and no waiting queues. Calls that cannot be immediately served are lost.

The M/M/S model is defined by:
- **M** (Markovian Inter-Arrival Times): The time between successive arrivals follows an exponential distribution.
- **M** (Markovian Service Times): The service times are exponentially distributed.
- **S** (Finite Servers): A fixed number of servers are available to serve arriving calls.

This model is commonly used in telecommunications, emergency services, and other systems where blocked arrivals result in losses rather than waiting.

---

### Events in the Simulation

The simulation revolves around two types of **events**:
1. **Arrival Event:** A new call enters the system. If a server is available, the call is served. Otherwise, the call is lost. Each arrival event generates the next arrival time (`next_arrival_time`) by sampling from the exponential inter-arrival time distribution.
2. **Departure Event:** A call completes its service and leaves the system.

Each event updates the system state, including the current number of active calls (`num_calls`) and the time (`current_time`).
Each event updates the system state, including the number of active calls (`num_calls`), number of lost calls (`num_dropped_calls`), and the time (`current_time`).

---

### Event Determination Process

At any point, the simulation determines which event (arrival or departure) occurs next:
  - **Arrival Time:** Calculated as the current time plus a random inter-arrival time (`exprnd(1 / lambda)`). This is updated every time an arrival event occurs.
  - **Departure Time:** The earliest scheduled departure time from the scheduled departures (`scheduled_departure_times`) list.

The simulation compares the next arrival time (`next_arrival_time`) and the earliest departure time (`min(scheduled_departure_times)`) to decide:
- If the arrival time is sooner, an **arrival event** occurs:
  - If servers are available (`num_calls < S`), the arriving call is served, the number of active calls (`num_calls`) increases, and a departure is scheduled.
  - If servers are full (`num_calls >= S`), the call is lost, and the lost calls counter (`num_dropped_calls`) is incremented .
  - Determines the next arrival time by sampling a new inter-arrival time.
- Otherwise, a **departure event** occurs:
  - Reduces the number of active calls (`num_calls`).
  - Removes the completed departure from the schedule.

---

### Simulation Parameters

- **Arrival Rate (λ):** The average number of arrivals per unit time.
- **Service Rate (μ):** The average number of services completed per unit time.
- **Number of Servers (S):** The total number of servers. This is an array for server utilization and loss probability statistics.
- **Total Time:** The duration of the simulation.

```matlab
lambda = 10      % arrivals / unit time
mu = 2           % services / unit time
S_range = 1:1:20 % number of servers
total_time = 100 % units
```

---

### Simulation Flow

1. Start the simulation clock at zero (`current_time = 0`).
2. Generate the first arrival event and initialize the system state:
   - Sample the time until the first arrival from the exponential distribution.
   - Log the initial system state (time and number of calls).
3. Iteratively process events by determining whether the next event is an **arrival** or **departure**:
   - Compare the time of the next arrival with the earliest departure.
   - Update the system state based on the type of event:
     - **Arrival Event:**
       - If space is available, serve the call, increase the number of active calls, schedule a departure and generate the next arrival time.
       - Otherwise, increment the lost calls counter (`num_dropped_calls`).
     - **Departure Event:**
       - Decrease the number of active calls.
       - Remove the completed departure from the schedule.
   - Log the system state after each event (time and number of calls).
4. Continue until simulation time is exceeded (`current_time >= total_time`).

This process iterates until simulations are computed with all server capacities.

---

### Visualizations

#### Number of Calls in the System Over Time

This plot tracks the evolution of the number of active calls during the simulation. The orange line represents the number of active calls at each logged event. The red dashed line shows the time-weighted average number of calls, computed across the simulation duration. The green dashed line indicates the overall maximum system capacity.

_This visualization is only created for the last server capacity (S) value. In this case for `S = 20`._

<div align="center">
  <img src="assets/graph1-dark.png#gh-dark-mode-only" alt="Number of Calls" style="width: 500px; height: auto; border-radius: 4px;">
  <img src="assets/graph1-light.png#gh-light-mode-only" alt="Number of Calls" style="width: 500px; height: auto; border-radius: 4px;">
</div>

#### Probability Distribution of Calls in the System

This histogram displays the probability distribution of the number of active calls during the simulation. The distribution reflects the system's overall load.

_This visualization is only created for the last server capacity (S) value. In this case for `S = 20`._

<div align="center">
  <img src="assets/graph2-dark.png#gh-dark-mode-only" alt="Calls Distribution" style="width: 500px; height: auto; border-radius: 4px;">
  <img src="assets/graph2-light.png#gh-light-mode-only" alt="Calls Distribution" style="width: 500px; height: auto; border-radius: 4px;">
</div>

#### Probability of Loss Across Server Capacities and Server Utilization Across Server Capacities

This plot provides two separate graphs:

1. **Probability of Loss Across Server Capacities:** This plot shows the probability of call loss for different server capacities (S), based on fixed arrival rate (λ) and service rate (μ). The red dashed line represent the trend of simulated loss probabilities across server capacities. The green dashed line indicates simulated system overall quality of service across server capacities. The cyan dashed line represents theoretical loss probability across server capacities. Orange dots are simulated loss probabilities at each server capacity.
2. **Server Utilization Across Server Capacities:** This plot represents the server utilization for different server capacities (S), based on fixed arrival rate (λ) and service rate (μ). The red dashed line shows the trend of simulated utilization across server capacities. The cyan dashed line represents theoretical utilization across server capacities. Orange dots are simulated utilizations at each server capacity.

<div align="center">
  <img src="assets/graph3-dark.png#gh-dark-mode-only" alt="Loss and Utilization" style="width: 500px; height: auto; border-radius: 4px;">
  <img src="assets/graph3-light.png#gh-light-mode-only" alt="Loss and Utilization" style="width: 500px; height: auto; border-radius: 4px;">
</div>

---

### Running the Simulation

1. Clone the repository:
   ```bash
   git clone https://github.com/mantvydasdeltuva/m_m_s-queue-simulation.git
   cd m_m_s-queue-simulation
2. Open the MATLAB script in the MATLAB editor.
3. Modify the parameters (`lambda`, `mu`, `S_range`, `total_time`) if needed.
4. Run the script to simulate and generate the visualizations.

---

### Dependencies

- MATLAB (Recommended version: R2023a or later).
- Statistics and Machine Learning Toolbox
- Curve Fitting Toolbox

---

### License

This project is licensed under the MIT License. See the LICENSE file for more details.

---

### Contributions

Contributions and suggestions are welcome. Please feel free to submit an issue or a pull request for improvements.
