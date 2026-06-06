# EV-Slip-Control-Digital-Twin

⭐ **1. Introduction**

Physical vehicle testing is often costly and time-consuming. This project leverages Altair MotionSolve and MATLAB/Simulink co-simulation to model and analyze vehicle dynamics in a virtual environment. By simulating real-world driving events, the workflow enables early performance evaluation, design optimization and improved ride comfort before physical prototyping of vehicle controllers and driver assist functions. 

<table>
  <tr>
    <td align="center">
      <b></b><br>
      <img src="Resources/TCS_Intro_GIF.gif" width="700"/>
      <br>
      <sub>
        Source: <a href="https://community.altair.com/discussion/36772/two-and-three-wheeler-vehicle-dynamics-and-durability-in-motionsolve.com">2W Vehicle Dynamics - MotionView Demo</a>
      </sub>
    </td>
  </tr>
</table>

---

🧩 **2. Challenge**

Electric two-wheelers produce high instantaneous torque and respond much faster than traditional ICE vehicles. While this improves performance, it also increases the risk of **unexpected wheel slip**, especially during critical maneuvers such as fast corner exits, riding on low-μ or variable-μ surfaces (wet roads, gravel, sand), and sudden uphill acceleration.

This becomes more significant in markets like India, where users are transitioning from conventional vehicles to high-performance electric platforms, often without a matching change in driving behavior expectations.

To address this, the Slip Reduction system in this project needs to be designed as a real-time control layer that continuously monitors wheel slip, detects traction loss under varying conditions, and applies corrective measures to maintain vehicle stability and rider safety.

---

🎯 **3. Objectives**

- Develop and validate a digital twin of a 2-wheeler system to reduce physical testing effort and development time.
- Design a robust, real-time slip detection framework for proactive traction monitoring.
- Evaluate and validate controller-based corrective actions under different riding and road conditions.

--- 
🛠 **4. Tech Stack**

- **Altair MotionView / MotionSolve** – multibody vehicle dynamics modeling and system-level simulation
- **MATLAB / Simulink** – control system design, slip detection logic, and real-time co-simulation with plant models
- **Vehicle Dynamics & Tire Models** – longitudinal dynamics, tire slip estimation, and μ-split / low-traction scenario modeling
- **Control Systems (rule-based logic)** – corrective torque intervention and stability control strategy design

---

🧠 **5. Digital Twin Modelling**

Before controller development, the real-world electric 2-wheeler was replicated in Altair MotionView as a high-fidelity digital twin. The model includes the chassis, tire dynamics, powertrain, suspension, and rider inputs to closely represent real vehicle behavior.

The twin was validated using standard and real-world drive cycles, with a focus on matching key vehicle dynamics outputs against prototype data. The simulation achieved 92–95% correlation accuracy, making it suitable for downstream control system development and testing.

<table>
  <tr>
    <td align="center">
      <img src="Resources/Configure_Suspension_Settings.PNG" width="100%"/><br>
      <sub><b>Vehicle & Rider Setup</b>: Set suspension and damping properties to match vehicle dynamics performance.</sub>
    </td>
    <td align="center">
      <img src="Resources/Configure_Road_and_Driver.PNG" width="100%"/><br>
      <sub><b>Road & Environment Setup</b>: Configure road profiles, friction levels and driving conditions to match real world.</sub>
    </td>
  </tr>
  <tr>
    <td align="center">
      <img src="Resources/Configure_Bike_and_Driver.PNG" width="100%"/><br>
      <sub><b>Suspension Tuning</b>: Defines 2-wheeler parameters (inclusing frame CAD) and rider inputs/ driver behaviour to match baseline dynamics.</sub>
    </td>
    <td align="center">
      <img src="Resources/Sensor_Settings.png" width="100%"/><br>
      <sub><b>Sensor Configuration</b>: Define virtual sensor locations to estimate critical quantities and control feedback.</sub>
    </td>
  </tr>
</table>

---

📉 **6. Control System Modelling**

- **Use-case analysis (TCS activation logic):** Evaluated acceleration/deceleration profiles to identify high-slip scenarios, highlighting maximum slip during vehicle launch and transient throttle/brake events.  
- **Jerk-based control:** Uses rate of change of acceleration to detect aggressive driver inputs and proactively reduce slip during dynamic driving conditions.  
- **Slip-ratio control:** Directly monitors slip ratio and modulates torque when predefined thresholds are exceeded for real-time traction regulation.  
- **Δω (wheel speed difference) control:** Tracks front–rear wheel angular velocity difference, optimized for launch control scenarios due to strong low-speed sensitivity.  
- **Hybrid TCS strategy:** Combines Δω control for launch and jerk-based control for cruising, validated through extensive testing across multiple driving maneuvers to ensure robust traction performance across the full operating range.

<table width="100%">
  <tr>
    <td align="center">
      <img src="Resources/Simulink.PNG" width="80%"/><br>
      <sub><b>Example Model</b>: Controller Development on Simulink.</sub>
    </td>
  </tr>
</table>

---

🧪 **7. Validation Pipeline (MIL / SIL / HIL)**

The Slip Reduction system was validated through a step-by-step approach, moving from pure simulation to real-time interaction to ensure reliable controller behavior.

🔹 **Model-in-the-Loop (MIL)** <br>
The control logic was first tested inside a full vehicle simulation (Altair MotionView + Simulink). This helped verify whether the slip control strategy works correctly under different driving scenarios like launch, braking, and low-μ conditions.

🔹 **Software-in-the-Loop (SIL)** <br>
The same controller was then run as standalone software and tested against the simulated vehicle model. This ensured the implementation behaves correctly without relying on ideal simulation blocks.

🔹 **Hardware-in-the-Loop (HIL – Prototype Setup)** <br>
A simple physical interface (breadboard-based setup) was used to manually send throttle inputs to the system in real time. This helped observe how the controller reacts to live driver-like inputs and validated responsiveness beyond pure simulation.

This layered workflow helped progressively validate the system from simulation → software → real-time interaction, improving robustness and confidence in the final control design.

---
🧪 **8. Demo Results**

The Slip Reduction system supports **five intervention levels**, allowing the controller aggressiveness to be tuned based on rider preference and operating conditions.

Higher intervention levels apply faster and stronger torque reduction upon slip detection, prioritizing traction and stability. Lower intervention levels provide a more gradual torque modulation strategy, preserving vehicle responsiveness while still mitigating wheel slip.

The following simulations demonstrate controller behavior across different slip events and intervention settings.

<p align="center">
  <img src="Resources/Traction_Control_Aggressive_GIF.gif" width="800"><br>
  <sub><b>Simulation Demo</b>: Working of Traction Control & Launch Control - Aggressive Mode</sub>
</p>

<p align="center">
  <img src="Resources/Traction_Control_Passive_GIF.gif" width="800"><br>
  <sub><b>Simulation Demo</b>: Working of Traction Control & Launch Control - Passive Mode</sub>
</p>

---

📊 **9. Project Outcomes - Key Numbers**

The developed Slip Reduction system not only improved vehicle safety and stability but also delivered measurable performance and efficiency gains.

### Key Results

- 📊 **0.2 s faster 0–60 km/h acceleration** during a full-throttle standing start compared to the baseline vehicle.
- 📊 **0.35 s faster 0–100 km/h acceleration** during a full-throttle standing start compared to the baseline vehicle.
- ⚡ **1.5–1.7% reduction in battery energy consumption** over the WLTC drive cycle relative to the baseline configuration.

Beyond these performance benefits, the primary objective of the controller—**maintaining traction and vehicle stability under critical driving conditions**—was successfully achieved. The system demonstrated robust behavior across all evaluated test scenarios and satisfied the requirements of standard vehicle dynamics validation procedures defined by ARAI, India. 

---

⚠️ **10. Data Note**

This repository showcases the **controller architecture, control logic, and representative Simulink models** developed as part of the project.

The complete vehicle model, MotionView/MotionSolve co-simulation setup, calibration data, and validation datasets remain proprietary to Raptee HV and are not included in this repository.

All shared content has been curated for demonstration and learning purposes while respecting confidentiality requirements.

---

👨‍💻 **11. Skills Demonstrated**

Through this project, I demonstrate the ability to:

- Develop end-to-end vehicle dynamics and control system models using **MATLAB/Simulink**.
- Build and validate **digital twins** of real-world mechanical systems for virtual testing and product development.
- Perform **co-simulation workflows** between Altair MotionView/MotionSolve plant models and Simulink-based controllers.
- Apply **MIL, SIL, and prototype HIL validation methodologies** to mature control strategies toward real-world deployment.

---
