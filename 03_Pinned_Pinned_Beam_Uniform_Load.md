# 2. Pinned end-Pinned end beam subjected to a uniform load

![Untitled](2%20Pinned%20end-Pinned%20end%20beam%20subjected%20to%20a%20unifor%20fae32381be214653b2e99bc1c5056265/Untitled.png)

The input for the function `function [U, V, M, F1, F2] = beam_pn_pn(L, EI, F, n)`

- $L \equiv \text{length of the beam}$
- $EI \equiv \text{Flexural rigidity}$
- $F \equiv \text{Distributed load}$
- $n \equiv \text{number of elements}$

```matlab
% n = 3;
h = L/n;
U = zeros(n+1,1);
V = zeros(n+1,1); 
M = zeros(n+1,1);
k = stiffness(h,EI);
```

$U(4,1)=V(4,1)=M(4,1)=\begin{bmatrix}0\\0\\0\\0\end{bmatrix}$ 

$k=\cfrac{EI}{h^3}\begin{bmatrix}12&6h&-12&6h\\6h&4h^2&-6h&2h^2\\-12&-6h&12&-6h\\6h&2h^2&-6h&4h^2\end{bmatrix}$

```matlab
N1 = @(xi, xn) 1 - 3*(xi - xn).^2/h.^2 + 2*(xi - xn).^3/h.^3; 
N2 = @(xi, xn) (xi - xn) - 2*(xi - xn).^2/h + (xi - xn).^3/h.^2; 
N3 = @(xi, xn) 3*(xi - xn).^2/h.^2 - 2*(xi - xn).^3/h.^3; 
N4 = @(xi, xn) -(xi - xn).^2/h + (xi - xn).^3/h.^2;
```

$N_1=1-\cfrac{3(x_i-x_n)^2}{h^2}+\cfrac{2(x_i-x_n)^3}{h^3}$

$N_2=(x_i-x_n)-\cfrac{2(x_i-x_n)^2}{h}+\cfrac{(x_i-x_n)^3}{h^2}$

$N_3=\cfrac{3(x_i-x_n)^2}{h^2}-\cfrac{2(x_i-x_n)^3}{h^3}$

$N_4=-\cfrac{(x_i-x_n)^2}{h}+\cfrac{(x_i-x_n)^3}{h^2}$

```matlab
fd = zeros(n*4,1); 
for i=1:n 
    fd((i-1)*4+1) = integral(@(xi)F(xi).*N1(xi,x(i)), x(i), x(i+1)); 
    fd((i-1)*4+2) = integral(@(xi)F(xi).*N2(xi,x(i)), x(i), x(i+1)); 
    fd((i-1)*4+3) = integral(@(xi)F(xi).*N3(xi,x(i)), x(i), x(i+1)); 
    fd((i-1)*4+4) = integral(@(xi)F(xi).*N4(xi,x(i)), x(i), x(i+1)); 
end
```

$f_d(12,1) = \begin{bmatrix}f_d(1)=\int^{x_2}_{x_1}F(x)N_1.dx\\f_d(2)=\int^{x_2}_{x_1}F(x)N_2.dx\\f_d(3)=\int^{x_2}_{x_1}F(x)N_3.dx\\f_d(4)=\int^{x_2}_{x_1}F(x)N_4.dx\\ \\f_d(5)=\int^{x_3}_{x_2}F(x)N_1.dx\\f_d(6)=\int^{x_3}_{x_2}F(x)N_2.dx\\f_d(7)=\int^{x_3}_{x_2}F(x)N_3.dx\\f_d(8)=\int^{x_3}_{x_2}F(x)N_4.dx\\ \\f_d(9)=\int^{x_4}_{x_3}F(x)N_1.dx\\f_d(10)=\int^{x_4}_{x_3}F(x)N_2.dx\\f_d(11)=\int^{x_4}_{x_3}F(x)N_3.dx\\f_d(12)=\int^{x_4}_{x_3}F(x)N_4.dx\end{bmatrix}$

```matlab
nn = n*2 + 2; %nn = 3*2 +2 =8
K = zeros(nn,nn); % Global matrix % K = zeros(8,8) 
for i=1:2:nn-3 % [1 3 5]
    K(i:i+3,i:i+3) = K(i:i+3,i:i+3)+k; 
end 
K=K([2:nn-2 nn],[2:nn-2 nn]); %K = K([2 3 4 5 6 8],[2 3 4 5 6 8]); 
```

The approximating solution is expressed as

$$
u = u_2Φ_2+u_3Φ_3 +u_4Φ_4 +u_5Φ_5 +u_6Φ_6+u_8Φ_8 .
$$

since $u_1=U(0)=0,$ $u_7=U(0)=0,$ 

![Untitled](1%20Fixed%20end-Free%20end%20beam%20subjected%20to%20a%20uniform%20l%20ea5e9144b097466bb05313f6b30b7fbe/Untitled%201.png)

$K(8,8)=\begin{bmatrix}\color{blue}\text k_{11}& \color{blue}\text k_{12}& \color{blue}\text k_{13}& \color{blue}\text k_{14}& & & & \\
\color{blue}\text k_{21}& \text k_{22}& \text k_{23}& \text k_{24}& & & & \\
\color{blue}\text k_{31}& \text k_{32}& \text k_{33}& \text k_{34}& \text k_{35}& \text k_{36}& & \\
\color{blue}\text k_{41}& \text k_{42}& \text k_{43}& \text k_{44}& \text k_{45}& \text k_{46}& & \\
& &\text k_{53}& \text k_{54}&\text k_{55}& \text k_{56}& \color{blue}\text k_{57}& \text k_{58}\\
& &\text k_{63}& \text k_{64}&\text k_{65}& \text k_{66}& \color{blue}\text k_{67}& \text k_{68}\\
& & & &\color{blue}\text k_{75}& \color{blue}\text k_{76}&\color{blue}\text k_{77}& \color{blue}\text k_{78}\\
& & & &\text k_{85}& \text k_{86}&\color{blue}\text k_{87}& \text k_{88}\\
\end{bmatrix}\Rarr K(6,6)=\begin{bmatrix}
\text k_{22}& \text k_{23}& \text k_{24}& & & & \\
\text k_{32}& \text k_{33}& \text k_{34}& \text k_{35}& \text k_{36}& & \\
\text k_{42}& \text k_{43}& \text k_{44}& \text k_{45}& \text k_{46}& & \\
&\text k_{53}& \text k_{54}&\text k_{55}& \text k_{56}& \text k_{58}\\
&\text k_{63}& \text k_{64}&\text k_{65}& \text k_{66}& \text k_{68}\\
& & &\text k_{85}& \text k_{86}& \text k_{88}\\
\end{bmatrix}$

```matlab
f = zeros(n*2+2,1); %f = zeros(nn,1);
f(1) = fd(1); 
f(2) = fd(2); 
for i=2:2:2*(n-1) %[2 4]
	f(i+1) = fd(2*i-1) + fd(2*i+1); 
	f(i+2) = fd(2*i) + fd(2*i+2);
end 
f(2*n+1) = fd(n*4-1); 
f(2*n+2) = fd(n*4);
f = f([2:nn-2 nn],1); %[2 3 4 5 6 8]
```

$f(8,1)=\begin{bmatrix}f(1) = f_d(1) \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \  \  \\
f(2)= f_d(2)\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \  \ \\
 \begin{rcases}f(3)= f_d(3)+f_d(5)\\
f(4)= f_d(4)+f_d(6)\\
f(5)= f_d(7)+f_d(9)\\f(6)= f_d(8)+f_d(10)\\
\end{rcases}\\
f(7)= f_d(11) \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \  \ \\f(8)= f_d(12)\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \  \ \end{bmatrix}\Rarr f(6,1) = \begin{bmatrix}f(2)\\f(3)\\f(4)\\f(5)\\f(6)\\f(8)\\\end{bmatrix}$ 

```matlab
% FEM 
ur = K\f; % Displacements and rotations. %ur(3:nn,1) in fxfr
for i=1:n-1 
    U(i+1) = ur(2*i);% Displacements.  %U = ur(1:2:nn,1); in fxfr
end
ur = [0; ur(1:n*2-1); 0;ur(n*2)];

```

$U_R(6,1) = K(6,6)^{-1}\times f(6,1)$ 

$U_R(6,1)=\begin{bmatrix}U_R(1)\\U_R(2)\\U_R(3)\\U_R(4)\\U_R(5)\\U_R(6)\end{bmatrix}\Rarr U_R(8,1)=\begin{bmatrix}0\\U_R(1)\\U_R(2)\\U_R(3)\\U_R(4)\\U_R(5)\\0\\U_R(6)\end{bmatrix}$

$R(4,1)=\begin{bmatrix}U_R(1)\\
U_R(3) \\U_R(5)\\
U_R(6)\end{bmatrix}$

$U(4,1)=\begin{bmatrix}0\\U_R(2) \\U_R(4)\\0\end{bmatrix}$

```matlab
V(1) = k(1,1)*ur(1) + k(1,2)*ur(2) + k(1,3)*ur(3) + k(1,4)*ur(4) - fd(1);
for i=2:n % [2 3]
    V(i) = k(1,1)*ur(2*i-1) + k(1,2)*ur(2*i) + k(1,3)*ur(2*i+1)+k(1,4)*ur(2*i+2) - fd((i-1)*4+1); 
    M(i) = -(k(2,1)*ur(2*i-1) + k(2,2)*ur(2*i) + k(2,3)*ur(2*i + 1) + k(2,4)*ur(2*i + 2)) + fd((i-1)*4 + 2);
end
V(n+1) = -(k(3,1)*ur(2*n-1) + k(3,2)*ur(2*n) + k(3,3)*ur(2*n+1) + k(3,4)*ur(2*n+2)) + fd(4*n-1);
```

since boundary conditions at node $1$ and node $4$: $M(0) = M(L) =0$

$V(4,1)=\begin{bmatrix}V(1) =
  k_{11}\cdot U_R(1)
+ k_{12}\cdot U_R(2)
+ k_{13}\cdot U_R(3)
+ k_{14}\cdot U_R(4)
- f_d(1) \\
V(2) =
  k_{11}\cdot U_R(3)
+ k_{12}\cdot U_R(4)
+ k_{13}\cdot U_R(5)
+ k_{14}\cdot U_R(6)
- f_d(5) \\
V(3) =
  k_{11}\cdot U_R(5)
+ k_{12}\cdot U_R(6)
+ k_{13}\cdot U_R(7)
+ k_{14}\cdot U_R(8)
- f_d(9)\\
V(4)=  
- k_{31}\cdot U_R(5)
- k_{32}\cdot U_R(6)
- k_{33}\cdot U_R(7)
- k_{34}\cdot U_R(8)
+ f_d(11) \end{bmatrix}$

$M(4,1)=\begin{bmatrix}M(1)=0 \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \\
M(2) =
- k_{21}\cdot U_R(3)
- k_{22}\cdot U_R(4)
- k_{23}\cdot U_R(5)
- k_{24}\cdot U_R(6)
+ f_d(6) \\
M(3) =
- k_{21}\cdot U_R(5)
- k_{22}\cdot U_R(6)
- k_{23}\cdot U_R(7)
- k_{24}\cdot U_R(8)
+ f_d(10) \\M(4)=0 \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \end{bmatrix}$

```matlab
F1 = V(1); 
F2 = -V(n+1); 
% Reactive forces.
```

$F_1=V(1)\\F_2=-F(4)$

The output from the function `function [U, V, M, F1, F2] = beam_pn_pn(L, EI, F, n)`

- $U(4,1) \equiv \text{Displacement}$
- $V(4,1)\equiv \text{Shear force}$
- $M(4,1)\equiv \text{Bending moment}$
- $F_1 \equiv \text{Reactive force}$
- $M_1 \equiv \text{Reactive moment}$