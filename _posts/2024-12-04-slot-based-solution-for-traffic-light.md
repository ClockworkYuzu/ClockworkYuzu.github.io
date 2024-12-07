---
layout: post
title: Intelligent Traffic Control System
subtitle: Slot-Based Solution
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/v2x.jpg
share-img: /assets/img/path.jpg
tags: [traffic system, slot-based, queue theory]
author: Yvette He
---
**Main propose:**

A slot-based solution in a traffic management project replaces traditional traffic lights by allocating specific time slots for vehicles from different directions, enabling more efficient traffic flow, reducing delays, and minimizing congestion.

**Configurations:**

Simulation period: 600s

Intersection of 2 direction:NS(North-South), EW(East-West)

Main road: NS. The main road simulates a busier road.

Crossroad: EW. The crossroad simulates a less busy road.

Vehicle arrivals follow a Poisson process where
```python
interarrival_times = np.random.exponential(1 / rate, size=n)
```

Vehicles on the main road approach at varying arrival rates:
```python
[5, 3, 2.5, 2, 1.9, 1.8, 1.7, 1.6, 1.5, 1.4, 1.3, 1.2, 1.1, 1, 0.9, 0.8, 0.7, 0.6, 0.5, 0.4, 0.3, 0.2, 0.1]
```

Vehicles on the crossroad travelling at a slower and consistent speed with arrival rate: 0.1

**Simulation Algorithm:**

```python
Traffic Light System Algorithm
# configuration:
green_ns = green_ew = 30
yellow = 2
# the time required for a single vehicle to move through the intersection.
service_time = 1

# main algorithm idea:
for each arrival_time in a single direction:
   # check traffic light status
    if is_green_light:
        # start_time: the time vehicle start entering the intersection. 
        # current_time: the time last vehicle was served
        start_time = max(arrival_time, current_time)
    else:
        calculate waiting_time for red and yellow light
        start_time = max(arrival_time, current_time) + waiting_time
    # update current system status
    total_waiting_time = start_time - arrival_time
    end_time = start_time + service_time
    current_time = end_time

Slot-based Solution Algorithm
# configuration
# service time for the same direction
safety_time_same = 1
# service time + safe period when direction changed
safety_time_switch = 2
slot_time = 8

# main algorithm idea:
for each time slot:
   calculate the weights according to the current queue length of both directions
   allocate the time slot to both directions
   for each direction in both directions:
       if no time assigned:
           break
       if direction changed:
           current_time += safety_time_switch
       while time_used + safety_time <= allocated_time:
           serve the vehicles with safety_time_same
   # A slot may extend slightly to accommodate vehicles that started crossing at   
   # the end of the slot
   current_time += extra_time_needed
```

**Main Observations:**
slot-based solution handles higher traffic demand and utilizes system capacity more efficiently

Traffic light system suffers from its “fixed waiting time” and hits the bottleneck at 0.6 vehicles/sec

![Crepe]({{ '/assets/img/capacity.jpg' | relative_url }})

**Slot-based: Capacity Analysis for Both Directions**
![Crepe]({{ '/assets/img/slot_4_avg_waiting_time.jpg' | relative_url }})
The average waiting time reduces

![Crepe]({{ '/assets/img/slot_4_capcity.jpg' | relative_url }})
The capacity reduces at the same time

Observation: the balance between capacity and average waiting time is inherently a trade-off. The choice of slot time (and overall system design) depends on the specific traffic conditions and priorities.

**SUMO Simulation**
SUMO is an open-source traffic simulation. It is designed for modeling and simulating transportation systems. Sumo simulation results:

![Crepe]({{ '/assets/img/sumo.jpg' | relative_url }})

[GithubLink](https://github.com/ClockworkYuzu/V2I_Models)