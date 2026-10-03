---
title: Stokes flow
---

## Stokes flow in terrain-following coordinates

----

Daniel Shapero, shapero@uw.edu

University of Washington

-v-

### The problem

Simulating free-surface fluid flow is hard because **the geometry is changing in time**.

${}$

A problem in glaciology, but also in geodynamics!

-v-

The **orthodox** approach: Cartesian coordinates and a mesh that **evolves in time**

-v-

<img src="plate-1a.svg">

-v-

<img src="plate-1b.svg">

-v-

<img src="plate-1c.svg">

-v-

The **alternative**: use a fixed mesh, but a coordinate system that **follows the terrain**

<img src="plate-2.svg">


-v-

### What did I do?

* Oceanographers use TFC but ignore viscosity.
* Glaciologists use TFC on simplified equations.
* **New**: Solve the **Stokes equations** in TFC.

-v-

### Why did I do this?

**Monolithic** coupling is necessary for:
* higher-order timestepping
* adaptive timestepping

-v-

<video width="640" height="480" controls>
  <source src="perlin-glacier.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

-v-

<video width="640" height="480" controls>
  <source src="rayleigh-taylor.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>


---

## Prologue

-v-

### Main characters

| Field | Symbol | Rank | Units
| ----- | ------: | ---: | -----:
| velocity | $u$ | 1 | m yr${}^{-1}$
| stress | $\tau$ | 2 | kPa
| pressure | $p$ | 0 | kPa
| thickness | $h$ | 0 | m
| surface | $s$ | 0 | m
| bed | $b$ | 0 | m

-v-

### Constitutive law

The strain rate tensor:
$$\dot\varepsilon = \frac{1}{2}\left(\nabla u + \nabla u^\*\right)$$
Newtonian fluid:
$$\tau = 2\mu\dot\varepsilon$$

-v-

### Stokes flow

minimize total viscous power dissipation + gravity

s.t. the flow is incompressible

-v-

### Free surface

Surface elevation follows the surface velocity + SMB

$$\frac{\partial s}{\partial t} + u\big|\_{z = s}\cdot\nabla s - w\big|\_{z = s} = \dot a - \dot m$$

---

## Terrain-following coordinates

-v-

<img src="plate-2.svg">

-v-

$$\left[\begin{matrix} x\_1 \\\\ x\_2 \\\\ x\_3\end{matrix}\right] = \left[\begin{matrix}\xi\_1 \\\\ \xi\_2 \\\\ b(\xi\_1, \xi\_2) + \xi\_3\cdot h(\xi\_1, \xi\_2)\end{matrix}\right]$$

-v-

### Linearization

The slope of the coordinate surfaces:
$$\gamma \equiv \nabla b + \xi\_3\cdot\nabla h$$
The derivative of the map from $\xi \to x$ is:
$$J \equiv \frac{\mathrm dx}{\mathrm d\xi} = \left[\begin{matrix} I & 0 \\\\ \gamma^* & h\end{matrix}\right]$$

-v-

<img src="plate-3.svg">

-v-

### Now what?

Everything else is calc 3

-v-

### The volume form

$$\mathrm dx = |\det J|\mathrm d\xi = h\\;\mathrm d\xi$$

-v-

### Velocities

$$u\_x = \frac{\mathrm dx}{\mathrm dt} = \frac{\mathrm dx}{\mathrm d\xi}\\;\frac{\mathrm d\xi}{\mathrm dt} = Ju\_\xi$$

-v-

<img src="plate-4.svg">

-v-

### Gradients

$$\nabla\_x\phi = \frac{\mathrm d\phi}{\mathrm dx} = \frac{\mathrm d\phi}{\mathrm d\xi}\frac{\mathrm d\xi}{\mathrm dx} = \nabla\_\xi\phi\\;J^{-1}$$

-v-

### Stokes flow in TFC

minimize total viscous power dissipation + gravity

s.t. the flow is incompressible

**but** now the velocity gradient is:

$$\nabla\_xu\_x = \nabla(Ju)J^{-1}$$

---

## The hard part

-v-

### Oh no

$$\nabla\_xu\_x = \nabla\_\xi\left({\color{\#81A1C1}{J}} u\_\xi\right)J^{-1}$$

-v-

### Oh NO

<img src="plate-5.svg">

-v-

### Solution

Give up for a couple years

-v-

### Solution

Use *discontinuous* Galerkin (DG) methods.

-v-

<img src="plate-6.svg">

-v-

### General approach

$$\begin{align\*}
& \text{DG variational form} = \\\\
& \qquad {\color{#81A1C1}{\text{original variational form}}} \\\\
& \qquad\qquad + {\color{#A3BE8C}{\text{fluxes across facets}}} \\\\
& \qquad\qquad\qquad + {\color{#D08770}{\text{facet jump penalty}}}
\end{align\*}$$

-v-

<img src="plate-7.svg">

-v-

### DG methods

* **Old idea**: enforce continuity of $u$
* **New use**: enforce continuity of $Ju$

-v-

### DG methods

* **Pros**: extremely flexible
* **Cons**: confusing, practitioners are annoying
* DG has a **pedagogy** problem!

---

### Results

-v-

<video width="640" height="480" controls>
  <source src="perlin-glacier.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

-v-

<video width="640" height="480" controls>
  <source src="rayleigh-taylor.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>


-v-

### Future work

- Nonlinear rheology requires *hybridization*
- Contact problems
- Viscoelasticity
- Zero thickness

-v-

<img src="https://icepack.github.io/images/logo.svg" class="r-stretch"/>

It has some redeeming qualities

---

### Let's get wrecked

-v-

$$L(u, p) = \int\_\Omega\left(\frac{1}{2}\tau : \dot\varepsilon - p\nabla\cdot u - \rho g\cdot u\right)\mathrm dx$$

-v-

$$L(u, p) = \int\_\Omega\left(\frac{h}{2}\tau : \dot\varepsilon - p\nabla\cdot hu - \rho gh\cdot Ju\right)\mathrm d\xi$$

where now

$$\dot\varepsilon = \frac{1}{2}\left\\{\nabla(Ju)J^{-1} + J^{-\*}\nabla(Ju)^\*\right\\}$$
