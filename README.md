# 2GHz-Differential-Ring-Oscillator
This project presents the design, implementation, and transient verification of a high-speed 3-stage differential ring oscillator built and simulated in LTspice. The oscillator produces a stable, high-frequency 2.0 GHz differential clock output from a 3.0 V DC power supply without requiring cross-coupled stage swaps.

SPICE Models & Technology File (models.txt)

The simulation relies on an external device model library included via the `.include models.txt` SPICE directive. The primary active devices utilize short-channel 50 nm Level-54 BSIM4 MOSFET models (`N_50n` for NMOS and `P_50n` for PMOS).

BSIM4 50 nm Transistor Models (N_50n / P_50n)

Level-54 BSIM4 model cards are parameterized for high-speed sub-100 nm CMOS design. Key model parameters include an equivalent oxide thickness (toxe) of 1.4 nm, a nominal threshold voltage (vth0) of 0.22 V, and realistic gate-to-drain/source overlap capacitances (cgso = cgdo = 0.6238 nF/m). These advanced parameters capture real-world short-channel phenomena such as velocity saturation, channel-length modulation, drain-induced barrier lowering (DIBL), and parasitic capacitance loading.

Legacy 1 µm Models (N_1u / P_1u)

Standard Level-3 long-channel MOSFET models are included within the file for comparison testing and low-frequency baseline verification.

Circuit Topology & Component Sizing

The oscillator circuit comprises three identical differential gain blocks (X1, X2, X3) connected in a closed-loop topology (OUT1 to IN1, OUT2 to IN2).

Differential Input Pair (M1, M2)

NMOS transistors are sized at W = 100 µm and L = 0.1 µm (W/L = 1000), providing the high transconductance (gm > 15 mS) required to achieve significant stage gain.

Tail Current Source (M3)

An NMOS transistor is sized at W = 300 µm and L = 0.1 µm (W/L = 3000), driven by a dedicated 0.4 V DC bias voltage source (Vbias) to sink approximately 4.86 mA of total tail current.

Passive Load Resistors (R1, R2)

Dual 300 Ω resistive loads pull up each drain output to the 3.0 V power supply rail (VDD).

Oscillation Mechanics & Performance Results

DC Stability & Self-Bias

Because an odd number of differential stages (N = 3) provides net negative feedback at DC (A_DC = -A_0^3), the loop inherently settles to a stable 2.27 V DC common-mode operating point during initial startup.

Barkhausen Criterion

At 2.0 GHz, the RC pole formed by the 300 Ω load resistance and stage parasitic capacitance contributes 60° of AC phase lag per stage. Combined with the 180° DC inversion, the total phase shift around the loop reaches 360° with a stage gain A0 > 13 dB, satisfying Barkhausen startup conditions.

Transient Response

Transient analysis (`.tran 20n`) confirms that small numerical noise grows exponentially over a < 3.5 ns startup period before settling into steady-state differential oscillations with a 1.74 Vp-p differential voltage swing (0.87 Vp-p single-ended per rail, swinging between 1.85 V and 2.72 V).
