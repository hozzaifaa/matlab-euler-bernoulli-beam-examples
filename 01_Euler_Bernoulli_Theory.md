# Chapter 6: The Euler-Bernoulli Beam

### Euler-Bernoulli beam equation

Assumtions:

1. The deformations are small.
2. The beam made of linear elastic isotropic material.
3. The Poisson’s ratio is ignored.
4. The plane section remain plane.

$V = M_x$

$M=EIU_{xx}$

$F = V_x = M_{xx}=EI U_{xxxx}$

$$
\rho U_{tt} +(EIU_{xx})_{xx}=F(x,t), \ \ \ 0<x<L, \ \ \ t>0
$$

where;

- $E \equiv \text{Young's modulus}$
- $I \equiv \text{Moment of inertia}$
- $\rho \equiv \text{Mass density for unit length}$

### Initial conditions:

Two initial conditions must be associated with the equation, corresponding to position and velocity of beam at $t = 0$.

$U(x,0) = \varphi(x),\ \ \ \ U_{t}(x,0)=\psi(x) \ \ \ 0\le x\le L$

### Boundary conditions:

four boundary conditions must be associated with equation, depending on the support constraints.y

- P**in support:**
    1. $U(x,t) = 0$
    2. $U_{x}(x,t) \not= 0$
    3. $U_{xx}(x,t) = 0$
    4. $U_{xxx}(x,t) \not= 0$
- **Fixed (clamped) support:**
    1. $U(x,t) = 0$
    2. $U_{x}(x,t) = 0$
    3. $U_{xx}(x,t) \not= 0$
    4. $U_{xxx}(x,t) \not= 0$
- **Free support:**
    1. $U(x,t) \not= 0$
    2. $U_{x}(x,t) \not= 0$
    3. $U_{xx}(x,t) = 0$
    4. $U_{xxx}(x,t) = 0$

### Finite Element Method

Multiplying **Euler-Bernoulli equation** by **test function** and integrate over $[0,L]$ and integrating the second integral by parts **twice** yields **weak form**:

$$
\displaystyle \int_0^L\rho U_{tt}\nu.dx +\int_0^LEIU_{xx}\nu''.dx=\int_0^LF\nu.dx+[\nu'M-\nu V]_0^L
$$

Integration by parts lowered the order of derivative with respect to $x$.

### Shape function

- Hermitian shape function (Cubic shape function) represents the displacement in different directions for the beam element
    
    $N_1=1-\cfrac{3(x_i-x_n)^2}{h^2}+\cfrac{2(x_i-x_n)^3}{h^3}$
    
    $N_2=(x_i-x_n)-\cfrac{2(x_i-x_n)^2}{h}+\cfrac{(x_i-x_n)^3}{h^2}$
    
    $N_3=\cfrac{3(x_i-x_n)^2}{h^2}-\cfrac{2(x_i-x_n)^3}{h^3}$
    
    $N_4=-\cfrac{(x_i-x_n)^2}{h}+\cfrac{(x_i-x_n)^3}{h^2}$
    
- Hermitian approximation of $u$: $u(x) = u_1N_1+u_2N_2+u_3N_3+u_4N_4= \displaystyle \sum_{j=1}^4 u_jN_j$
- $\nu(x) = \nu_1N_1+\nu_2N_2+\nu_3N_3+\nu_4N_4$
- inserting approximation solution into weak form equation

$$
\displaystyle \sum_{j=1}^4\ddot{u}_j\underbrace{\int_0^h\rho N_iN_j.dx}_{M_{ij}} +\sum_{j=1}^4u_j\underbrace{\int_0^hEIN_i''N_j''.dx}_{K_{ij}}=\underbrace{\int_0^hFN_i.dx}_{f_d}+\underbrace{[N_i'M-N_i V]_0^h}_{f_b}
$$

$M_{ij}=\cfrac{\rho h}{420}\begin{bmatrix}156&22h&54&-13h\\22h&4h^2&13h&-3h^2\\54&13h&156&-22h\\-13h&-3h^2&-22h&4h^2\end{bmatrix}$

$K_{ij}=\cfrac{EI}{h^3}\begin{bmatrix}12&6h&-12&6h\\6h&4h^2&-6h&2h^2\\-12&-6h&12&-6h\\6h&2h^2&-6h&4h^2\end{bmatrix}$

$f_d=\begin{bmatrix}\int_0^hFN_1.dx\\ \int_0^hFN_2.dx\\ \int_0^hFN_3.dx\\ \int_0^hFN_4.dx\\\end{bmatrix}$

$f_b=\begin{bmatrix}V(0)\\-M(0)\\-V(h)\\M(h)\end{bmatrix}=\begin{bmatrix}F_1\\M_1\\F_2\\M_2\end{bmatrix}$

in Matrix form

$$
\begin{bmatrix}
\text k_{11}& \text k_{12}& \text k_{13}& \text k_{14} \\
\text k_{21}& \text k_{22}& \text k_{23}& \text k_{24}\\
\text k_{31}& \text k_{32}& \text k_{33}& \text k_{34}\\
\text k_{41}& \text k_{42}& \text k_{43}& \text k_{44}
\end{bmatrix}\begin{bmatrix}u_1\\u_2\\u_3\\u_4\\\end{bmatrix}=\begin{bmatrix}f_{d1}\\f_{d2}\\f_{d3}\\f_{d4}\\\end{bmatrix}+\begin{bmatrix}f_{b1}\\f_{b2}\\f_{b3}\\f_{b4}\\\end{bmatrix}
$$

in compacted way

$$
M\ddot{u}+Ku=F
$$

# In static

### Euler-Bernoulli beam equation

$$
(EIU'')''=F, \ \ \ 0<x<L, \ \ \ t>0
$$

$$
Ku=F
$$

If two elements are considered

$[0,L] = e_1 ∪ e_2 = [x_1,x_2]∪ [x_2,x_3],$

the approximating solution is expressed as

$u =  \begin{cases}u_1N_{11} +u_2N_{21} +u_3N_{31} +u_4N_{41}, & x ∈ e_1\\
u_3N_{12} +u_4N_{22} +u_5N_{32} +u_6N_{42}, & x ∈ e_2,\end{cases}$

where the second subscript in Ni j is related to the element. Setting
$Φ_1 = N_{11},$
$Φ_2 = N_{21},$

 $Φ_3 =\begin{cases} N_{31}, x ∈ e_1,\\
N_{12}, x ∈ e_2,\end{cases}$

$Φ_4 =\begin{cases} N_{41}, x ∈ e_1,\\
N_{22}, x ∈ e_2,\end{cases}$

$Φ_5 = N_{32},$

$Φ_6 = N_{42},$

formula is rewritten as

$$
u = u_1Φ_1 +u_2Φ_2 +u_3Φ_3 +u_4Φ_4 +u_5Φ_5 +u_6Φ_6.
$$

$$
\begin{bmatrix}
\text k_{11}& \text k_{12}& \text k_{13}& \text k_{14} \\
\text k_{21}& \text k_{22}& \text k_{23}& \text k_{24}\\
\text k_{31}& \text k_{32}& \text k_{33}& \text k_{34}& \text k_{35}& \text k_{36}\\
\text k_{41}&\text k_{42}& \text k_{43}& \text k_{44}& \text k_{45}& \text k_{46} \\
& &\text k_{53}& \text k_{54}&\text k_{55}& \text k_{56}\\
& &\text k_{63}& \text k_{64}&\text k_{65}& \text k_{66}
\end{bmatrix}\begin{bmatrix}u_1\\u_2\\u_3\\u_4\\u_5\\u_6\\\end{bmatrix}=\begin{bmatrix}f_{d1}\\f_{d2}\\f_{d3}\\f_{d4}\\f_{d5}\\f_{d6}\end{bmatrix}+\begin{bmatrix}f_{b1}\\f_{b2}\\f_{b3}\\f_{b4}\\f_{b5}\\f_{b6}\end{bmatrix}
$$

### Concentrated force

$$
\rho U_{tt} +(EIU_{xx})_{xx}=F(x,t)+q_h\delta(x-x_h), \ \ \ 0<x<L, \ \ \ t>0
$$

[1. Fixed end-Free end beam subjected to a uniform load](Chapter%206%20The%20Euler-Bernoulli%20Beam%207e375993689345f181a0e970ba97458f/1%20Fixed%20end-Free%20end%20beam%20subjected%20to%20a%20uniform%20l%20ea5e9144b097466bb05313f6b30b7fbe.md)

[2. Pinned end-Pinned end beam subjected to a uniform load](Chapter%206%20The%20Euler-Bernoulli%20Beam%207e375993689345f181a0e970ba97458f/2%20Pinned%20end-Pinned%20end%20beam%20subjected%20to%20a%20unifor%20fae32381be214653b2e99bc1c5056265.md)

[3. Fixed end-Pinned end beam subjected to a parabolic load](Chapter%206%20The%20Euler-Bernoulli%20Beam%207e375993689345f181a0e970ba97458f/3%20Fixed%20end-Pinned%20end%20beam%20subjected%20to%20a%20parabol%204c70483cc7a2483a8c5ef5672839f281.md)

[4. Fixed end-Fixed end beam subjected to a uniform load and to a displacement of the first constraint](Chapter%206%20The%20Euler-Bernoulli%20Beam%207e375993689345f181a0e970ba97458f/4%20Fixed%20end-Fixed%20end%20beam%20subjected%20to%20a%20uniform%20%20ba6afae3eb3448b79d1668c3e9cd0087.md)

[5. Pinned end-Pinned end beam subjected to a uniform load and a concentrated force](Chapter%206%20The%20Euler-Bernoulli%20Beam%207e375993689345f181a0e970ba97458f/5%20Pinned%20end-Pinned%20end%20beam%20subjected%20to%20a%20unifor%20cb1e8977843a4488adb21acdffd3dd96.md)

[6. Overhanging beam with a roller support subjected to the tip load](Chapter%206%20The%20Euler-Bernoulli%20Beam%207e375993689345f181a0e970ba97458f/6%20Overhanging%20beam%20with%20a%20roller%20support%20subjected%20f5140e889ade471fb3b3dbe3bc7aaac3.md)