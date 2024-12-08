# Introduction

This is the draft for the  LAB01's report.

Diego Porto s313169

## Assignment 1

### Part one

In this section we will run the simulator using the two given workloads and the default timeout policy.  
At this moment, only the transitions between **RUN** and **IDLE** states are allowed.  
The PSM file used is `dpm-simulator/example/psm.txt`.

#### Workload analysis

The differences in the sensor's response times between workload_1.txt and workload_2.txt can significantly impact the DPM policy's effectiveness and behavior.

##### Impact on DPM

Fast Sensors (workload_1.txt): With a response time of 4ms, it could be better to the system to stay in RUN state compared to transition to IDLE, due to the overhead in the transitions.

Slow Sensors (workload_2.txt): With a response time of 100ms, it could be convenient to the system to switch to IDLE in the meantime, because it could happen that the overhead due to transition is still better than stay in RUN state all the time.

Shorter timeout values may be more effective in managing power consumption, as the system can quickly transition to low-power states when the sensors are not in use. But we have also avoid frequent transitions, which can be counterproductive due to the high transition overhead.

#### Workload 1

This workload uses "fast" sensors so the value is returned in 4 ms.
The file has two values in each line: the first value is the arrival time of the task and the second value stands for the duration of that task.

I will try with these 4 different timeouts values:

- 1ms
- 20ms
- 40ms
- 60ms

The command to run the first workload with a timeout of 1ms is:

```shell
./dpm_simulator -t 1 -psm example/psm.txt -wl ../workloads/workload_1.txt > results/workload_1/workload_1ms
```

Note: The results of the simulation are stored in the folder `results` divided by workload.

In the output, the `[sim]` section provides detailed results of the simulation:

- **Active and Inactive Time**: The total time the system spent in active and inactive states.
- **Total Time**: The total simulation time with and without DPM.
- **State Times**: The total time spent in each state (Run, Idle, Sleep).
- **Timeout Waiting Time**: The time spent waiting for the timeout to expire.
- **Transitions Time**: The total time spent in state transitions.
- **Number of Transitions**: The total number of state transitions.
- **Energy for Transitions**: The total energy consumed during state transitions.
- **Total Energy**: The total energy consumption with and without DPM.

**Results for Different Timeout Values**
In this first workload, we can observe the following values:

- **timeout_1ms**:
  - High number of transitions (138)
  - Low total energy with DPM (0.6997882530J)
  - Short timeout waiting time (0.086400s)

- **timeout_20ms**:
  - Moderate number of transitions (18)
  - Slightly higher total energy with DPM (0.7070946960J)
  - Longer timeout waiting time (0.400100s)

- **timeout_40ms**:
  - Moderate number of transitions (18)
  - Slightly higher total energy with DPM (0.7119600960J)
  - Longer timeout waiting time (0.580100s)

- **timeout_60ms**:
  - Moderate number of transitions (18)
  - Highest total energy with DPM among the tested values (0.7168254960J)
  - Longest timeout waiting time (0.760100s)

As we can see, different timeout values affect the number of state transitions, the total energy consumption, and the time spent in each state.
Shorter timeouts lead to more frequent transitions and overall a lower energy consumption, while longer timeouts result in fewer transitions and higher energy consumption.
This because, the overhead due to the transition between RUN and IDLE state has a lower impact on the energy consumption compared to staying on the RUN state during that time.

1. **Energy Consumption**:

- **Lower Timeout Values**: The simulations with lower timeout values (e.g., 1ms) show lower total energy consumption with DPM. For instance, the total energy with DPM for a 1ms timeout is `0.6997882530J`.
- **Higher Timeout Values**: As the timeout value increases, the total energy consumption with DPM also increases. For example, the total energy with DPM for a 60ms timeout is `0.7168254960J`.

2. **Number of Transitions**:

- **Lower Timeout Values**: Lower timeout values result in a higher number of state transitions. For example, the simulation with a 1ms timeout has `138` transitions.
- **Higher Timeout Values**: Higher timeout values result in fewer state transitions. For example, the simulations with 20ms, 40ms, and 60ms timeouts all have `18` transitions.

<s>
3. **Timeout Waiting Time**:

- **Lower Timeout Values**: The timeout waiting time is shorter for lower timeout values. For instance, the timeout waiting time for a 1ms timeout is `0.086400s`.
- **Higher Timeout Values**: The timeout waiting time increases with higher timeout values. For example, the timeout waiting time for a 60ms timeout is `0.760100s`.
</s>

4. **Transitions Time and Energy**:

- **Lower Timeout Values**: The total time and energy spent on transitions are higher, as we expected, for lower timeout values. For example, the transitions time for a 1ms timeout is `0.055200s`, and the energy for transitions is `0.0013800000J`.
- **Higher Timeout Values**: The total time and energy spent on transitions are lower for higher timeout values. For example, the transitions time for a 60ms timeout is `0.007200s`, and the energy for transitions is `0.0001800000J`, due to the lower number of transitions.

5. **State Times**:

- **Run State**: The total time spent in the Run state is slightly less for a 1ms timeout (`3.010400s`) compared to the 60ms timeout (`3.684100s`).
- **Idle State**: The total time spent in the Idle state is also relatively consistent, with slight variations. For example, the total time in the Idle state for a 1ms timeout is `1079.510900s`, while for a 60ms timeout, it is `1078.884800s`.

#### Conclusion

Overall, from the results we observed that lower timeout values lead to more frequent state transitions so higher transitions time values and energy, but overall they have lower energy consumption.

Higher timeout values result in fewer transitions, that means lower transition times and energy, but an overall higher energy consumption.

With these results, we can say that with the first workload (workload_1, fast sensors), despite the high number of transitions, it is better to keep the timeout threshold to low values like 1ms.

#### Workload 2 - slow sensors

This workload uses "slow" sensors so the value is returned in 100 ms.
The file `workload_2.txt` has two values in each line: the first value is the arrival time of the task and the second value stands for the duration of that task.

I will try with timeouts values different from the workload_1, because of the higher response time.
Timeouts

- 80ms
- 100ms
- 120ms
- 140ms
- 160ms
- 180ms
- 200ms

**Explanation of the Results**

##### Summary of Results for Different Timeout Values

1. **Total Energy Consumption**:
   - The energy consumption without DPM is always `Tot. Energy w/o DPM = 30.1388136000J`.
   The total energy consumption with DPM for different timeout values is:
     - timeout_80ms: `Tot. Energy w DPM = 0.9073267669J`
     - timeout_100ms: `Tot. Energy w DPM = 0.9464934979J`
     - timeout_120ms: `Tot. Energy w DPM = 0.9552641029J`
     - timeout_140ms: `Tot. Energy w DPM = 0.9606701029J`
     - timeout_160ms: `Tot. Energy w DPM = 0.9660761029J`
     - timeout_180ms: `Tot. Energy w DPM = 0.9714821029J`
     - timeout_200ms: `Tot. Energy w DPM = 0.9768881029J`

2. **State**:

   - Analyze the total time spent in different states (RUN and IDLE) for each timeout value:
     - timeout_80ms: `Total time in state Run = 10.469800s`, `Total time in state Idle = 1081.439100s`
     - timeout_100ms: `Total time in state Run = 11.968700s`, `Total time in state Idle = 1079.995400s`
     - timeout_120ms: `Total time in state Run = 12.306200s`, `Total time in state Idle = 1079.671900s`
     - timeout_140ms: `Total time in state Run = 12.506200s`, `Total time in state Idle = 1079.471900s`
     - timeout_160ms: `Total time in state Run = 12.706200s`, `Total time in state Idle = 1079.271900s`
     - timeout_180ms: `Total time in state Run = 12.906200s`, `Total time in state Idle = 1079.071900s`
     - timeout_200ms: `Total time in state Run = 13.106200s`, `Total time in state Idle = 1078.871900s`

3. **Number of Transitions**:

   - Observe the number of state transitions and the energy consumed for transitions:
     - timeout_80ms: `N. of transitions = 194`, `Energy for transitions = 0.0019400000J`
     - timeout_100ms: `N. of transitions = 56`, `Energy for transitions = 0.0005600000J`
     - timeout_120ms, timeout_140ms, timeout_160ms, timeout_180ms, timeout_200ms: `N. of transitions = 20`, `Energy for transitions = 0.0002000000J`

4. **Timeout Waiting Time**:

   - Examine the timeout waiting time for different timeout values. For example
     - timeout_80ms: `Timeout waiting time = 7.922800s`
     - timeout_100ms: `Timeout waiting time = 9.421700s`
     - timeout_120ms: `Timeout waiting time = 9.759200s`
     - timeout_140ms: `Timeout waiting time = 9.959200s`
     - timeout_160ms: `Timeout waiting time = 10.159200s`
     - timeout_180ms: `Timeout waiting time = 10.359200s`
     - timeout_200ms: `Timeout waiting time = 10.559200s`

##### Conclusion

Optimal Timeout Value:

As the timeout value increases, the total energy consumption with DPM slightly increases. For example, the energy consumption with DPM is 0.9073J at 80ms and increases to 0.9769J at 200ms. This suggests that shorter timeout values are more energy-efficient.

Number of Transitions:

The number of state transitions decreases significantly as the timeout value increases. In this case, there are 194 transitions at 80ms compared to only 20 transitions at 200ms. Fewer transitions can reduce the overhead and energy consumption associated with state changes but overall, despite the overhead in transitions, it is still convenient switch to IDLE state.
Timeout Waiting Time:

The timeout waiting time increases with higher timeout values. This indicates that higher timeout values result in longer periods of inactivity before transitioning to a low-power state.

#### Final conclusion

With `workload_2`, similar to `workload_1`, shorter timeouts lead to more frequent transitions but an overall lower energy consumption. While longer timeouts result in fewer transitions but higher energy consumption.
Furthermore, as we expected, the energy consumed with DPM is significantly lower than without DPM across all timeout values for both workloads.

## Assignment 1 - Part 2

Modify the timeout policy to enable transitions also to *SLEEP*.
To do this it is necessary to modify the `dpm_decide_state` function inside `dpm_policies.c` adding a new entry in the case `DPM_TIMEOUT`.
Finally in the file `dpm_policies.h` we must add in the `dpm_timeout_params` struct another parameter to save the value of the sleep timeout.

Let's compare the results the results/workload_2 folder. We will focus on the energy consumption and the time spent in different states.

### Comparison of Results

#### 1. `timeout_80ms` vs `timeout_80ms_sleep`

**timeout_80ms**:

**Comparison**:

- **Total Time in States**:
  - `timeout_80ms`: More time in Idle state (`1083.413600s`), no time in Sleep state.
  - `timeout_80ms_sleep`: All inactive time spent in Sleep state (`1088.948100s`), no time in Idle state.

- **Timeout Waiting Time**:
  - `timeout_80ms`: `5.946700s`
  - `timeout_80ms_sleep` : `0.000000s`

- **Transitions Time**:
  - `timeout_80ms`: `0.079200s`
  - `timeout_80ms_sleep` : `0.495000s`

- **Number of Transitions**:
  - Both have `198` transitions.
- **Energy for Transitions**:
  - `timeout_80ms`: `0.0019800000J`
  - `timeout_80ms_sleep`: `0.1999800000J`

- **Total Energy with DPM**:
  - `timeout_80ms`: `0.8539518719J`
  - `timeout_80ms_sleep`: `0.3682825289J`

#### 2. `timeout_100ms` vs `timeout_100ms_sleep`

**Comparison**:

- **Total Time in States**:
  - `timeout_100ms`: More time in Idle state (`1079.995400s`), no time in Sleep state.
  - `timeout_100ms_sleep`: All inactive time spent in Sleep state (`1088.948100s`), no time in Idle state.
- **Timeout Waiting Time**:
  - `timeout_100ms`: `9.421700s`
  - `timeout_100ms_sleep`: `0.000000s`
- **Transitions Time**:
  - `timeout_100ms`: `0.022400s`
  - `timeout_100ms_sleep`: `0.495000s`
- **Number of Transitions**:
  - `timeout_100ms`: `56`
  - `timeout_100ms_sleep`: `198`
- **Energy for Transitions**:
  - `timeout_100ms`: `0.0005600000J`
  - `timeout_100ms_sleep`: `0.1999800000J`
- **Total Energy with DPM**:
  - `timeout_100ms`: `0.9464934979J`
  - `timeout_100ms_sleep`: `0.3682825289J`

### Summary of Insights

1. **Energy Consumption**:
   - The simulations with the "sleep" parameter (`timeout_80ms_sleep` and `timeout_100ms_sleep`) show significantly lower total energy consumption with DPM compared to their counterparts without the "sleep" parameter.

- This is because the system spends more time in the Sleep state, which has lower power consumption.

2. **Time in States**:
   - In the "sleep" simulations, the system spends all inactive time in the Sleep state, whereas in the non-sleep simulations, the system spends most of the inactive time in the Idle state.
   - The Sleep state has lower power consumption compared to the Idle state, leading to lower overall energy consumption.

3. **Transitions**:
   - The number of transitions is higher in the "sleep" simulations, leading to higher energy consumption for transitions.
   - However, the overall energy savings from spending more time in the Sleep state outweigh the increased energy consumption from transitions.

4. **Timeout Waiting Time**:  
   - The timeout waiting time is zero in the "sleep" simulations, indicating that the system transitions to the Sleep state immediately after becoming inactive.

### Conclusion

Enabling transitions to the Sleep state significantly reduces the overall energy consumption of the system, despite the increased number of transitions and higher energy consumption for transitions. This is because the Sleep state has much lower power consumption compared to the Idle state.

---

### Comparison between RUN->IDLE and RUN->SLEEP Timeout Policies

#### What Changes?

1. **Energy Consumption**:
   - **RUN->IDLE**: The IDLE state consumes more power than the SLEEP state, resulting in higher overall energy consumption. not totally true.
   - **RUN->SLEEP**: The SLEEP state consumes less power, resulting in lower overall energy consumption.

2. **Number of Transitions**:
   - **RUN->IDLE**: Fewer transitions due to longer periods in the IDLE state.
   - **RUN->SLEEP**: More transitions due to shorter periods in the SLEEP state.

#### Best Timeout Value (TTO)

1. **RUN->IDLE**:
   - The best timeout value is typically lower, as it allows the system to transition to the IDLE state more frequently, reducing overall energy consumption. For example, a timeout value of 20ms or 40ms.

2. **RUN->SLEEP**:
   - The best timeout value is also lower, as it allows the system to transition to the SLEEP state more frequently, significantly reducing overall energy consumption. For example, a timeout value of 20ms or 40ms.

#### Overall Lower Energy

- **RUN->SLEEP** results in overall lower energy consumption compared to **RUN->IDLE**. This is because the SLEEP state has much lower power consumption than the IDLE state.

#### Changes for the Two Workloads

1. **Workload 1 (Fast Sensors)**:
   - **RUN->IDLE**: Lower timeout values lead to more frequent transitions and lower energy consumption.
   - **RUN->SLEEP**: Lower timeout values lead to more frequent transitions to the SLEEP state, resulting in significantly lower energy consumption.

2. **Workload 2 (Slow Sensors)**:
   - **RUN->IDLE**: Lower timeout values lead to more frequent transitions and lower energy consumption.
   - **RUN->SLEEP**: Lower timeout values lead to more frequent transitions to the SLEEP state, resulting in significantly lower energy consumption.

#### Why?

- The SLEEP state consumes significantly less power than the IDLE state, leading to lower overall energy consumption.
- Lower timeout values allow the system to transition to low-power states more frequently, reducing the time spent in high-power states and thus lowering overall energy consumption.
- The impact of transitions is outweighed by the energy savings from spending more time in the SLEEP state.

### Conclusion

- **RUN->SLEEP** policy results in significantly lower overall energy consumption compared to the **RUN->IDLE** policy.
- The best timeout value for both policies is typically lower (e.g., 20ms or 40ms) as it allows for more frequent transitions to low-power states.
- The **RUN->SLEEP** policy is more effective in reducing energy consumption for both workloads due to the lower power consumption of the SLEEP state.
- Lower timeout values lead to more frequent transitions, but the energy savings from spending more time in the SLEEP state outweigh the increased energy consumption from transitions, especially for the workload_1.
