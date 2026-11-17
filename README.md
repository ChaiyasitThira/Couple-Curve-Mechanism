# Couple Curve Mechanism

## Project overview

Project name: Couple Curve Mechanism

Team: นักรบมังกรคนสุดท้าย

Group Members:

| Name | Student ID |
|---|---|
| Chaiyasit Thirayansit | 6730111821 |
| Thanaphon Wongsitthinont | 6730209721 |
| Chayanan Vongsawasdi | 6730087421 |


## Repository structure

| Path | Purpose |
|---|---|
| hardware/ | CAD models, drawings, fabrication files, and bill of materials |
| machine-skills/ | Machine commands, procedures, and skill instructions |
| Simulation/ | Models, scripts, assumptions, and simulation results |
| docs/ | Assembly, setup, operation, and maintenance instructions |
| Picture/ | Pictures used in this README |
| Animation/ | Animations used in this README |

## Part I: Basics including kinematic diagram, DoF analysis

### 1.1. Link and Joint Identification

### 1.2. Degrees of Freedom (DOF)

### 1.3. Workspace Analysis

## Part II: Symbolic Math for the Selected Mechanism

### 2.1. MATLAB Kinematic Simulation

MATLAB Symbolic Math Toolbox was used to determine the paths traced
by P1, P2, and P3 on the coupler of a planar four-bar linkage.
The model consists of the fixed link O2–O4, input crank O2–A,
coupler A–B, and output rocker O4–B.

| Parameter | Description | Value |
|---|---|---|
| l1 | Fixed-pivot distance O2–O4 | 90 mm |
| l2 | Input crank length O2–A | 30 mm |
| l3 | Coupler length A–B | 75 mm |
| l4 | Output rocker length O4–B | 75 mm |
| O2 | Input fixed pivot | (0, 0) mm |
| O4 | Output fixed pivot | (90, 0) mm |
| theta2 | Input crank angle | 0–360° |
| omega2 | Constant input angular velocity | 1 rad/s |

The loop-closure equations were solved symbolically and evaluated
at 361 crank angles with a 1° increment. A consistent assembly branch
was selected, with joint B above the fixed link at the initial position.

The animation shows the linkage motion and tracing points.
The x–y plots describe the generated paths, while the position vs
crank-angle plots show each point's coordinates throughout one revolution.

[▶ Watch MATLAB animation](Animation/Coupler_curve_generator_motion.mp4)

*Video 1. MATLAB animation showing the linkage and synchronized position, velocity, and acceleration plots.*

https://github.com/ChaiyasitThira/Couple-Curve-Mechanism/issues/1

### 2.2. Governing Equations and Position Analysis

**Joint coordinates and loop closure**

The origin is located at O2, with the positive x-axis directed toward O4.
Angles theta2 and theta3 are measured from the positive x-axis.
The model defines theta4 from the negative x-axis toward the positive
y-direction.

The joint coordinates are

$$
A_x=l_2\cos\theta_2,\qquad A_y=l_2\sin\theta_2
$$

$$
B_x=l_1-l_4\cos\theta_4,\qquad B_y=l_4\sin\theta_4.
$$

The rigid coupler imposes the loop-closure equations

$$
l_2\cos\theta_2+l_3\cos\theta_3+l_4\cos\theta_4-l_1=0
$$

$$
l_2\sin\theta_2+l_3\sin\theta_3-l_4\sin\theta_4=0.
$$

For each prescribed theta2, these equations determine theta3 and theta4.

**Coupler-point positions**

Each tracing point has a fixed distance APi from A and an angular
offset betai relative to the coupler direction. Their construction is

$$
\beta_i=90^\circ-\frac{\gamma_i}{2},
\qquad AP_i=2l_3\cos\beta_i.
$$

| Point | gamma_i | beta_i | AP_i |
|---|---|---|---|
| P1 | 72° | 54° | 88.168 mm |
| P2 | 180° | 0° | 150.000 mm |
| P3 | 216° | −18° | 142.658 mm |

The global coordinates are

$$
x_{P_i}=A_x+AP_i\cos(\theta_3+\beta_i)
$$

$$
y_{P_i}=A_y+AP_i\sin(\theta_3+\beta_i).
$$

The offsets beta_i are converted to radians for evaluation in MATLAB.
Plotting yPi against xPi over theta2 = 0–360° produces the coupler curves.

![MATLAB coupler paths](Picture/coupler_paths.png)

*Figure 1. Paths traced by P1, P2, and P3 during one complete input-crank revolution.*

## Part III: Simscape Multibody

## Part IV: Built Mechanism
