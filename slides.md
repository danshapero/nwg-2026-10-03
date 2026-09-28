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

## The solutions

* The **orthodox** approach: Cartesian coordinates and a mesh that **evolves in time**
* The **alternative** approach: use a fixed mesh in a **moving coordinate system**

-v-

### What did I do?

Solve the **Stokes equations** in a coordinate system that follows the fluid surface.

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

* The strain rate tensor:
$$\dot\varepsilon = \frac{1}{2}\left(\nabla u + \nabla u^\*\right)$$
The part of the velocity that isn't due to rotation
* Newtonian fluid:
$$\tau = 2\mu\dot\varepsilon$$

-v-

### Stokes flow

$$L(u, p) = \int\_{\Omega}\left(\frac{1}{2}\tau : \dot\varepsilon - p\nabla\cdot u - g\cdot u \right)\mathrm dx$$

minimize this ⤴

---

## Terrain-following coordinates

-v-

### Mapped coordinates

illustration

-v-

### Mapped coordinates

$$\left[\begin{matrix} x\_1 \\\\ x\_2 \\\\ x\_3\end{matrix}\right] = \left[\begin{matrix}\xi\_1 \\\\ \xi\_2 \\\\ b(\xi\_1, \xi\_2) + \xi\_3\cdot h(\xi\_1, \xi\_2)\end{matrix}\right]$$

-v-

### Linearization

The slope of the coordinate surfaces:
$$\gamma = \nabla b + \xi\_3\cdot\nabla h$$
The derivative of the map from $\xi \to x$ is:
$$J = \left[\begin{matrix} I & 0 \\\\ \gamma^* & h\end{matrix}\right]$$

-v-

### Illustration (again)

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

### Gradients

$$\nabla\_x\phi = \frac{\mathrm d\phi}{\mathrm dx} = \frac{\mathrm d\phi}{\mathrm d\xi}\frac{\mathrm d\xi}{\mathrm dx} = \nabla\_\xi\phi\\;J^{-1}$$

-v-

### Divergences

* This one is less obvious:
$$\nabla\_x\cdot F\_x = \frac{1}{h}\nabla\_\xi\cdot (hF\_\xi)$$
Soln: How do gradients and divergences relate?
* This will come back to haunt us, remember it.

-v-

$$L(u, p) = \int\_\Omega\left(\frac{h}{2}\tau :\dot\varepsilon - p\nabla\cdot hu - hg\cdot Ju\right)\mathrm d\xi$$

where we use the formula from a few slides back to define the velocity gradient, strain rate, etc.

---

## The hard part

-v-

### Oh no

$$\nabla\_xu\_x = \nabla\_\xi\left(Ju\_\xi\right)J^{-1}$$

-v-

### Oh NO

-v-

### Will this agony never cease

-v-

### The agony shows few signs of cessation

A transport equation is a divergence in spacetime:

$$\frac{\partial}{\partial t}\rho + \nabla\cdot \rho u = 0$$

$$\leftrightarrow$$

$$\nabla\cdot F = 0 \quad \text{where}\\; F = \left[\begin{matrix}\rho \\\\ \rho u\end{matrix}\right]$$

---

### Results

-v-

### Future work

- Nonlinear rheology requires *hybridization*
- Contact problems
- Viscoelasticity
- Zero thickness

-v-

<img src="https://icepack.github.io/images/logo.svg" class="r-stretch"/>

It has some redeeming qualities
