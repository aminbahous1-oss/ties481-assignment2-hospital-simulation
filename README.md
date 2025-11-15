# TIES481 – Simulation Assignment 2  
### Hospital Patient Flow Simulation Using SimPy

This repository contains the implementation of **Assignment 2** for the course **TIES481 – Simulation** at the University of Jyväskylä.  
The goal is to simulate patient flow through a simplified hospital model using **SimPy**.

---

## 📌 1. System Description

The hospital process consists of the following stages:

Arrival → Preparation → Surgery (Operating Theatre) → Recovery → Exit


Patients move through these stages sequentially.  
If all units at a stage are occupied, the patient waits in the corresponding queue.

### **Resources:**

- **Preparation units**: 3  
- **Operating Theatre (OR)**: 1  
- **Recovery beds**: 3  

### **Stochastic modeling**  
All times follow **exponential distributions** with the following means:

| Process     | Mean (min) |
|-------------|------------|
| Interarrival | 25        |
| Preparation  | 40        |
| Surgery      | 20        |
| Recovery     | 40        |

---

## 📌 2. Implementation (SimPy)

The implementation follows a **process-based DES model** with the following components:

### **Classes**
- `Config` – contains all model parameters  
- `Patient` – stores individual service times (prep, surgery, recovery)

### **SimPy Processes**
- `patient_generator` – generates patients using exponential interarrival times  
- `patient_flow` – implements the full lifecycle of each patient  
- `monitor` – collects queue length and OR utilization data over time  

### **SimPy Resources**
- `prep = simpy.Resource(env, capacity=3)`  
- `operating_theatre = simpy.Resource(env, capacity=1)`  
- `recovery = simpy.Resource(env, capacity=3)`  

The Operating Theatre is **held during surgery and until a recovery bed becomes available**, ensuring correct blocking behavior.

---

## 📌 3. Performance Metrics

The simulation records:

- **Average throughput time** (arrival → end of recovery)  
- **Average entrance queue length** (before preparation)  
- **OR utilization** (exact + monitored approximation)  
- **Number of completed patients**  
- **Simulation horizon** = 20,000 minutes  

Metrics are collected through a monitoring process executed every **5 minutes**.

---

## 📌 4. Example Results
A representative run (seed = 123) gives:

simulation_time: 20000
n_completed: 555
avg_throughput_time: 3387.58
avg_entrance_queue_length: 138.99
or_utilization_exact: 0.55
or_utilization_monitored: 0.55


### **Interpretation**
- OR is the bottleneck of the system  
- Entrance queue grows due to OR congestion  
- Throughput time is high because patients wait multiple times  
- Results are consistent with queues in high-utilization systems  

---

## 📌 5. How to Run

Open the notebook:

hospital_sim_assignment2.ipynb


Run all cells in sequence.

### **Modify configuration**
Inside the notebook, edit:

```python
cfg = Config(
    interarrival_mean=25,
    prep_mean=40,
    surg_mean=20,
    rec_mean=40,
    P=3,
    R=3,
    sim_time=20000
)
```
You can change:

- number of units (P, R)

- service time means

- arrival rate

- simulation horizon

- random seed


## 📌 6. Files in This Repository

hospital_sim_assignment2.ipynb – Simulation model

Assignment2_Documentation.pdf – Full written report

README.md – This documentation

(Optional) Plots or extra experiments may be added later

## 📌 7. Authors

Mohammad Ghafouri Varzaneh

Amine Bahous

## 📌 8. Notes

This model serves as the foundation for Assignment 3, where the simulation will be extended to include:

Multiple patient types

Statistical replications

Further performance analysis

Experiment design
