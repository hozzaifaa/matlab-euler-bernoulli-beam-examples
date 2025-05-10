# Euler-Bernoulli Beam Theory

This document provides a concise summary of the Euler-Bernoulli beam theory, including its derivation, governing equations, boundary conditions, and links to solved examples.

## Assumptions

1. Small deformations  
2. The material is linear elastic and isotropic  
3. Plane sections remain plane after deformation  
4. No shear deformation (i.e., pure bending)

## Governing Equation (General)

$$
\rho U_{tt} + (EIU_{xx})_{xx} = F(x,t), \quad 0 < x < L, \quad t > 0
$$

- $E$: Young's modulus  
- $I$: Moment of inertia  
- $\rho$: Mass density per unit length
- $F(x,t)$: Distributied load



## Initial Conditions

- Displacement: $U(x,0) = \varphi(x)$  
- Velocity: $U_t(x,0) = \psi(x)$

## Boundary Conditions

### Pinned Support
- $U = 0$
- $U_{xx} = 0$

### Fixed Support
- $U = 0$
- $U_x = 0$

### Free Support
- $U_{xx} = 0$
- $U_{xxx} = 0$

---

## Solved Examples

1. [Fixed-Free Beam with Uniform Load](https://github.com/hozzaifaa/matlab-euler-bernoulli-beam-examples/blob/main/02_Fixed_Free_Beam_Uniform_Load.md)
2. [Pinned-Pinned Beam with Uniform Load (1)](https://github.com/hozzaifaa/matlab-euler-bernoulli-beam-examples/blob/main/03_Pinned_Pinned_Beam_Uniform_Load.md)
3. [Fixed-Pinned Beam with Parabolic Load](https://github.com/hozzaifaa/matlab-euler-bernoulli-beam-examples/blob/main/04_Fixed_Pinned_Beam_Parabolic_Load.md)
4. [Fixed-Fixed Beam with Uniform Load](https://github.com/hozzaifaa/matlab-euler-bernoulli-beam-examples/blob/main/05_Fixed_Fixed_Beam_Uniform_Load.md)
5. [Pinned-Pinned Beam with Uniform Load (2)](https://github.com/hozzaifaa/matlab-euler-bernoulli-beam-examples/blob/main/06_Pinned_Pinned_Beam_Uniform_Load_2.md)
6. [Overhanging Beam with Roller Support](https://github.com/hozzaifaa/matlab-euler-bernoulli-beam-examples/blob/main/07_Overhanging_Beam_Roller_Support.md)
