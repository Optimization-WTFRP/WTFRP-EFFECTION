# WTFRP-EFFECTION

Test instances and computational results for **“EFFECTION: An Exact Solution Approach for the Formation and Routing of Worker Teams.”** The paper studies the Worker Team Formation and Routing Problem (WTFRP).

This repository contains **2,310 test instances** in JSON format and **nine result PDFs**.

## Directory structure

```text
WTFRP-EFFECTION/
├── README.md
├── instances/
│   ├── SA/
│   │   ├── uniform_weights/
│   │   └── standard_weights/
│   └── SB/
│       ├── granularity_1s/
│       ├── granularity_5s/
│       ├── granularity_15s/
│       ├── granularity_30s/
│       ├── granularity_60s/
│       └── granularity_300s/
└── results/
    ├── algorithm_comparison/
    ├── detailed_results/
    └── analysis/
```

## Test instances

| Directory | Uniform weights | Standard weights | Total | One stored time unit |
|---|---:|---:|---:|---|
| `instances/SA/` | 945 | 945 | 1,890 | 300 seconds |
| `instances/SB/granularity_1s/` | 135 | 135 | 270 | 1 second |
| `instances/SB/granularity_5s/` | 15 | 15 | 30 | 5 seconds |
| `instances/SB/granularity_15s/` | 15 | 15 | 30 | 15 seconds |
| `instances/SB/granularity_30s/` | 15 | 15 | 30 | 30 seconds |
| `instances/SB/granularity_60s/` | 15 | 15 | 30 | 60 seconds |
| `instances/SB/granularity_300s/` | 15 | 15 | 30 | 300 seconds |
| **Total** | **1,155** | **1,155** | **2,310** | |

**S_A** contains instances with three planning horizons (60, 90, and 120 minutes), three flight rates (10, 20, and 30 per hour), three execution-mode settings (`i`, `sf`, and `sif`), seven worker strengths (0.3–0.9), five repetitions, and two weight settings. WTFRP and WTFRPA use the same input files; WTFRPA additionally imposes the no-idle-worker assumption.

**S_B** contains 270 one-second instances and 150 coarser instances, all using the `sif` mode setting. The granularity experiment uses the `90min-30fph` subset at six granularities: 1, 5, 15, 30, 60, and 300 seconds. Its 30 one-second files are part of the complete 270-file one-second set.

### Naming and time units

For example, `90min-30fph-sif-UW_1_[0.5].json` denotes a 90-minute horizon, 30 flights per hour, slow/intermediate/fast modes, uniform weights, repetition 1, and worker strength 0.5. The absence of `-UW` denotes standard weights. The other mode settings are `i` (intermediate) and `sf` (slow/fast).

Input times are expressed in the units shown above. For example, a travel time of `3` in `granularity_15s` represents 45 seconds.

Coarser S_B inputs are obtained from one-second inputs using `ceil(time_in_seconds / granularity_seconds)` for the time fields.

### JSON fields

| Fields | Contents |
|---|---|
| `tasks`, `depot`, `skill_levels` | Task, depot, and skill-level identifiers |
| `weights` | Objective weight of each task |
| `begin_horizon`, `end_horizon`, `instants` | Planning horizon and input time indices |
| `earliest_start`, `latest_finish` | Task time windows |
| `modes`, `modesSpeedID` | Execution duration and speed label for each task–formation pair |
| `travel_times` | Travel duration for each origin–destination pair |
| `formations`, `formations_w_d` | Team composition by exact skill level and cumulative skill threshold, respectively |
| `workers`, `workers_w_d` | Workforce availability by exact skill level and cumulative skill threshold, respectively |
| `tasks_per_formation` | Formation-to-task mapping |

## Computational results

### Algorithm comparison

Each PDF covers both weight settings of S_A. The filenames containing `EFFECTION` report ECS.

| PDF | Algorithm | Setting |
|---|---|---|
| [Results_DK23_WTFRPA.pdf](results/algorithm_comparison/Results_DK23_WTFRPA.pdf) | Authors' implementation of DK23 | WTFRPA |
| [Results_Ha26_WTFRPA.pdf](results/algorithm_comparison/Results_Ha26_WTFRPA.pdf) | Ha26, deterministic configuration | WTFRPA |
| [Results_Ha26_WTFRP.pdf](results/algorithm_comparison/Results_Ha26_WTFRP.pdf) | Ha26, deterministic configuration | WTFRP |
| [Results_EFFECTION_WTFRPA.pdf](results/algorithm_comparison/Results_EFFECTION_WTFRPA.pdf) | ECS | WTFRPA |
| [Results_EFFECTION_WTFRP.pdf](results/algorithm_comparison/Results_EFFECTION_WTFRP.pdf) | ECS | WTFRP |

The algorithms are compared on the same machine, with a time limit of **300 seconds per instance under WTFRPA** and **1,800 seconds under WTFRP**. The experiments use an Intel Core i7-10750H 2.60 GHz processor, 16 GB RAM, and Gurobi 11.0.2. ECS is implemented in Java.

### Ablation and granularity experiments

| PDF | Contents |
|---|---|
| [Detailed_uniform_instances.pdf](results/detailed_results/Detailed_uniform_instances.pdf) | Uniform-weight results for algorithm variants, additional hard S_A instances, and granularity experiments |
| [Detailed_standard_instances.pdf](results/detailed_results/Detailed_standard_instances.pdf) | Corresponding standard-weight results |
| [Analysis_on_uniform_instances.pdf](results/analysis/Analysis_on_uniform_instances.pdf) | Uniform-weight ablation summaries, tour counts, and granularity analysis |
| [Analysis_on_standard_instances.pdf](results/analysis/Analysis_on_standard_instances.pdf) | Corresponding standard-weight analyses |

In each `Detailed_*` PDF, Table 1 reports ECS-NTP, ECS-NSL, and ECS-NRS; Table 2 reports additional hard S_A instances. Tables 5–10 report the medium-size instances at 300, 60, 30, 15, 5, and 1 second, respectively. Table 11 reports the complete one-second set, with a **3,600-second time limit**. The one-second records in Table 10 also appear in Table 11.

In each `Analysis_on_*` PDF, Table 1 reports EC and ECS for the ablation subset; Tables 2–3 provide grouped statistics for hard S_A instances and one-second instances.

### Reading the tables

| Entry | Meaning |
|---|---|
| `Name` | Instance identifier; combine it with the weight, mode, worker-strength, and granularity headings |
| `Cost` | Objective value |
| `Gap` | Optimality gap in ten-thousandths (‱), unless explicitly stated otherwise |
| `CPU` | Computation time in seconds |
| `ws` | Worker strength |
| `-` | Infeasible instance |
| `T` | Time limit reached |
| `M` | Memory limit reached |

## Benchmark source

The S_A benchmarks originate from Dall'Olio, G., and Kolisch, R. (2023), *Formation and Routing of Worker Teams for Airport Ground Handling Operations: A Branch-and-Price-and-Check Approach*, Transportation Science, 57(5), 1231–1251. See [TeamFormationRoutingAirport](https://github.com/giacomodallolio/TeamFormationRoutingAirport).

The S_B instances are generated for the accompanying study.
