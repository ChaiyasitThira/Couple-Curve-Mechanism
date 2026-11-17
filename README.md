# Couple Curve Mechanism

## Project overview

Project name: Couple Curve Mechanism

Problem / purpose: TBD

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

The coupler curve generator was analyzed using MATLAB Symbolic Math
Toolbox. The objective was to determine the paths traced by three
points, P1, P2, and P3, attached to the coupler of a planar four-bar
linkage.

The mechanism consists of the fixed link O2–O4, the input crank O2–A,
the coupler A–B, and the output rocker O4–B. The three tracing points
are fixed relative to the coupler and therefore move together as one
rigid body.

The dimensions and input conditions used in the analysis are listed below.

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

The symbolic analysis was implemented in
`Coupler_curve_generator.mlx`. The procedure begins by defining the
joint coordinates and solving the loop-closure equations. The
coupler-point coordinates are then evaluated at 361 input angles,
corresponding to an angular increment of 1° over one complete revolution.

The selected assembly has joint B above the fixed link at the initial
configuration. The same assembly branch is retained throughout the
motion. The animation displays the moving links, joint labels, and
coupler curves. A synchronized angle cursor identifies the current
configuration on the position plots, and the Replay button repeats
the motion.

The principal result for path generation is the plot of global
y-coordinate against global x-coordinate for each tracing point.
The position-versus-crank-angle plots provide additional information
about where each point is located during the input revolution.

Changing the input angular velocity changes the time required to
traverse a curve. For the same link dimensions, point locations, and
assembly branch, the geometric path remains the same.

### 2.2. Governing Equations and Position Analysis

**Coordinate system and angle definitions**

The origin is located at O2, and the positive x-axis points from O2
toward O4. The input angle theta2 is measured counterclockwise from
the positive x-axis. The coupler angle theta3 describes the direction
from A toward B.

In the MATLAB model, theta4 is measured from the negative x-direction
toward O4–B, as defined by the coordinate equations below.

The moving-joint coordinates are

$$
A_x=l_2\cos\theta_2,\qquad
A_y=l_2\sin\theta_2
$$

$$
B_x=l_1-l_4\cos\theta_4,\qquad
B_y=l_4\sin\theta_4.
$$

Because the distance from A to B is the coupler length l3,

$$
B_x=A_x+l_3\cos\theta_3
$$

$$
B_y=A_y+l_3\sin\theta_3.
$$

Combining these expressions gives the two loop-closure equations

$$
l_2\cos\theta_2+l_3\cos\theta_3
+l_4\cos\theta_4-l_1=0
$$

$$
l_2\sin\theta_2+l_3\sin\theta_3
-l_4\sin\theta_4=0.
$$

MATLAB uses these constraints to determine the dependent angles
theta3 and theta4 for a prescribed theta2.

**Continuous position solution**

The distance between A and O4 is

$$
d=\sqrt{(l_1-A_x)^2+A_y^2}.
$$

For the selected assembly, the coupler angle can be evaluated as

$$
\theta_3=
\operatorname{atan2}(-A_y,l_1-A_x)
+\cos^{-1}\left(
\frac{l_3^2+d^2-l_4^2}{2l_3d}
\right).
$$

The rocker angle is then recovered using the convention of the model

$$
\theta_4=
\operatorname{atan2}\left(
A_y+l_3\sin\theta_3,\,
l_1-A_x-l_3\cos\theta_3
\right).
$$

This geometric form provides a finite position solution at
theta2 = 180°, where the tangent-half-angle form returned by a
symbolic solver can contain a removable singularity.

For the specified dimensions, d varies from 60 to 120 mm. Therefore,

$$
|l_3-l_4|<d<l_3+l_4
$$

is satisfied throughout the revolution. The two joint-location
circles intersect without becoming tangent, allowing the selected
assembly to complete the prescribed crank rotation.

**Locations of the tracing points**

Each tracing point is defined by a fixed distance APi and an angular
offset betai relative to the coupler direction. The construction used
in the MATLAB script is

$$
\beta_i=90^\circ-\frac{\gamma_i}{2},
\qquad
AP_i=2l_3\cos\beta_i.
$$

| Point | gamma_i | beta_i | AP_i |
|---|---|---|---|
| P1 | 72° | 54° | 88.168 mm |
| P2 | 180° | 0° | 150.000 mm |
| P3 | 216° | −18° | 142.658 mm |

The global coordinates of each point are

$$
x_{P_i}=A_x+AP_i\cos(\theta_3+\beta_i)
$$

$$
y_{P_i}=A_y+AP_i\sin(\theta_3+\beta_i).
$$

Angles expressed in degrees are converted to radians before being
combined with the symbolic angles in MATLAB.

The generated path is the set of positions

$$
\mathcal{C}_i=
\left\{
\bigl(x_{P_i}(\theta_2),y_{P_i}(\theta_2)\bigr)
:\ 0\leq\theta_2\leq2\pi
\right\}.
$$

**Interpretation of the generated paths**

P1, P2, and P3 produce different closed curves because their fixed
locations on the same coupler are different. Each curve results from
the combined translation and rotation of the coupler.

After one complete input revolution, the mechanism returns to its
initial configuration and the tracing points return to their
starting positions. Geometric consistency can be checked by
verifying AB = 75 mm and O4B = 75 mm throughout the motion.

The results demonstrate how a four-bar linkage generates repeatable
paths through the selection of link dimensions and tracing-point
locations. The present analysis evaluates the three point locations
defined in the MATLAB model. Matching a specified target curve would
require an additional synthesis or optimization procedure.

## Part III: Simscape Multibody

## Part IV: Built Mechanism
