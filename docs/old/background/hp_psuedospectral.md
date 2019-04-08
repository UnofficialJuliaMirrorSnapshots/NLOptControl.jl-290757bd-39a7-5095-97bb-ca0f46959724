hp-psuedospectral method
========================

To solve the integral constraints within the optimal control problem we
employs the hp-pseudospectral method. The hp-psuedospectral method is an
form of Gaussian Quadrature, which uses multi-interval collocation
points.

Single Phase Optimal Control
----------------------------

Find:

> -   The state: $\mathbf{x}(t)$
> -   The control: $\mathbf{u}(t)$
> -   The integrals: $\mathbf{q}$
> -   The initial time: $t_0$
> -   The final time: $t_f$

To Minimize:
:   $$J = \Phi(\mathbf{x}(t_0),\mathbf{x}(t_f),\mathbf{q},t_0,t_f)$$

That Satisfy the Following Constraints:

> -   Dynamic Constraints:
>
> > $$\frac{\mathrm{d}\mathbf{x}}{\mathrm{d}t} = \mathbf{\psi}(\mathbf{x}(t),\mathbf{u}(t),t)$$
>
> -   Inequality Path Constraints:
>
> $$\mathbf{c}_{min} <= \mathbf{c}(\mathbf{x}(t),\mathbf{u}(t),t) <= \mathbf{c}_{max}$$
>
> -   Integral Constraints:
>
> $$q_i = \int_{t_0}^{t_f} \Upsilon_i(\mathbf{x}(t),\mathbf{u}(t),t)\, \mathrm{d}t,\;\;(i=1,....,n_q)$$
>
> -   Event Constraints:
>
> $$\mathbf{b}_{min} <= \mathbf{b}(\mathbf{x}(t_0),\mathbf{x}(t_f),t_f,\mathbf{q}) <= \mathbf{b}_{max}$$

Change of Interval
------------------

To can change the limits of the integration (in order to apply
Quadrature), we introduce $\tau \in [-1,+1]$ as a new independent
variable and perform a change of variable for $t$ in terms of $\tau$, by
defining:

> $$t = \frac{t_f - t_0}{2}\tau + \frac{t_f + t_0}{2}$$

The optimal control problem defined above (TODO: figure out equation
references), is now redefined in terms of $\tau$ as:

Find:

> -   The state: $\mathbf{x}(\tau)$
> -   The control: $\mathbf{u}(\tau)$
> -   The integrals: $\mathbf{q}$
> -   The initial time: $t_0$
> -   The final time: $t_f$

To Minimize:
:   $$J = \Phi(\mathbf{x}(-1),\mathbf{x}(+1),\mathbf{q},t_0,t_f)$$

That Satisfy the Following Constraints:

> -   Dynamic Constraints:
>
> > $$\frac{\mathrm{d}\mathbf{x}}{\mathrm{d}\tau} = \frac{t_f-t_0}{2} \mathbf{\psi}(\mathbf{x}(\tau),\mathbf{u}(\tau),\tau,t_0,t_f)$$
>
> -   Inequality Path Constraints:
>
> $$\mathbf{c}_{min} <= \mathbf{c}(\mathbf{x}(\tau),\mathbf{u}(\tau),\tau,t_0,t_f) <= \mathbf{c}_{max}$$
>
> -   Integral Constraints:
>
> $$q_i = \frac{t_f-t_0}{2} \int_{-1}^{+1} \Upsilon_i(\mathbf{x}(\tau),\mathbf{u}(\tau),\tau,t_0,t_f)\, \mathrm{d}\tau,\;\;(i=1,....,n_q)$$
>
> -   Event Constraints:
>
> $$\mathbf{b}_{min} <= \mathbf{b}(\mathbf{x}(-1),\mathbf{x}(+1),t_f,\mathbf{q}) <= \mathbf{b}_{max}$$

Divide The Interval $\tau \in [-1,+1]$
--------------------------------------

The interval $\tau \in [-1,+1]$ is now divided into a mesh of K mesh intervals as:
:   $$[T_{k-1},T_k], k = 1,...,T_K$$

with $(T_0,...,T_K)$ being the mesh points; which satisfy:
:   $$-1 = T_0 < T_1 < T_2 < T_3 < ........... < T_{K-1} < T_K = T_f = +1$$

Rewrite the Optimal Control Problem using the Mesh
--------------------------------------------------

Find:

> -   The state : $\mathbf{x}^{(k)}(\tau)$ **in mesh interval k**
> -   The control: $\mathbf{u}^{(k)}(\tau)$ **in mesh interval k**
> -   The integrals: $\mathbf{q}$
> -   The initial time: $t_0$
> -   The final time: $t_f$

To Minimize:
:   $$J = \Phi(\mathbf{x}^{(1)}(-1),\mathbf{x}^{(K)}(+1),\mathbf{q},t_0,t_f)$$

That Satisfy the Following Constraints:

-   Dynamic Constraints:

> $$\frac{\mathrm{d}\mathbf{x}^{(k)}(\tau^{(k)})}{\mathrm{d}\tau^{(k)}} = \frac{t_f-t_0}{2} \mathbf{\psi}(\mathbf{x}^{(k)}(\tau^{(k)}),\mathbf{u}^{(k)}(\tau^{(k)}),\tau^{(k)},t_0,t_f),\;\;(k=1,...,K)$$

-   Inequality Path Constraints:

$$\mathbf{c}_{min} <= \mathbf{c}(\mathbf{x}^{(k)}(\tau^{(k)}),\mathbf{u}^{(k)}(\tau^{(k)}),\tau^{(k)},t_0,t_f) <= \mathbf{c}_{max},\;\;(k=1,...,K)$$

-   Integral Constraints:

$$q_i = \frac{t_f-t_0}{2} \displaystyle\sum_{k=1}^{K} \int_{T_{k-1}}^{T_k} \Upsilon_i(\mathbf{x}^{(k)}(\tau^{(k)}),\mathbf{u}^{(k)}(\tau^{(k)}),\tau,t_0,t_f)\, \mathrm{d}\tau,\;\;(i=1,....,n_q, k=1,...,K)$$

-   Event Constraints:

$$\mathbf{b}_{min} <= \mathbf{b}(\mathbf{x}^{(1)}(-1),\mathbf{x}^{(K)}(+1),t_f,\mathbf{q}) <= \mathbf{b}_{max}$$

-   State Continuity

    > -   Also, we must **now** constrain the state to be continuous at
    >     each interior mesh point $(T_1,...T_{k-1})$ by enforcing:
    >
    >     $$\mathbf{y}^{k}(T_k) = \mathbf{y}^{k+1}(T_k)$$
    >
Optimal Control Problem Approximation
-------------------------------------

The optimal control problem will now be approximated using the Radau
Collocation Method as which follows the description provided by
b-garg2011advances. In collocation methods, the state and control are
discretized at particular points within the selected time interval. Once
this is done the problem can be transcribed into a nonlinear programming
problem (NLP) and solved using standard solvers for these types of
problems, such as IPOPT or KNITRO.

For each mesh interval $k\in[1,..,K]$:
:   $$:nowrap:$$$$\begin{eqnarray}
     \mathbf{x}^{(k)}(\tau)&\approx\mathbf{X}^{(k)}(\tau)=\displaystyle\sum_{j=1}^{N_k+1}\mathbf{X}_j^{(k)}\frac{\mathrm{d}\mathcal{L}_j^{k}(\tau)}{\mathrm{d}\tau}\\
      where,\;\;&\\
     \mathcal{L}_j^{k}(\tau)&=\prod_{\substack{l=1 \\ l\neq j}}^{N_k+1}\frac{\tau-\tau_l^{(k)}}{\tau_j^{(k)}-\tau_l^{(k)}}\\
           and,\;\;&\\
           &D_{ki}=\dot{\mathcal{L}}_i(\tau_k)=\frac{\mathrm{d}\mathcal{L}_j^{k}(\tau)}{\mathrm{d}\tau}
        \end{eqnarray}$$

also,
:   -   $\mathcal{L}_j^{(k)}(\tau),\;\;(j=1,...,N_k+1)$ is a basis of
        Lagrange polynomials
    -   $(\tau_1^{k},.....,\tau_{N_k}^{(k)})$ are the
        Legendre-Gauss-Radau collocation points in mesh interval k

        > -   defined on the subinterval $\tau^{(k)}\in[T_{k-1},T_k]$
        > -   $\tau_{N_k+1}^{(k)}=T_k$ is a noncollocated point

A basic description of Lagrange Polynomials is presented in
lagrange\_poly

The $\mathbf{D}$ matrix:
:   -   Has a size $= [N_c]\times[N_c+1]$
        :   -   with $(1<=k<=N_c), (1<=i<=N_c+1)$
            -   this non-square shape because the state approximation uses the $N_c+1$ points:
                :   $(\tau_1,...\tau_{N_c+1})$

            -   but collocation is only done at the $N_c$ LGR points:
                :   $(\tau_1,...\tau_{N_c})$

If we define the state matrix as:

$$:nowrap:$$$$\begin{equation}
  \mathbf{X}^{LGR}= \left [
  \begin{aligned}
    &\mathbf{X}_1\\
    &.\\
            &.\\
            &.\\
    &\mathbf{X}_{N_c+1}
  \end{aligned} ] \right.
\end{equation}$$

The dynamics are collocated at the $N_c$ LGR points using:

> $\mathbf{D}_k\mathbf{X}^{LGR} = \frac{(t_f-t_0)}{2}\mathbf{f}(\mathbf{X}_k,\mathbf{U}_k,\tau,t_0,t_f)\;\;for\;\;k = {1,...,Nc}$
>
> with,
> :   -   $\mathbf{D}_k$ being the $k^{th}$ row of the $\mathbf{D}$
>         matrix.
>
**References**
