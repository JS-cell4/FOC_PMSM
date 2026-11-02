
Field-Oriented Control (FOC) of PMSM with Bidirectional Inverter (Simulink)

A MATLAB/Simulink model of a closed-loop, Field-Oriented Control (FOC) system driving a Permanent Magnet Synchronous Motor (PMSM). The architecture utilizes a bidirectional three-phase inverter tied to a rechargeable battery pack, allowing for active speed tracking and energy recovery during regenerative braking.

Overview
The model regulates the speed (RPM) and electromagnetic torque of a high-performance PMSM plant under varying load conditions. By applying Clarke and Park transformations, the system maps physical three-phase AC stator currents into a decoupled synchronous rotor reference frame (d)-(q) axes), where independent PI loops track flux and torque command references.

System Description


Power Stage

- Energy Storage: Rechargeable battery pack monitoring Terminal Voltage, Current, and State of Charge (SOC).
- Inverter: Bidirectional, three-phase voltage source inverter (VSI) utilizing six power switches (IGBTs/MOSFETs) with antiparallel freewheeling diodes.
- DC Link: Integrated smoothing capacitor bank stabilizing voltage fluctuations across the DC bus.
- Motor Plant: High-efficiency Permanent Magnet Synchronous Motor (PMSM) equipped with an internal rotor position sensor.

Control Architecture (FOC Loop)

- Outer Speed Loop: A proportional-integral (PI) controller evaluates the error between the reference command speed (800 RPM base input) and measured rotor velocity to output the target torque current reference (I_q*).
- Inner Current Loops: Dual decoupled tracking loops where the direct axis reference (I_d*) is held at 0 for Maximum Torque Per Ampere (MTPA) operation, and the quadrature axis current (I_q) controls active torque generation.
- Transformations:
  # Clarke / Park Blocks: Map measured physical stator lines (i_a, i_b, i_c) into stationary (alpha\beta) and then rotating (d)-(q) space.
  # Inverse Park / Inverse Clarke Blocks: Project internal voltage controller tracking corrections (V_d, V_q) back down onto the stator phase reference frames to drive the space vector or carrier modulation layout.

Motor & System Parameters

- Nominal Base Speed: 800 RPM reference step command
- DC Bus Configuration: Battery voltage feedback line coupled to a high-capacity capacitor bus
- Inverter Topology: Dual-directional active power flow topology supporting motoring and regenerative braking
- Feedback Signals: Synchronous rotor position angle (theta _m) and rotor mechanical speed (omega _m)

Load and Measurement

  - Mechanical Load: Constant external load torque (T_m) applied directly to the rotor shaft.
- Telemetry Bus: A centralized signal demultiplexer captures and channels data into a primary visualization scope tracking:
  - Stator phase currents (i_a, i_b, i_c)
	- Rotor mechanical speed (omega _m) in rad/s and RPM equivalents)
	- Absolute rotor electrical angle (theta _m)
	- Electromagnetic output torque (T_e)
	- Battery health diagnostics (Voltage, Current, SOC %)

Requirements

- MATLAB and Simulink (R2022a  recommended)
- Simscape / Simscape Electrical (Specialized Power Systems tools)

How to Run

1. Clone the repository
cd pmsm-foc-simulink
Use code with caution.
2. Open the .slx model file in Simulink.
3. Run the simulation (Ensure the powergui block environment is configured to match your solver timeline, e.g., Discrete step 5e-06 s).
4. Double-click the rightmost Scope block to view transient speed responses, torque stabilization curves, and three-phase current waveforms
