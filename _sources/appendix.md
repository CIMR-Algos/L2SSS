# Appendix

##### Appendix A: TSM ROTATION MATRIX 


As shown in {numref}`Conf_coordinate_system`, $\hat{z_l}$ is a normal unit vector to the local
facet defined as follows

$$
\hat{z_l}=\displaystyle\frac{-s_x\hat{x}-s_y\hat{y}+\hat{z}}{\sqrt{1+s_x^2+s_y^2}}
$$ (eq:A1)

where $s_x$ and $s_y$ represent the slopes in the x- and y-directions for the local facet coordinate, respectively, relative to the mean surface. In addition, $\hat{z_l}$ can be expressed in terms of zenith and
azimuth angles $\theta_n$ and $\phi_n$ of the local facet with respect to mean surface normal vector  $\hat{z}$ in the global coordinate system, by,

$$
\hat{z_l}=\sin{\theta_n}\cos{\phi_n}\hat{x}+\sin{\theta_n}\sin{\phi_n}\hat{y}+\cos{\theta_n}\hat{z}
$$ (eq:A2)

Assuming $\hat{x}$ points in the wind direction without loss of
generality, the local facet vector in the x-direction $\hat{x_l}$ can be
chosen to lie in the xz plane. By enforcing $\hat{x_l}$ to be orthogonal
to $\hat{z_l}$ , we define an angle $\beta$ to calculate $\hat{x_l}$ and $\hat{y_l}$ as :

$$
\beta=\tan^{-1}{(\tan{\theta_n}\cos{\phi_n})}
$$ (eq:A3)


Using $\beta$ defined in previous equation, $\hat{x_l}$ and $\hat{y_l}$ can be written as follows:

$$
\hat{x_l}=\displaystyle\frac{\hat{x}+s_x\hat{z}}{\sqrt{1+s_x^2}}=\cos{\beta}\hat{x}-\sin{\beta}\hat{z}
$$ (eq:A4)


$$
\hat{y_l}=\hat{z_l}\times\hat{x_l}
$$ (eq:A5)


Therefore, we can relate the unit vectors of the local
coordinate system with those of the global coordinate system
using a matrix, where the zenith and azimuth angles of the local normal vector
can be calculated using the following relationship:

$$
\theta_n=\displaystyle\cos^{-1}{\left(\frac{1}{\sqrt{s_x^2+s_y^2+1}}\right)}
$$ (eq:A6)


$$
\phi_n=\tan^{-1}{\left(\frac{s_y}{s_x}\right)}
$$ (eq:A7)


The electromagnetic wave vector $\bar{k}$ in the global coordinate
system can be related to that in the local coordinate system $\bar{k_l}$ by implementing an associated matrix function 
$\overline{\overline A}$, that is,

$$
\bar{k_l}=\overline{\overline Ak}
$$ (eq:A8)


Now, the relationship between the polarization basis vector
directions in the local and global coordinates can be determined.
For the two linear polarization unit vectors $(\hat{h},\hat{v})$ and
in the global coordinate system,

$$
\hat{h}=\displaystyle\frac{\bar{k}\times\bar{z}}{|\bar{k}\times\bar{z}|}
$$ (eq:A9)


$$
\hat{v}=\displaystyle\frac{\bar{h}\times\bar{k}}{|\bar{h}\times\bar{k}|}
$$ (eq:A10)


where $\hat{h}$ and $\hat{v}$ are the global unit vectors for horizontal and
vertical polarizations, respectively. Thus, for these unit vectors
in the local coordinate system,


$$
\hat{h_l}=\displaystyle\frac{\bar{k}_l\times\bar{z}_l}{|\bar{k}_l\times\bar{z}_l|}
$$ (eq:A11)


$$
\hat{v_l}=\displaystyle\frac{\bar{h}_l\times\bar{k}_l}{|\bar{h}_l\times\bar{k}_l|}
$$ (eq:A12)


Using the inner product, the cross-polarization angle $\alpha_r$  can
be obtained as

$$
\alpha_r=\cos^{-1}{(\hat{v}\cdot\hat{v_l})}=\cos^{-1}{(\hat{h}\cdot\hat{h_l})}
$$ (eq:A13)


or

$$
\alpha_r=\sin^{-1}{(\hat{v}\cdot\hat{h_l})}=\sin^{-1}{(-\hat{h}\cdot\hat{v_l})}
$$ (eq:A14)


The scattered electric field vector $\bar{E}$ in the local coordinate
system is written as $\bar{E}=E_{hl}\hat{h_l}+E_{vl}\hat{v_l}$, where $E_{hl}$ and $E_{vl}$ are the horizontally and vertically polarized electric field
components in the local polarization coordinate, respectively.
Thus, the linearly polarized component of electric fields in
global coordinate can be written as


$$
\begin{eqnarray}
E_{vg} & = & \bar{E}\cdot\hat{v}=(E_{hl}\hat{h_l}+E_{vl}\hat{v_l})\cdot\hat{v} \\
       & = & E_{hl}\sin{\alpha_r}+E_{vl}\cos{\alpha_r} \\
E_{hg} & = & \bar{E}\cdot\hat{h}=(E_{hl}\hat{h_l}+E_{vl}\hat{v_l})\cdot\hat{h} \\
       & = & E_{hl}\cos{\alpha_r}-E_{vl}\sin{\alpha_r} \\
\end{eqnarray}	   
$$ (eq:A15)


where $E_{hg}$ and $E_{vg}$ are the horizontally and vertically polarized electric field
components in the global polarization coordinate,
respectively. Therefore, the emissivity in the global
coordinate system can be now expressed in the matrix form
as

$$
\bar{e}_g=\overline{\overline{R}} \bar{e}_l
$$ (eq:A16)

  
where $\bar{e}_l$ and $\bar{e}_g$ are the Stokes vectors of the ocean surface
emissivity in the local and global coordinate systems, respectively. The rotation matrix $\overline{\overline{R}}$ for relating global
and local coordinate systems is:
 
 $$
 \begin{equation}
\overline{\overline{R}} =
\begin{pmatrix}
\cos^2{\alpha_r} & \sin^2{\alpha_r} & \frac{1}{2}\sin{2\alpha_r} & 0 \\
\sin^2{\alpha_r} & \cos^2{\alpha_r} & -\frac{1}{2}\sin{2\alpha_r} & 0 \\
-\sin{2\alpha_r} & \sin{2\alpha_r} & \cos{2\alpha_r} & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
\end{equation}
$$ (eq:A17)


The above form does not include foam influences for simplicity.


##### Appendix B: TSM Projected Facet Area

The local incidence angle $\theta_l$ for a certain slope tilted by
large-scale ocean waves can be written as the inner product of
local normal vector $\hat{z_l}$ and unit vector $\hat{e}$ for radiometer looking
direction as

$$
\cos{\theta_l}=\hat{z_l}\cdot\hat{e}=\displaystyle\frac{\cos{\theta_i}-s_x\sin{\theta_i}}{\sqrt{s_x^2+s_y^2+1}}
$$ (eq:B1)


The projected facet area $g_0$ is defined as a ratio of the
inclined facet area to the flat surface area as

$$
g_0=\displaystyle\frac{\cos{\theta_l}\cdot\sec{\theta_n}}{\cos{\theta_i}}
$$ (eq:B2)

Substituing [Equation 262](#eq:A6) and [Equation 274](#eq:B1) into  [Equation 276](#eq:B2), the following equation can be obtained:

$$
g_0=\displaystyle\frac{\cos{\theta_l}\cdot\sec{\theta_n}}{\cos{\theta_i}}=\displaystyle\frac{\displaystyle\frac{\cos{\theta_i}-s_x\sin{\theta_i}}{\sqrt{s_x^2+s_y^2+1}}\left(\frac{1}{\sqrt{s_x^2+s_y^2+1}}\right)^{-1}}{\cos{\theta_i}}
$$ (eq:B3)

so that finally

$$
g_0=(1-s_x\tan{\theta_i})
$$ (eq:B4)

 
##### Appendix C: Sea Surface Height spectral model

In the present algorithm, we use the semi-empirical spectrum model developed by V. N. Kudryavtsev and co-authors ({cite:t}`Kudryavtsev1999`,
{cite:t}`Kudryavtsev2003`,{cite:t}`Kudryavtsev2005`; Appendix A in {cite:t}`Yurovskaya2013`). The Kudryavtsev spectrum model proposed in the first papers ({cite:t}`Kudryavtsev1999`, {cite:t}`Kudryavtsev2003`) had a gap in
the gravity‐capillary range (see Fig. 4 in {cite:t}`Kudryavtsev2003`). In 2013, the spectrum was improved using
new experimental data. Data of a full‐scale experiment on stereographic measuring of the two‐dimensional
spectrum with wavelengths from several millimeters to several centimeters are presented in the paper
by {cite:p}`Yurovskaya2013`. Based on the data of this experiment, the Kudryavtsev spectrum was modified.
The measurements were carried out at wind speeds $U_{10}$ from 5 to 15 m/s. The wind fetch in the wave model is taken into account in the same way as in the 
{cite:p}`Elfouhaily1997` model. The long‐wavelength part of the spectrum is 
calculated by the model given in {cite:t}`Donelan1985`. The transition between long‐ and short‐wavelength parts of the spectrum is described by the same exponential factor 
$F_{p}$ which is provided in Appendix C as it was in {cite:p}`Elfouhaily1997`. 
The spatial spectrum of curvatures is presented as:

$$
B(\vec{k},U_{10})=B_{lw}(\vec{k},U_{10},c_p/U_{10})\times F_{p} + B_{sw}(\vec{k},U_{10})\times (1-F_{p})
$$ (eq:C1)


where $B_{lw}$ and $B_{sw}$ are the respective curvature spectrum contributions from low and high wavenumbers and where $c_p/U_{10}$ is the wave age. 

###### Low-wavenumber spectrum

In {cite:t}`Kudryavtsev1999`'s model, the low-wavenumber curvature spectrum $B_{lw}$ follows the
approach suggested by {cite:t}`Elfouhaily1997`:  $B_{lw}$ is assumed to obey {cite:t}`Donelan1985`:

$$B_{lw}=\alpha_p L_{PM} F_p c(k_p)/2c(k_{\rho}') $$ (eq:C2)

 The degree of wind wave development is determined by the inverse wave age $\Omega$
and its dependence on the dimensionless wind fetch is given by

$$
\Omega=0.84\cdot\tanh^{-0.75}{(\displaystyle\frac{\tilde{x}}{x_o})^{0.4}}
$$ (eq:C3)
 
The parameters in [Equation(279)](#equation-eq-c2) are dependent on $U_{10}$, and on $\Omega=U_{10}/c(k_p)$, where $c(k_p)$ is the phase speed at $k_p$, 
the wavenumber at the spectral peak.

$$ 
\begin{matrix}
\alpha_p=0.006 \Omega^{1/2} & k_p=g \Omega^{2}/U^2_{10} \\
c(k_{\rho}')=[g(1+k_{\rho}^{'2}/k_{m}^{2})/k^{'}_{\rho}]^{1/2} \\
\end{matrix}
$$ (eq:C4)

where $g$ is gravitational constant and $k_m$=370 rad.m$^{-1}$. The function $F_p$ is given by:

$$
F_p=\gamma^{\Gamma}\exp{[-\Omega[(k_{\rho}'/k_{\rho})^{1/2}-1]\sqrt{10}]}
$$ (eq:C5)

where

$$
\left\{
\begin{array}\\
    \gamma=1.7 & \mbox{if } \ 0.84 < \Omega\leq 1 \\
   \gamma=1.7+6\mathrm{log}(\omega) & \mbox{if } \ 1 < \Omega\le 5 \\
        \end{array}
\right.
$$ (eq:C6)

and where

$$
\Gamma=\exp{(-[(k_{\rho}'/k_{\rho})^{1/2}-1]^2/2\sigma^2)} \ \ \ \ \sigma=0.08(1+4/\Omega^3)
$$ (eq:C7)


The function $F_p(k/k_p)$ is the high-frequency ‘‘cut-off’’ function, suppressing $B_{lw}$ at wave numbers 
exceeding $10 k_p$ $(k_p$ is wave number of the spectral peak).

The Pierson-Mosckowitz shape spectrum $L_{PM}$ is defined as follows:

$$
L_{PM}=\exp{[-\frac{5}{4}(k_{\rho}'/k_{\rho})^{-2}]}
$$ (eq:C8)

###### High-wavenumber spectrum

The main features of the {cite:t}`Kudryavtsev1999`'s spectral form is a  physical model of the short wind wave
spectrum $B_{sw}(\vec{k},U_{10})$ in the wavelength range from a few millimeters to few meters which take into
account statistical properties of breaking waves and mechanism of generation of capillaries. The spectrum shape 
results from the solution of the energy spectral density balance equation:

$$
Q(B)=\displaystyle\beta_{\nu}(\mathbf{k})B(\mathbf{k})-B(\mathbf{k})(\displaystyle\frac{B(\mathbf{k})}{\alpha})^n+Q_b(\mathbf{k})=0
$$ (eq:C9)

where $Q(B)$  is the total spectral energy source represented as a sum of the effective wind forcing, nonlinear energy
losses (associated with wave breaking in the gravity range, and a nonlinear quadratic limitation of the spectral level in
the capillary-gravity and capillary range), generation of short waves by breaking of longer waves $Q_b$ including
generation of parasitic capillaries. In this equation $B(\mathbf{k})$ is
the saturation spectrum, $\beta_{\nu}(\mathbf{k})=\beta(\mathbf{k})-4\nu k^2 /\omega$ is the
effective growth rate, which is the difference between the
wind growth rate, $\beta(\mathbf{k})$, and the rate of viscous dissipation, $\alpha$ and $n$  are tuning functions of $k/k_{\gamma}$ , which are equal to constants $\alpha=\alpha_g$, $n=n_g$ at $k/k_{\gamma}\lt\lt 1$,  and equal to otherconstants (e.g., n=1) at $k/k_{\gamma}\gt 1$, $k_{\gamma}=\sqrt{g/\gamma}$ -wave
number of minimum phase velocity. Wind wave growth rate is parameterized as suggested
in Kudryavtsev and Makin (2004):

$$
\beta(\vec{k})=c_{\beta}(1+cos^2\phi)u_{\star}(U_k\cos\phi-c)/c^2
$$ (eq:C10)

where $c_{\beta}$ is constant $(c_{\beta}=1.2\cdot 10^{-3})$, $U_{k}$ is mean wind velocity at $z=k^{-1}$. The source term $Q_b=Q_b^{pc}+Q_b^{w}$ describes generation of short surface waves by wave breaking.
Depending on the scale of a breaking wave, two mechanisms are specified. First, due to the effect of the surface
tension, short breaking waves with $k\gt k_b$ (where $k_b$ is of order $k_b~10^2$ rad/m) are not disrupted, but produce ‘‘regular’’
trains of parasitic capillaries (bound waves). These
parasitic capillaries provide energy losses in breaking
waves. Therefore, rate of generation of parasitic capillaries
is equal to the energy dissipation by the carrying short
gravity wave at wave number $K=k_{\gamma}^2$:

$$
Q_{b}^{pc}(\mathbf{k})=\phi(k)[B(B/\alpha)^n]_{k=K}
$$ (eq:C11)

where $\phi(k)$ is a filter function restricting generation of parasitic
capillaries in wave number space. The crests of longer
breaking waves with wave number $k\lt k_b$ disrupt and produce
mechanical perturbations of the sea surface. These
mechanical perturbations generate ‘‘freely’’ propagating
surface waves in all directions as reported by Rozenberg
and Ritter (2005). The rate of their generation is defined by

$$
Q_{b}^{\omega}=c_{b}\omega^{-1}\int\int_{k\lt k_{bm}}\omega\beta(\mathbf{k})B(\mathbf{k}) dlnk d\phi
$$ (eq:C12)

where $c_b$ is an empirical constant (here $c_b=4.5\cdot 10^{-3}$); $k_{bm}=min(k/10,k_b)$ is the upper limit of integration defining interval of breaking waves which generate freely propagating short waves at wave number $k$.

The wave spectrum in the equilibrium range, $B_{sw}$, can be represented as a superposition of the spectrum of freely propagating waves (generated by the wind 
per se and by wave breaking; hereinafter these waves are called as wind waves), $B_{w}$, and the spectrum of parasitic capillaries (bound waves), $B_{pc}$, generated 
on the crests of that freely propagating waves:

$$
B_{sw}(\vec{k})=B_{w}(\vec{k})+B_{pc}(\vec{k})
$$ (eq:C13)

where $B_{w}$ represents the contribution from short gravity and gravity-capillary waves while $B_{cap}$ is the contribution from capillary waves. Either $B_w$ and $B_{pc}$ spectra are difined as solution of the energy balance equation. For the wind wave spectrum the wave breaking source $Q_b$ in the energy source equation is $Q_{b}=Q^w_b$. Solution of such a nonlinear algebraic equation is not straightforward (index of
power has an arbitrary value). However, it can be effectively found by iterations on the basis of its known asymptotic solutions at up-wind, down-wind, and cross-wind directions. Aligned in the wind directions, the wave breaking source $Q^w_b$ is expected to be small in comparison with
direct wind energy input. Then approximate solution in
down-wind direction is

$$
B_w^d=\alpha\beta_{\nu}^{1/n}
$$ (eq:C14)

At cross-wind directions, where the effective wind
input is small or vanishing, $\beta_{\nu}\simeq 0$, the wave spectrum
approximately reads:

$$
B_w^{cr}=\alpha^{n/(n+1)}(Q^w_b)^{1/(n+1)}
$$ (eq:C15)


At the up-wind directions, where $\beta_{\nu}(\mathbf{k})\lt 0$, one
may anticipate the low spectral density, thus nonlinear term
in the energy balance equation can be omitted, and $B_w$ results from the
balance of wave breaking source and energy losses due to
viscosity and interaction with the opposing wind:

$$
B_{w}^{up}\sim-I_{wb}/\beta_{\nu}
$$ (eq:C16)

Combination of these three "asymptotic" solutions,

$$
B_{w}^{0}=\mathrm{max}[B_w^d,\mathrm{min}(B_w^{cr},B_w^{up})]
$$ (eq:C17)

provides a first guess for wave spectrum. Next iteration for
wave spectrum is defined by

$$
B_{w}^{j}=B_{w}^{j-1}-[Q(B_w)/(\partial Q/\partial B_w)]_{B_w=B_w^{j-1}}
$$ (eq:C18)

where $Q(B_w)$ is the total energy source function  for
wind waves (where $Q_b$ is equal to $Q_b=Q_b^w$). Practically,
the iteration scheme $B_{w}^{j}$ with the first-guess solution $B_{w}^{0}$
converges to the exact solution after three iterations.

Spectrum of parasitic capillaries results from the
same energy balance equation where the parasitic capillary
term, $Q_b=Q_b^{pc}$, is the only energy source (wind forcing
is omitted) which is balanced by viscous and nonlinear
dissipation. Solution of this equation is straightforward and
reads:

$$
B_{pc}(\mathbf{k})=\displaystyle\frac{\alpha}{2}[-4\nu k^2/\omega+\sqrt{(4\nu k^2/\omega)^2+4Q_{b}^{pc}(\mathbf{k})/\alpha}]
$$ (eq:C19)

with $Q_b^{pc}$  defined by relation above for the wind wave spectrum $B_w$. Now to complete the model we need to define the model functions, $n(k/k_{\gamma})$ and $\alpha(k/k_{\gamma})$, following kudryavtsev et al. (2003). The key function $n(k/k_{\gamma})$ is related to the wind exponent $m=2/n(k/k_{\gamma})$. Its shape is specified as:

$$
\displaystyle\frac{1}{n(k/k_{\gamma})}=(1-\frac{1}{n_g})f(k/k_{\gamma})+\frac{1}{n_g}
$$ (eqC20)

where $f(k/k_{\gamma})$ is a tuning function satisfying conditions, $f\rightarrow 0$ at $k/k_{\gamma}\lt\lt1$, and $f\rightarrow 1$ at $k/k_{\gamma}\sim 1$. 
In order to fit wind exponent determined from observations, the tuning function $f(k/k_{\gamma})$ is defined as:

$$
f(k/k_{\gamma})=[1+\mathrm{tanh}(2(\zeta-\zeta_b))]/2
$$ (eq:C21)

where $\zeta=\mathrm{ln}k$, $\zeta_b=\mathrm{ln}k_b$ and $k_b$ is a wave number corresponding
to center of transition interval which is fixed at $k_b=1/4k_{\gamma}$. Parameter $n_g$ is fixed at $n_g=10$ in order to be consistent with Banner et al. [1989] data. As argued in Kudryavtsev et al. [2003], the other
tuning function, $\alpha(k/k_{\gamma})$, is linked to $n(k/k_{\gamma})$ as 

$$
\mathrm{ln}[\alpha(k/k_{\gamma})]=\mathrm{ln} a -\mathrm{ln} (\bar{C_{\beta}})/n(k/k_{\gamma})
$$ (eq:C22)

with $\bar{C_{\beta}}=0.03$ as a mean value of the growth rate, and $a$ is
a constant which is chosen as $a=2\cdot 10^{-3}$ in order to get
right value of the sea surface MSS.

The filter function  $\phi(k)$ restricting action of the parasitic
capillaries source is refined to be consistent with
spectral behavior of f-function defining nonlinear
dissipation. The ‘‘high-frequency’’ cut-off of
$\phi(k)$ must be linked to the transition wave number, $k_b$, as
$k_{pc}^{h}=k_{\gamma}^2/k_b$, while the low-frequency cut-off of 
$\phi(k)$ to be $k_{pc}^{l}=nk_{\gamma}$ where $n$ should be about $n=2$, but the authors fixed it at
n=3/2 in order to get location of the parasitic capillaries peak as observed in the experiments. Thus the filter function is

$$
\phi(k)=f(k/k_{pc}^{l})-f(k/k_{pc}^{h})
$$ (eq:C23)

Following the model construction, the wind exponent
parameter $m=2/n(k/k_{\gamma})$, also imposes the angular
distribution, as $B(\cos(\phi)^{2/n}$.  The measured
spectrum is a folded spectrum, $B_j(\mathbf{k})$, that relates to the directional spectrum as 

$$
B_j(\mathbf{k})=[B(\mathbf{k})+B(-\mathbf{k})]/2
$$ (eqC24)

The angular modulation parameter, $\Delta$, of the folded
spectra can be expressed through the directional spectrum
in up-wind, down-wind, and cross-wind directions (spectra
correspondingly) as:

$$
\Delta(k)=\displaystyle\frac{\pi}{2}\frac{B_{up}+B_{d}-2B_{cr}}{B_o}
$$ (eq:C25)

where $B_o$ is omnidirectional (integrated over all directions)
spectrum.

##### Appendix D: SPM  polarimetric bistatic coefficients

The functions $g_i$ (i = v, h, U, and V ) are the second-order weighting functions from
the small perturbation method ({cite:t}`yueh1994`), and the detailed information
and formulation of $g_i$ are provided as:


$$
\begin{matrix}
g_h(f,\theta_l,\phi_l,\epsilon_{sw},k_{\rho},\phi)=2\Re{\left[R_{hh}^{(0)\star}f_{hh}^{(2)}\right]} +\displaystyle\frac{k_{zi}}{k_z}\left[|f_{hh}^{(1)}|^2+|f_{hv}^{(1)}|^2\right]F\\
g_v(f,\theta_l,\phi_l,\epsilon_{sw},k_{\rho},\phi)=2\Re{\left[R_{vv}^{(0)\star}f_{vv}^{(2)}\right]}+\displaystyle\frac{k_{zi}}{k_z}\left[|f_{vv}^{(1)}|^2+|f_{vh}^{(1)}|^2\right]F\\
g_U(f,\theta_l,\phi_l,\epsilon_{sw},k_{\rho},\phi)=2\Re{\left[(R_{hh}^{(0)\star}-R_{vv}^{(0)\star})f_{hv}^{(2)}\right]}+\displaystyle\frac{2k_{zi}}{k_z}\Re{\left[f_{vh}^{(1)}f_{hh}^{(1)\star}+f_{vv}^{(1)}f_{hv}^{(1)\star}\right]}F\\
g_V(f,\theta_l,\phi_l,\epsilon_{sw},k_{\rho},\phi)=2\Im{\left[(R_{hh}^{(0)\star}+R_{vv}^{(0)\star})f_{hv}^{(2)}\right]}+\displaystyle\frac{2k_{zi}}{k_z}\Im{\left[f_{vh}^{(1)}f_{hh}^{(1)\star}+f_{vv}^{(1)}f_{hv}^{(1)\star}\right]}F\\
\end{matrix}
$$ (eq:D1)

In the above equations, $\Re$ and $\Im$ represent the real
and imaginary part operators respectively, $k_o=2\pi/\lambda_o$ is
the electromagnetic wavenumber, $k_{zl}=k_o\cos(\theta_l)$, and
$f_{\alpha\beta}^{(1)}$ and $f_{\alpha\beta}^{(2)}$ are the first
and second order SPM scattering coefficients. The function $F$ is a function
defined as $F$=1 if $\Im{(k_z)}=0$ and $F$=0 if $\Im{(k_z)}\neq 0$. 

The first terms in the above $g_{\gamma}$ expressions represent the second order
coherent reflection coefficient contributions, while the second
terms represent the incoherent Bragg scatter contributions.

Second order scattering coefficients are exactly those taken from {cite:t}`yueh1994`, with the variables $k_x$, $k_y$, and $k_z$
given by 

$$
\begin{matrix}
k_x = k_{xl} +k_{\rho'}\cos{\phi'} \\
k_y = k_{yl} +k_{\rho'}\sin{\phi'} \\
k_z = \sqrt{k_o^2-k_{x}^2-k_y^2} \\
\end{matrix}
$$ (eq:D2)

where $k_{xl} = k_o\sin{\theta_l}\cos{\phi_l}$ and $k_{yi} =k_o\sin{\theta_l}\sin{\phi_l}$. Note that $f_{hv}^{(2)}$
 is used with $g_U$ and $g_V$ as opposed to $f_{vh}^{(2)}$ in {cite:t}`yueh1994` due to
an evaluation of the second order reflection coefficient in
scattered field coordinates.
First order coefficients are as given in {cite:t}`yueh1994`, except that the incident $(k_{xl},k_{yl}, k_{zl})$ and scattered $(k_x, k_y, k_z)$ variables
are first interchanged and then the above equations used to
represent $k_x$, etc. in terms of $k_{xl}$ , $k_{\rho'}$ and
$\phi'$. This set of new projected coefficients are rewritten herebelow for completeness.



###### First-Order Scattering coefficients

The coefficients for the incoherent bistatic scattering
coefficients due to the first-order scattered fields are defined
as:

$$
\Gamma_{\alpha\beta\mu\nu}(k_x,k_y,k_xi,k_yi)
=f_{\alpha\beta}^{(1)}(\theta,\phi;\theta_i,\phi_i)f_{\mu\nu}^{(1)\star}(\theta,\phi;\theta_i,\phi_i)
$$ (eq:D3)

with

$$
\begin{matrix}
f_{hh}^{(1)}(\theta,\phi;\theta_i,\phi_i)=\displaystyle\frac{2k_{zi}(k_1^2-k_o^2)}{k_z+k_{1z}}\displaystyle\frac{1}{k_{zi}+k_{1zi}}\\
\\
\cdot\left(\displaystyle\frac{k_{xi}}{k_{\rho_i}}\displaystyle\frac{k_x}{k_{\rho}}+\displaystyle\frac{k_{yi}}{k_{\rho_i}}\displaystyle\frac{k_y}{k_{\rho}}\right)
\end{matrix}
$$ (eq:D4)

$$
\begin{matrix}
f_{hv}^{(1)}(\theta,\phi;\theta_i,\phi_i)=\displaystyle\frac{2k_{zi}(k_1^2-k_o^2)}{k_z+k_{1z}}\displaystyle\frac{k_{1zi}k_o}{k_1^2k_{zi}+k_o^2k_{1zi}}\\
\\
\cdot\left(-\displaystyle\frac{k_{yi}}{k_{\rho_i}}\displaystyle\frac{k_x}{k_{\rho}}+\displaystyle\frac{k_{xi}}{k_{\rho_i}}\displaystyle\frac{k_y}{k_{\rho}}\right)
\end{matrix}
$$ (eq:D5)

$$
\begin{matrix}
f_{vh}^{(1)}(\theta,\phi;\theta_i,\phi_i)=\displaystyle\frac{2k_{zi}(k_1^2-k_o^2)}{k_1^2k_z+k_o^2k_{1z}}\displaystyle\frac{k_{1z}k_o}{k_{zi}+k_{1zi}}\\
\\
\cdot\left(-\displaystyle\frac{k_{yi}}{k_{\rho_i}}\displaystyle\frac{k_x}{k_{\rho}}+\displaystyle\frac{k_{xi}}{k_{\rho_i}}\displaystyle\frac{k_y}{k_{\rho}}\right)
\end{matrix}
$$ (eq:D6)

$$
\begin{matrix}
f_{vh}^{(1)}(\theta,\phi;\theta_i,\phi_i)=\displaystyle\frac{2k_{zi}(k_1^2-k_o^2)}{k_1^2k_z+k_o^2k_{1z}}\displaystyle\frac{1}{k_1^2k_{zi}+k_o^2k_{1zi}}\\
\\
\cdot\left[k_1^2k_{\rho}k_{\rho_i}-k_o^2k_{1z}k_{1zi}\left(\displaystyle\frac{k_{xi}}{k_{\rho_i}}\displaystyle\frac{k_x}{k_{\rho}}+\displaystyle\frac{k_{yi}}{k_{\rho_i}}\displaystyle\frac{k_y}{k_{\rho}}\right)\right]
\end{matrix}
$$ (eq:D7)

where

$$
\begin{matrix}
k_{\rho_i}=k_o\sin(\theta_i) \\
\\
k_{xi}=k_{\rho_i}\cos(\phi_i) \\
\\
k_{yi}=k_{\rho_i}\sin(\phi_i) \\
\\
k_{zi}=\sqrt{k_o^2-k_{\rho_i}^2} \\
\\
k_{1zi}=\sqrt{k_1^2-k_{\rho_i}^2} \\
\\
k_{\rho}=k_o\sin(\theta) \\
\\
k_{x}=k_{\rho}\cos(\phi) \\
\\
k_{y}=k_{\rho}\sin(\phi) \\
\\
k_{z}=\sqrt{k_o^2-k_{\rho}^2} \\
\\
k_{1z}=\sqrt{k_1^2-k_{\rho}^2} \\
\end{matrix}
$$  (eq:D8)

and $k_o$ and $k_1$ are the electromagnetic wavenumbers of the
free space and lower half space. In sea water with complex
dielectric constant $\epsilon_{sw}$, the electromagnetic
wavenumber is complex and given by:
$k_1=k_o\sqrt{\epsilon_{sw}}$

###### Second-Order Scattering coefficients

With  $k_{xi}$, $k_{yi}$, $k_{\rho i}$, $k_{zi}$, $k_{1zi}$,
$k_{\rho}$, $k_z$, and $k_{1z}$ as given just above, the second order bistactic scattering
coefficients are:


$$
\begin{array}{l}
f^{(2)}_{hh}=\displaystyle\frac{k_1^2-k_o^2}{k_{zi}+k_{1zi}}\displaystyle\frac{2k_{zi}}{k_{zi}+k_{1zi}}\\
\\
\times\left\{k_{1zi}+
\displaystyle\frac{(k_o^2-k_1^2)}{(k^2_{\rho}+k_{1z}k_z)(k_z+k_{1z})}
\cdot\left[k_{1z}k_z+k^2_{\rho}\left(\displaystyle\frac{k_{xi}}{k_{\rho
i}}\displaystyle\frac{k_x}{k_{\rho}}+\displaystyle\frac{k_{yi}}{k_{\rho
i}}\displaystyle\frac{k_y}{k_{\rho}}\right)^2\right]\right\}\\
\end{array}
$$ (eq:D9)

$$
\begin{array}{l}
f^{(2)}_{vh}=\displaystyle\frac{k_1^2-k_o^2}{k_{zi}+k_{1zi}}\displaystyle\frac{2k_ok_{zi}}{k_1^2k_{zi}+k_o^2k_{1zi}}\left(\displaystyle\frac{k_{xi}}{k_{\rho
i}}\displaystyle\frac{k_y}{k_{\rho}}-\displaystyle\frac{k_{yi}}{k_{\rho
i}}\displaystyle\frac{k_x}{k_{\rho}}\right) \\
\\
\times\left[\displaystyle\frac{k_{\rho}k_{\rho
i}k_1^2}{k^2_{\rho}+k_{1z}k_z}+\displaystyle\frac{k_{1zi}k^2_{\rho}(k_o^2-k_1^2)}{(k^2_{\rho}+k_{1z}k_z)(k_z+k_{1z})}\left(\displaystyle\frac{k_{xi}}{k_{\rho
i}}\displaystyle\frac{k_x}{k_{\rho}}+\displaystyle\frac{k_{yi}}{k_{\rho
i}}\displaystyle\frac{k_y}{k_{\rho}}\right)\right]
\end{array}
$$ (eq:D10)

$$
f^{(2)}_{hv}=-f^{(2)}_{vh}
$$ (eq:D11)

$$
\begin{array}{l}
f^{(2)}_{vv}=\displaystyle\frac{k_o^2-k_1^2}{k_1^2k_{zi}+k_o^2k_{1zi}}\displaystyle\frac{2k_{zi}k_1^2k_o^2}{k_1^2k_{zi}+k_o^2k_{1zi}}
\cdot\left\{\displaystyle\frac{k_{\rho
i}^2k_{\rho}^2(k_1^2-k_o^2)}{k_o^2(k^2_{\rho}+k_{1z}k_z)(k_z+k_{1z})}\right.\\
\\
+k_{1zi}\left[1-\displaystyle\frac{2k_{\rho
i}k_{\rho}}{k_{1z}k_z+k^2_{\rho}}\left(\displaystyle\frac{k_{xi}}{k_{\rho
i}}\displaystyle\frac{k_x}{k_{\rho}}+\displaystyle\frac{k_{yi}}{k_{\rho
i}}\displaystyle\frac{k_y}{k_{\rho}}\right)\right]
\\
+\displaystyle\frac{k_{1zi}^2(k_o^2-k_1^2)}{k_1^2(k_z+k_{1z})}
\cdot\left.
\left[1-\displaystyle\frac{k_{\rho}^2}{k_{1z}k_z+k^2_{\rho}}\left(\displaystyle\frac{k_{xi}}{k_{\rho
i}}\displaystyle\frac{k_x}{k_{\rho}}+\displaystyle\frac{k_{yi}}{k_{\rho
i}}\displaystyle\frac{k_y}{k_{\rho}}\right)^2\right]\right\}\\
\end{array}
$$ (eq:D12)

###### SSA/SPM azimuthal harmonic expansion

According to ({cite:t}`yueh1997`, 1997; {cite:t}`Johnson2006`), the emissivity perturbation $\Delta \bar{e_{ss}}$ in
the local polarization coordinate system with local incidence angle $\theta_l$ and local azimuthal angle $\phi_l$  can be determined as:

$$
\Delta \overline{e}_{ss}=
\left[\begin{matrix}
\Delta \overline{e}_{ssh} \\ 
\Delta \overline{e}_{ssv} \\
\Delta \overline{e}_{ssU} \\
\Delta \overline{e}_{ssV} \\
\end{matrix}\right]=
\displaystyle\int_{k_{l}}^{k_{u}}\int_o^{2\pi}k_{\rho}'{W(k_{\rho}',\phi'+\phi_l)} 
\left[
\begin{matrix}
g_h(f,\theta_l,\phi_l,\epsilon_{sw},k_{\rho}',\phi')\\
g_v(f,\theta_l,\phi_l,\epsilon_{sw},k_{\rho}',\phi')\\
g_U(f,\theta_l,\phi_l,\epsilon_{sw},k_{\rho}',\phi')\\
g_V(f,\theta_l,\phi_l,\epsilon_{sw},k_{\rho}',\phi')\\
\end{matrix}
\right]
k_{\rho}'dk_{\rho}'d\phi'
$$  (eq:D13)

The integral in the equation  for emissivities is over all length
 scales of the ocean spectrum $(k_{\rho'}$  from $k_l$ to $k_u$; however, the integration
 of incoherent scattering coefficients should
be limited to only those length scales which produced a
propagating Bragg scattered wave. The function $\it{F}$ in the second
terms of the scattering coefficients equations indicates this fact: $\it{F}$ is
defined to be 1 for $k_z$ real, 0 for $k_z$ complex, and limits
the incoherent contributions to waves propagating in the upper
hemisphere. Evaluation of the rough surface emission is
performed through numerical integration of the double integral for
fixed values of all the radiometer and surface parameters $(f,\theta_l, \phi_l, \epsilon_{sw}, \rm{and}, W(k_{\rho'},\phi'))$.
Typical formulations based on {cite:t}`yueh1994` would
numerically calculate double integrals for the coherent and
incoherent contributions separately; the above expression combines
both into one integration. This is important because the coherent
and incoherent terms when calculated separately for large height
surfaces can both obtain very large values which cancel when
combined to yield the total emission prediction. This cancellation
effect results in extremely high accuracy required in the separate
numerical integrations. Combining the two into one double integral
eliminates this problem.

Since evaluation of the integral emission equation results in an emissivity
vector for one value of $\phi_i$ only, studies of
brightness temperature azimuthal harmonics require repeated
evaluation of the double integral for varying values of  $\phi_i$
to produce functions of azimuth. Harmonic coefficients can then be
extracted from these functions through a Fourier transform.
Calculation of emissivities azimuthal harmonic
coefficients and their variations with other surface and
radiometer parameters can therefore be quite time consuming.


{cite:t}`john:zhang99` proposed to use several properties of the original $g_{\gamma}$
functions and of the surface directional spectrum and this approach is
applied in the present algorithm. First, it is widely accepted
that the ocean surface spectrum should vary as $1/k_{\rho}^{'4}$
for large values of $k_{\rho}'$; it is advantageous to remove this
dependency through use of the ocean curvature spectrum,
$C(k_{\rho}',\phi')$, defined as
$k_{\rho}^{'4}W(k_{\rho}',\phi')$. Next it is noted that the
$g_{\gamma}$ functions have a $k_o^2$ dependence on frequency
which can be factored out by defining: 

$$
\tilde{g_{\gamma}}(\theta_l,\phi_l,\epsilon_{sw},k_{\rho}'/k_o,\phi')=\frac{1}{k_o^2}g_{\gamma}(f,\theta_l,\phi_l,\epsilon_{sw},k_{\rho}',\phi')
$$ (eq:D14)

with the resulting $\tilde{g_{\gamma}}$ functions depending on
frequency only through: $\displaystyle\frac{k_{\rho}'}{k_o}$. Using these ideas and re-writing the second order change in emissivity from a flat surface $(\Delta e_{\gamma})$ in terms of
$\beta=k_{\rho}'/k_o$ yields

$$
\left[
\begin{matrix}
\Delta \bar{e_{ssh}} \\
\Delta \bar{e_{ssv}} \\
\Delta \bar{e_{ssU}}\\
\Delta \bar{e_{ssV}}\\
\end{matrix}
\right]=- \left(
\displaystyle\int_{k_l}^{k_u}\displaystyle\int_o^{2\pi}B(k_o\beta,\phi')
\left[
\begin{matrix}
g_h'(\theta_l,\phi_l;\epsilon_{sw},\beta,\phi')\\
g_v'(\theta_l,\phi_l;\epsilon_{sw},\beta,\phi')\\
g_U'(\theta_l,\phi_l;\epsilon_{sw},\beta,\phi')\\
g_V'(\theta_l,\phi_l;\epsilon_{sw},\beta,\phi')\\
\end{matrix}
\right]d\phi'd\beta \right)
$$ (eq:D15)

where

$$
\begin{array}{ll}
g_{\gamma}'(\theta_l,\phi_l;\epsilon_{sw},\beta,\phi')d\beta&=\displaystyle\frac{1}{\beta^3}\tilde{g_{\gamma}}
\left(\theta_l,\phi_l;\epsilon_{sw},\displaystyle\frac{k_{\rho}'}{k_o}=\beta,\phi'\right)d\beta \\
&\hspace{-.5cm}=\displaystyle\frac{k_o^2}{k_{\rho}^{'3}}\left[\displaystyle\frac{1}{k_o^2}g_{\gamma}\left(f,\theta_l,\phi_l;\epsilon_{sw},k_{\rho}'=k_o\beta,\phi'\right)\right]dk_{\rho}'
\end{array}
$$ (eq:D16)

and the new $g_{\gamma}'$ functions have no explicit dependence on frequency.

A second simplification is used to separate individual azimuth
harmonics of the emission vector. First a study of the
$g_{\gamma}'$ functions reveals them to be functions of
$\phi_l-\phi'$  alone and not $\phi_l$ and $\phi'$ separately.
This motivates expansion of the $g_{\gamma}'$  functions in a
Fourier series as:

$$
g_{\gamma}'(\theta_l,\phi_l;\epsilon_{sw},\beta,\phi')=\displaystyle\sum_{n=-\infty}^{\infty}e^{in(\phi_l-\phi')}g_{\gamma,n}'(\theta_l,\epsilon_{sw},\beta)
$$ (eq:D17)

Consideration of the fact that the $g_{\gamma}'$  functions are
real functions and the symmetric properties in $\phi_l-\phi'$ for
each polarimetric quantity reveals that $g_h'$ and $g_v'$ should
have only real valued $g_{\gamma,n}'$ which are even in $\it{n}$, while
$g_U'$ and $g_V'$ should have only imaginary valued
$g_{\gamma,n}'$ which are odd in $\it{n}$. Using the Fourier
expansion in the emssion equation results in:

$$
\begin{array}{l}
\Delta e_{ss\gamma}=-\left(\left[
\begin{array}{c}
\displaystyle\int_o^{\infty} g_{\gamma,0}'B_0(k_o\beta) d\beta\\
0 \\
\end{array}\right]+ \right.
\left. \displaystyle\sum_{n=1}^{\infty} \left[
\begin{array}{c}
2\cos(n\phi_l)\int_o^{\infty} \Re{\left[g_{\gamma,n}'(\beta)\right]}B_n(k_o\beta) d\beta\\
-2\sin(n\phi_l)\int_o^{\infty} \Im{\left[g_{\gamma,n}'(\beta)\right]}B_n(k_o\beta) d\beta\\
\end{array}\right]\right)\\
\end{array}
$$ (eq:D18)

where the upper row in the final equality holds for $h$ and $v$,
the lower row for $U$ and $V$, and an assumption that the
curvature spectrum contains only cosine harmonics has been made.

The last Equation has separated out individual
emission azimuthal harmonic terms (the $\cos(n\phi_l)$ and
$\sin(n\phi_l)$ terms) and reveals them to be proportional to an
integral of a weighting function $g_{\gamma,n}'(\beta)$ times the
$B_n(k_o\beta)$ functions. Note that:

$$
B_n(k_o\beta)=\displaystyle\int_o^{2\pi}e^{-in
\phi'}C(k\beta,\phi')d\phi'
$$(eq:D19)

represents the $n$ th harmonic of the surface curvature spectrum,
so the above equation demonstrates the direct correspondence
between emission and surface azimuthal harmonics. Again, since the
properties of a surface directional spectrum require it to have no
odd azimuthal harmonics, the above equation clarifies that no odd
emission harmonics will be obtained in this second order
formulation. In addition, the above equation makes calculation of
emission harmonics a much more direct procedure, since two single
integrals (one for the Fourier series expansion of the
$g_{\gamma}'$ functions and the $d\beta$ integration in this last equation
replace the multiple double integrals in an
azimuth sweep procedure.

Given a sea surface roughness curvature spectrum model and the ssa/spm $g_{\gamma,n}'$ fonctions, the sea surface roughness-induced Stokes vector emission can be estimated following:

$$
\left[
\begin{matrix}
\Delta \bar{e_{ssh}} \\ 
\Delta \bar{e_{rv}} \\
\Delta \bar{e_{rU}} \\
\Delta \bar{e_{rV}} 
\end{matrix}
\right]=
\left[
\begin{matrix}
e_{ssh}^{(0)}+e_{ssh}^{(2)}\cos(2\phi_{wr}) \\ 
e_{ssv}^{(0)}+e_{ssv}^{(2)}\cos(2\phi_{wr}) \\
e_{ssU}^{(2)}\sin(2\phi_{wr}) \\
e_{ssV}^{(2)}\sin(2\phi_{wr}) 
\end{matrix}
\right]
$$ (eq:D20)

where the  terms $e_{r\gamma}^{(n)}$ represent the nth azimuthal harmonics of the wind-excess emissivity. Note that due to the assumption of gaussianity in the sea surface statistics, 
the solution can be expressed strictly in terms of a roughness spectrum. Properties of a directional spectrum result in no first azimuthal harmonic variations being obtained; introduction of non-gaussianity 
is required to obtain first azimuthal harmonics.


##### Appendix E: Efficient Implementation of Bistatic Scattering Coefficients

 In this appendix we review the method we used for bistatic scattering
coefficients efficient numerical implementation. First, the Kirchhoff Integral (\ref{eq:KI0}) is analytically expressed in polar coordinates so that  the
bistatic scattering coefficients dependency on wind direction is separated from the dependencies on other
variables, without introducing further approximations. This allows
calculation of bistatic scattering coefficients using a single
numerical integration over the radial distance instead of two as with
a cartesian coordinate formulation, saving a large amount of processing time.

Second, we make use of pre-calculated lookup tables (LUTS) of the bistatic scattering harmonic coefficients to provide a fast computing
method to estimate rough sea surface bistatic coefficients using
either the small slope or Kirchhoff asymptotic theory.

###### Kirchhoff Integral in polar coordinates

The Kirchoff Integral  can be written as follows in polar coordinates

$$
\begin{equation}
I_K=\displaystyle\int_{0}^{\infty}\displaystyle\int_{0}^{2\pi}
\left\{e^{\displaystyle\left[(q_s+q_o)^2\rho(\vec{r})\right]}-1\right\}
e^{\displaystyle-i(\vec{k}_{s}-\vec{k}_{o}) \cdot \vec{r}}d\vec{r}
\label{eq:A1}
\end{equation}
$$

where $(\vec{k}_{s},\vec{k}_{o})$ are the scattered
and incident wavenumber vectors, $(q_s,q_o) = (\hat{z}\cdot
\vec{k}_{s}, -\hat{z}\cdot \vec{k}_{o})$ represent the vertical
projection of the wavevectors with $\hat{z}$ a unit vector normal to the surface at target and directed upward, $(r,\Phi)=\vec{r}$ are the polar coordinates and $\rho(\vec{r})$ is the
sea surface
elevation autocorrelation function. The Fourier series expansion of $\rho(\vec{r})$ in azimuthal harmonics up to the second order can be expressed as follows ({cite:t}`berginc02`,
 {cite:t}`Elfouhaily1997`):

$$
\begin{equation}
\rho(r,\Phi)=\rho_0(r)-\rho_2(r)\cos 2(\Phi-\varphi_w),
\label{eq:A2}
\end{equation}
$$
where $\varphi_w$ is the wind direction, and where the isotropic $\rho_0(r)$ and anisotropic $\rho_2(r)$ parts are given by:

$$
\begin{equation}
\left\{
\begin{array}{l}
\rho_0(r)=\displaystyle\int_0^{\infty}W(k)J_0(rk)dk \\
\\
\rho_2(r)=\displaystyle\int_0^{\infty} W(k)\Delta(k)J_2(rk)dk \\
\end{array}
\right.\label{eq:A3}
\end{equation}
$$

with $k$ is a surface wavenumber, $J_n$ is the $n^{th}$ order Bessel function of the first kind, $W(k)$ is the omni-directional sea surface height
spectrum at wavenumber $k$, and $\Delta(k)$ is the so-called upwind-crosswind ratio at wavenumber $k$ ({cite:t}`Elfouhaily1997`).

In the following, we denote $\vec{q}=\vec{k}_{s} - \vec{k}_{o} = q_{Hx}\hat{x} + q_{Hy}\hat{y}+ q_{z}\hat{z}$, the vector
defined as the difference between the scattered and incident
electromagnetic wavenumbers. We further denote $q_H = |q_{Hx}\hat{x}+ q_{Hy}\hat{y}|$, the horizontal component of $\vec{q}$ and introduce the angle $\Phi_{si} =
\tan^{-1}\left(q_{Hy}/q_{Hx}\right)$.

Substituting (\ref{eq:A2})  into (\ref{eq:A1}), the Kirchhoff Integral in polar coordinates becomes

$$
\begin{equation}
I_K=\displaystyle\int_0^\infty\displaystyle\int_0^{2\pi}\left\{e^{\displaystyle\left[q_z^2
\rho_0(r)-q_z^2\rho_2(r)\cos2(\Phi-\varphi_w)\right]}-1\right\}
\displaystyle e^{\displaystyle-iq_H r \cos(\Phi-\Phi_{si})}rd\Phi dr,
\label{eq:A4}
\end{equation}
$$

Gathering the terms functions of $\Phi$ in (\ref{eq:A4}),

$$
\begin{equation}
I_K=2\pi\displaystyle\int_0^{\infty}\left[I_{\Phi_1}(r)
e^{\displaystyle q_z^2\rho_0(r)}-I_{\Phi_2}(r)\right]rdr.
\label{eq:A5}
\end{equation}
$$

where we need to solve the following integrals over $\Phi$:

$$
\begin{equation}
\left\{
\begin{array}{l}
I_{\Phi_1}(r)=\displaystyle\frac{1}{2\pi}\displaystyle\int_0^{2\pi}e^{-a(r)\cos2(\Phi-\varphi_w)-ib(r)
\cos(\Phi-\Phi_{si})}d\Phi, \\ \\
I_{\Phi_2}(r)=\displaystyle\frac{1}{2\pi}\displaystyle\int_0^{2\pi}e^{-ib(r)
\cos(\Phi-\Phi_{si})}d\Phi, \\
\end{array}\right.
\label{eq:A6}
\end{equation}
$$

where $a(r)=q_z^2\rho_2(r)$ and $b(r)=q_H r$. The complex exponential in (\ref{eq:A6}) can be expressed in terms of
Bessel functions using the Jacobi-Anger expansion (see,
e.g.,{cite:t}`2005mmp`):

$$
\begin{equation}
e^{-a\cos 2(\Phi-\varphi_w)}=\displaystyle\sum_{m=-\infty}^{\infty}i^m
J_m(ia)e^{2im(\Phi-\varphi_w)}=J_0(ia)+2\displaystyle\sum_{m=1}^{\infty}i^m
J_m(ia)\cos{2m(\Phi-\varphi_w)},
\label{eq:A7}
\end{equation}
$$

and

$$
\begin{equation}
e^{-ib \cos(\Phi-\Phi_{si})}=\displaystyle\sum_{n=-\infty}^{\infty}i^n
J_n(b)e^{in(\Phi-\Phi_{si}+\pi)}=J_0(b)+2\displaystyle\sum_{n=1}^{\infty}i^{3n}
J_n(b)\cos{n(\Phi-\Phi_{si})},
\label{eq:A8}
\end{equation}
$$
where $J_m$ is the Bessel function of the first kind and order
$m$. Substituting (\ref{eq:A7}) and (\ref{eq:A8}) into
(\ref{eq:A6}) and performing the integration over $\Phi$, we obtain

$$
\begin{equation}
I_{\Phi_1}(r)=J_0(ia)J_0(b)
+2\displaystyle\sum_{m=1}^{\infty}\displaystyle\sum_{n=1}^
{\infty}\delta(2m-n)J_m(ia)J_n(b)i^{m+3n}\cos{n(\Phi_{si}-\varphi_w)}
\end{equation}
$$

where $\delta$ is the Kronecker Delta. Using the relation
$J_m(ia)=i^mI_m(a)$ where $I_m$ denotes the modified Bessel function
of the first kind and order $m$, we obtain:

$$
\begin{equation}
I_{\Phi_1}(r) = I_0(a)J_0(b)+2\displaystyle\sum_{m=1}^{\infty}I_m(a)J_{2m}(b)\cos{2m(\Phi_{si}-\varphi_w)}.\\
\label{eq:A9}\end{equation}
$$

For $I_{\Phi_2}(r)$, we have
$$
\begin{equation}
I_{\Phi_2}(r) =  \frac{1}{2\pi}\displaystyle\int_0^{2\pi} \left[J_0(b)+2\displaystyle\sum_{n=1}^{\infty}i^n J_n(b)\cos{n(\Phi-\Phi_{si})}\right]d\Phi = J_0(b).
\label{eq:A10}\end{equation}
$$
Substituting (\ref{eq:A9}) and (\ref{eq:A10}) into
(\ref{eq:A5}),the Kirchhoff Integral reads

$$
\begin{equation}
I_K = \displaystyle\sum_{m=0}^{\infty} I_K^{m} \cos{2m(\Phi_{si}-\varphi_w)}.
\label{eq:A11}\end{equation}
$$

where

$$
\begin{equation}
I_K^0 = 2\pi\displaystyle\int_0^\infty J_0(q_H r)\left[I_0(q_z^2\rho_2(r))e^{q_z^2\rho_0(r)}-1\right]r dr
\end{equation}
$$

and where, for $m$ from $1$ to $\infty$,

$$
\begin{equation}
I_K^m = 4\pi \displaystyle\int_0^\infty I_m(q_z^2\rho_2(r))J_{2m}(q_H r) e^{q_z^2\rho_0(r)} r dr,
\end{equation}
$$

is the coefficient of harmonic $2m$ in a cosine series decomposition
of the Kirchhoff Integral.

The formulation (\ref{eq:A11}) for the Kirchhoff Integral allows
calculation of bistatic scattering coefficients using a single
numerical integration over the radial distance instead of four as with
a cartesian coordinate formulation.


###### Kirchhoff Approximation kernel functions

The kernel functions $K_{\alpha\alpha_\circ}(\bf{k_{s}},\bf{k_o})$
are functions of both the scattering geometry and the dielectric
constant. Explicit expressions of these kernel functions for the Kirchhoff Approximation (KA)  may be found in
 {cite:t}`Voronovich2001`. We provide theuir definition herebeow:
 
If $\lambda_o=c/f$ is the EM wavelength with *c*, the speed of light [m/s], then $K_o=2\cdot\pi/\lambda_o$  is the amplitude of the incident EM wavenumber
 and we let $K=K_o$. 
 Let $ko_x$, $ko_y$, and  $ko_z$ be the horizontal projection on x-axis, on y-axis, and the vertical component of the incide wavevector, respectively:

$$
\begin{equation}
\begin{array}{l}
ko_x=-K_o\sin{(\theta_o)}\cdot\cos{(\phi_o)} \\
ko_y=-K_o\sin{(\theta_o)}\cdot\sin{(\phi_o)} \\
ko_z=-q_o=-K_o\cos{(\theta_o)}
\end{array}
\end{equation}
$$


Let also $k_x$, $k_y$, and  $k_z$ be the horizontal projection on x-axis, on y-axis, and the vertical component of the  scattered wavenumber:

$$
\begin{equation}
\begin{array}{l}
k_x=K_o\sin{(\theta_s)}\cdot\cos{(\phi_s)} \\
k_y=K_o\sin{(\theta_s)}\cdot\sin{(\phi_s)} \\
k_z=q_k=K_o\cos{(\theta_s)}
\end{array}
\end{equation}
$$

We also define the vector $\bf{q}$, the difference between the scattered and incident wavenumber:

$$
\begin{equation}
\begin{array}{l}
q_{Hx}=k_x-ko_x \\
q_{Hy}=k_y-ko_y \\
q_z=k_z-ko_z 
\end{array}
\end{equation}
$$

and:

$$q_H=\sqrt{(q^2_{Hx}+q^2_{Hy})}$$

so that:

$$
q^2=q_{Hx}\cdot q_{Hx}+q_{Hy}\cdot q_{Hy}+q_z\cdot q_z
$$

Then:

$$|k|=\sqrt{(k_x^2+k_y^2)}$$ 

and 

$$|ko|=\sqrt{(ko_x^2+ko_y^2)}$$

 and,

$$\vec{k}\cdot\vec{ko}=\frac{(k_x\cdot ko_x+k_y\cdot ko_y)}{|k| |k_o|}$$

and

$$\vec{k}\times\vec{ko}=\frac{(k_x\cdot ko_y-k_y\cdot ko_x)}{|k| |k_o|}$$

We then define the following vectors $V_i$, $H_i$, $V_s$ and $H_s$:

$$
\begin{equation}
\begin{array}{l}
V_{ix}=\displaystyle\frac{q_o\cdot ko_x}{|k| |k_o|} \\
V_{iy}=\displaystyle\frac{q_o\cdot ko_y}{|k| |k_o|} \\
V_{iz}=\displaystyle\frac{|k_o|}{k_o}
\end{array}
\end{equation}
$$

and

$$
\begin{equation}
\begin{array}{l}
V_{sx}=\displaystyle\frac{q_k\cdot k_x}{|k| K} \\
V_{sy}=\displaystyle\frac{q_k\cdot k_y}{|k| K} \\
V_{sz}=\displaystyle\frac{|k|}{K}
\end{array}
\end{equation}
$$

$$
\begin{equation}
\begin{array}{l}
H_{ix}=\displaystyle\frac{ko_y\cdot ko_x}{|k_o|} \\
H_{iy}=\displaystyle\frac{-ko_x}{|k_o|} \\
H_{iz}=0
\end{array}
\end{equation}
$$

and

$$
\begin{equation}
\begin{array}{l}
H_{sx}=\displaystyle\frac{k_y}{|k|} \\
H_{sy}=\displaystyle\frac{-k_x}{|k|} \\
H_{sz}=0
\end{array}
\end{equation}
$$

Now we define:

$$
\begin{equation}
\begin{array}{l}
R_{VV}=\displaystyle\frac{\left(\frac{q}{2}\epsilon_{sw}-\sqrt{(\epsilon_{sw}-1)\cdot K^2+\frac{q^2}{4}}\right)}{(\frac{q}{2}\epsilon_{sw}+\sqrt{(\epsilon_{sw}-1)\cdot K^2+\frac{q^2}{4})}}\\
R_{HH}=-\displaystyle\frac{(q/2-\sqrt{(\epsilon_{sw}-1)\cdot K^2+\frac{q^2}{4}}}{(\frac{q}{2}+\sqrt{(\epsilon_{sw}-1)\cdot K^2+\frac{q^2}{4})}}\\
R_{VH}=(R_{VV}+R_{HH})/2 \\
R_{HV}=R_{VH} 
\end{array}
\end{equation}
$$

Then, let:

$$
\begin{equation}
\begin{array}{l}
Ki_{x}=ko_x/K \\
Ki_y=ko_y/K \\
Ki_z=-qo/K \\
\end{array}
\end{equation}
$$

and

$$
\begin{equation}
\begin{array}{l}
Ks_{x}=kx/K \\
Ks_y=k_y/K \\
Ks_z=q_k/K \\
\end{array}
\end{equation}
$$

We have:

$$
\begin{equation}
\begin{array}{l}
(K_s\times K_i)_x= Ki_z\cdot Ks_y - Ki_y\cdot Ks_z \\ 
(K_s\times K_i)_y= Ki_x\cdot Ks_z - Ki_z\cdot Ks_x \\
(K_s\times K_i)_z= Ki_y\cdot Ks_x - Ki_x\cdot Ks_y
\end{array}
\end{equation}
$$

and we next define:

$$
\begin{equation}
\begin{array}{l}
V_iK_s=Vi_x\cdot Ks_x+Vi_y\cdot Ks_y+Vi_z\cdot Ks_z \\
H_iK_s=Hi_x\cdot Ks_x+Hi_y\cdot Ks_y+Hi_z\cdot Ks_z \\
K_iV_s=Ki_x.*Vs_x+Ki_y\cdot Vs_y+Ki_z\cdot Vs_z \\
K_iH_s=Ki_x.*Hs_x+Ki_y\cdot Hs_y+Ki_z\cdot Hs_z \\
\end{array}
\end{equation}
$$

$$
\begin{equation}
\begin{array}{l}
(V_iK_s\times K_i)=Vi_x\cdot (K_s\times K_i)_x+Vi_y\cdot(K_s\times K_i)_y+Vi_z\cdot(K_s\times K_i)_z \\
(H_iK_s\times K_i)=Hi_x\cdot (K_s\times K_i)_x+Hi_y\cdot(K_s\times K_i)_y+Hi_z\cdot(K_s\times K_i)_z \\
(K_s\times K_iV_s)=(K_s\times K_i)_x\cdot Vs_x+(K_s\times K_i)_y\cdot Vs_y+(K_s\times K_i)_z\cdot Vs_z \\
(K_s\times K_iH_s)=(K_s\times K_i)_x\cdot Hs_x+(K_s\times K_i)_y\cdot Hs_y+(K_s\times K_i)_z\cdot Hs_z \\
(K_s\times K_i)^2=(K_s\times K_i)_x^2+(K_s\times K_i)_y^2+(K_s\times K_i)_z^2
\end{array}
\end{equation}
$$

Finally, the kernel functions $K_{\alpha\alpha_\circ}(\bf{k_{s}},\bf{k_o})$ are defined as follows:

$$
\begin{equation}
\begin{array}{l}
K_{VV}=\displaystyle\frac{q^2}{2}\cdot\left(\frac{R_{VV}\cdot V_iK_s\cdot K_iV_s + R_{HH}\cdot (V_{i}K_{s}\times K_i) \cdot (K_s\times K_{i}V_{s})}{(K_s\times K_i)^2}\right) \\
K_{VH}=\displaystyle\frac{q^2}{2}\cdot\left(\frac{R_{VV}\cdot V_iK_s\cdot K_iH_s + R_{HH}\cdot (V_{i}K_{s}\times K_i) \cdot (K_s\times K_{i}H_{s})}{(K_s\times K_i)^2}\right) \\
K_{HV}=\displaystyle\frac{q^2}{2}\cdot\left(\frac{R_{VV}\cdot H_iK_s\cdot K_iV_s + R_{HH}\cdot (H_{i}K_{s}\times K_i) \cdot (K_s\times K_{i}V_{s})}{(K_s\times K_i)^2}\right) \\
K_{HH}=\displaystyle\frac{q^2}{2}\cdot\left(\frac{R_{VV}\cdot H_iK_s\cdot K_iH_s + R_{HH}\cdot (H_{i}K_{s}\times K_i) \cdot (K_s\times K_{i}H_{s})}{(K_s\times K_i)^2}\right) \\
\end{array}
\end{equation}
$$




###### Numerical implementation


Recalling the expression for the bistatic scattering coefficients,

$$
\begin{equation}
\begin{array}{l}
    \sigma_{\alpha\alpha_o}(\vec{k}_{s},\vec{k}_{o})=\displaystyle\frac{1}{\pi}\left|\frac{2q_s
    q_o}{q_s+q_o}K_{\alpha\alpha_o}(\vec{k}_{s},
    \vec{k}_{o})\right|^2e^{\displaystyle-(q_s+q_o)^2\rho(0)}\cdot I_K
    \\ \\
    \end{array}
\end{equation}
$$

we see that we can incorporate the polarization-dependent coefficients multiplying the Kirchhoff Integral into the sum (\ref{eq:A11}) over
the Kirchoff Integral harmonics.  We thus obtain harmonic decompositions
of the bistatic scattering coefficients:

$$
\begin{equation}
\sigma_{\alpha\alpha_o}(\vec{k}_{s},\vec{k}_{o}, u_{10}, \varphi_w) =
\displaystyle\sum_{m=0}^{\infty}
\sigma^m_{\alpha\alpha_o}(\vec{k}_{s},\vec{k}_{o}) \cos{2m(\Phi_{si}-\varphi_w)},
\label{eq:B2}
\end{equation}
$$

where we have explicitly included the dependence of the final
scattering coefficients on the wind speed $u_{10}$ and wind direction
$\varphi_w$, and where

$$
\begin{equation}
\begin{array}{l}
    \sigma^m_{\alpha\alpha_o}(\vec{k}_{s},\vec{k}_{o})=\displaystyle\frac{1}{\pi}\left|\frac{2q_s
    q_o}{q_s+q_o}B_{\alpha\alpha_o}(\vec{k}_{s},
    \vec{k}_{o})\right|^2e^{\displaystyle-(q_s+q_o)^2\rho(0)}\cdot I^m_K
    \\ \\
    \end{array}
\end{equation}
$$

Note that the scattering coefficient harmonics are independent of wind
direction.  Moreover, these harmonics only depend on the incoming and
scattered radiation incidence angles, the wind speed, the dielectric constant and the difference
between the incoming and scattered radiation azimuth angles. Thus,
switching from vector notation to angles, we can write the scattering
coefficients as

$$
\begin{equation}
\sigma_{\alpha\alpha_o}(\theta_o, \phi_o, \phi_s, \theta_s, u_{10}, \varphi_w) =
\displaystyle\sum_{m=0}^{\infty}
\sigma^m_{\alpha\alpha_o}(\theta_o, \phi_s - \phi_o, \theta_s, u_{10}) \cos{2m(\Phi_{si}-\varphi_w)},
\end{equation}
$$

where $\theta_o$ is the incoming radiation incidence angle, $\phi_s$
is the scattered radiation azimuth angle, $\phi_o$ is the incoming
radiation azimuth angle, $\theta_s$ is the scattered radiation
incidence angle.  As defined previously, $\varphi_w$ is the wind
direction (towards which the wind is blowing) and $\Phi_{si}(\theta_o, \phi_o, \phi_s, \theta_s)  =
\tan^{-1}\left(\frac{\sin \theta_s \sin \phi_s + \sin \theta_o \sin
\phi_o}{\sin \theta_s \cos \phi_s + \sin \theta_o \cos \phi_o}\right)$ is a function of the incidence
and azimuth angles of the incoming and scattered radiation.


We pre-calculate lookup tables (LUTS) of the harmonic coefficients
$\sigma^m_{\alpha\alpha_o}(\theta_o, \phi_s - \phi_o, \theta_s,
u_{10})$ for even azimuthal wavenumbers 0 through 10 on a discrete
grid and interpolate from that grid.  As a final step,  the sum is
implemented over the harmonics, given specific values for the
incoming and scattered azimuths and wind direction. As it has been
found that only the first few harmonics contribute significantly
to the scattering coefficients, the sum is over the first six
even harmonics.

It is found that linear interpolation introduces artifacts in all
dimensions, but especially in incidence angle dimensions. We have
therefore implemented a cubic Hermite interpolation method.   The basic idea
behind the method is to ensure continuity of the first derivative
of the interpolating function. To accomplish this, a multi point
stencil along each dimension surrounding the interpolation point
is required but the nature of the method is such that one can
compute the coefficients once for all harmonics and reuse the
weights, saving a large amount of processing time.



##### Appendix F: Coordinate systems used for Sky Glint

We begin by establishing several reference frames. The first is the
earth fixed reference frame, or $\cal E$, centered at the center of
the earth reference ellipsoid, with cartesian basis $({\bf\hat x_e},
{\bf\hat y_e}, {\bf\hat{z}_e})$.  ${\bf\hat x_e}$ points outward along
the equatorial plane towards the Greenwich meridian, ${\bf\hat y_e}$
points outward along the equatorial plane towards 90 $^\circ$ E, and
${\bf\hat{z}_e}$ points towards the North Pole.

The second frame is the so-called alt-azimuth frame, or $\cal A$, with
cartesian basis $({\bf\hat x_t}, {\bf\hat y_t}, {\bf\hat z_t})$, with
${\bf\hat x_t}$ pointing eastward, ${\bf\hat y_t}$ pointing northward,
and ${\bf\hat z_t}$ pointing upward (zenith).  This frame is the same
as the topocentric frame defined in the EE CFI Mission Convention
Document  and is the natural frame in which to express
the surface scattering problem since the scattering geometry is
typically defined relative to the local surface orientation. Since
this coordinate frame is defined relative to the location on the
earth's surface, the projection of the cartesian basis vectors for
this frame in the earth fixed coordinate System depends on latitude
and longitude.

The third frame is the True of Date (TOD) celestial frame, or $\cal
C$, with cartesian basis $({\bf\hat x_c}, {\bf\hat y_c}, {\bf\hat
z_c})$, in which the celestial noise map is provided. This frame is
aligned with the earth fixed frame except for a time-dependent
rotation of $({\bf\hat x_c}, {\bf\hat y_c})$ relative to $({\bf\hat
x_e}, {\bf\hat y_e})$. The center of the frame coincides with the
center of the earth, and the x-axis coincides with the direction of
the true vernal equinox of date. This coordinate system's orientation
accounts for the nutation of the earth owing to a periodic effect of
the gravitation fields of the moon and other planets acting on the
earth's equatorial bulge.  Using the terminology in the EE CFI MCD, to
transform a vector from the TOD celestial frame to the earth fixed
frame we apply a rotation about the ${\bf\hat z_e}$-axis by the earth
rotation angle, $H$, which in turn is the sum of the Greenwich
siderial angle $G$ and a nutation angle $\mu$.

The transformation from one frame to another can be written as a $3
\times 3$ matrix, and we denote the transform from frame ${\bf I}$ to
frame ${\bf J}$ as ${\bf T_{IJ}}$. The matrices are built with the
help of EE CFI. As mentioned above, the transformation from the earth
fixed frame to the TOD celestial frame, denoted ${\bf T_{ec}}$, is a
rotation about the ${\bf\hat z_e}$-axis by an angle $H$. Introducing
the rotation matrix ${\bf R_z}$ for a counterclockwise rotation by
angle $\phi$ of the coordinate system about the ${\bf\hat z_c} =
{\bf\hat z_e}$-axis:

$$
\begin{equation}
\begin{array}{c}
{\bf R_z}(\phi) = \left( \begin{array}{ccc}
 \cos\phi & \sin\phi & 0 \\
-\sin\phi & \cos\phi & 0 \\
          0 &          0 & 1 \end{array} \right).
\end{array}
\end{equation}
$$

we can write the $\cal E$ to $\cal C$ transformation as

$$
\begin{equation}
{\bf T_{ec}} = {\bf R_z}(-H) = {\bf R_z}(-G - \mu),
\end{equation}
$$

Its inverse is

$$
\begin{equation}
{\bf T_{ce}} = {\bf T_{ec}}^{-1} = {\bf R_z}(H) = {\bf R_z}(G + \mu).
\end{equation}
$$

$G$ is a third-order polynomial in time.  When $G$ is expressed in
degrees and $T$ is the so-called Universal Time (UT1) expressed in the
Modified Julian Day 2000 (MJD2000) date convention, the formula for
$G$ is:

$$
\begin{equation}
G = 99.96779469 + 360.9856473662860 T + 0.29079 \times 10^{-12} T^2.
\end{equation}
$$

Universal Time, UT1, is very close to the common Coordinated Universal
Time (UTC), but differes slightly from it in that it does not differ
from actual atomic clock time (TAI) by a constant integer number of
seconds, whereas UTC time does. Since both UT1 and UTC are defined to
be consistent with the mean diurnal motion of the earth relative to
the stars, they must be adjusted periodically, but the adjustment is
accomplished differently in the two systems.  For UT1 time this is
done smoothly, whereas for UTC time this is accomplished by inserting
leap seconds at scheduled times into UTC when it is predicted to lag
behind UT1 time by a threshold (.9 s). This introduces discontinuities
in UTC. In UT1, no discontinuities exist since the adjustment is
continuous.

In the Modified Julian Day 2000 (MJD2000) $J_m$ date convention time,
either in the UT1 system or in the UTC system, is expressed as the
interval of time in days (including fractional days) since midnight
January 1, 2000. It differs from true Julian date $J_d$ by a constant
value of 2451544.5 decimal days, so that in decimal days we have

$$
\begin{equation}
J_d = J_m + 2451544.5.
\end{equation}
$$

As previously mentioned, the transformation between the alt-azimuth
frame $\cal A$ and another frame is complicated by the fact that the
alt-azimuth frame depends on location of the origin on the surface of
the earth, so that ${\cal A} = {\cal A}(\vartheta_g,\varphi_g)$, where
$\vartheta_g$ is geodetic latitude and $\varphi_g$ is the geodetic
longitude.  Here we take the geodetic longitude to be equal to the
geocentric longitude. The geodetic latitude is the angle between the
equatorial plane and the local normal to the earth's surface at the
point in question.  It is related to the spherical coordinate latitude
$\theta_s$ by the relation

$$
\begin{equation}
\theta_s = \tan^{-1}\left((1 - e^2)\tan \vartheta_g \right),
\end{equation}
$$
where

$$
\begin{equation}
e = \sqrt{\frac{A_e^2 - B_e^2}{A_e^2}},
\end{equation}
$$

is the eccentricity of the ellipsoidal earth, $A_e=6378137$ m is the
earth's major axis (equatorial) radius and $B_e=6356752.3142$ m is the
earth's minor axis radius.  To determine explicitly the transformation
from ${\cal A}$ to $\cal E$ we introduce a second rotation matrix that
rotates the coordinate system counterclockwise about the $x$-axis,

$$
\begin{equation}
\begin{array}{c}
{\bf R_x}(\phi) = \left( \begin{array}{ccc}
1 &           0 &          0 \\
0 &  \cos\phi & \sin\phi \\
0 & -\sin\phi & \cos\phi \end{array} \right);
\end{array}
\end{equation}
$$

To compute the composite transformation from $\cal A$ to $\cal E$, we
rotate the local alt-azimuth frame about the ${\bf\hat x_t}$-axis by
the angle $\phi = \vartheta_g - \frac{\pi}{2}$ to align ${\bf\hat
z_t}$ with ${\bf\hat z_e}$, and then we rotate the resulting
intermediate (primed) frame about the new ${\bf\hat z}$-axis ${\bf
\hat z_t'}$ by the angle $-\varphi_g - \frac{\pi}{2}$ to align ${\bf
\hat x_t'}$ with $x_e$. The resulting composite transformation from
$\cal A$ to $\cal E$ can be written as

$$
\begin{equation}
{\bf T_{ae}}(\vartheta_g,\varphi_g) = {\bf R_z}\left(-\varphi_g - \frac{\pi}{2}\right) {\bf R_x}\left(\vartheta_g - \frac{\pi}{2}\right),
\end{equation}
$$

and its inverse, mapping vectors from $\cal E$ to $\cal A$, is simply

$$
\begin{equation}
{\bf T_{ea}}(\vartheta_g,\varphi_g) = {\bf R_x}\left(-\vartheta_g + \frac{\pi}{2}\right) {\bf R_z}\left(\varphi_g + \frac{\pi}{2}\right).
\end{equation}
$$

To organize the results of the scattering calculations and to compare
results with specular reflection calculations, it is also useful to be
able to compute the specular direction for a given incidence/azimuth
angle.  This can be accomplished by applying a transformation matrix
${\bf T_{ref}}$ which rotates a vector by 180$^\circ$ about the ${\bf\hat
z_t}$ axis in the alt-azimuth coordinate system:

$$
\begin{equation}
{\bf T_{ref}} = {\bf R_z}\left(\pi\right).
\end{equation}
$$

The fourth frame is the instrument reference frame, $\cal I$. By our
convention, cartesian basis vectors for this frame, denoted $({\bf\hat
x_i}, {\bf\hat y_i}, {\bf\hat z_i})$ are such that ${\bf\hat x_i}$ points to the right
of instrument motion, normal to the orbital plane, ${\bf\hat y_i}$ points
in the direction of spacecraft motion, and ${\bf\hat z_i}$ points upward
normal to the instrument plane containing ${\bf\hat x_i}$ and $\hat
y_i$. The sign convention we adopt here for ${\bf\hat x_i}$ and ${\bf\hat y_i}$
is opposite that in EE CFI, in which ${\bf\hat y_i}$ is directed along
spacecraft velocity vector towards the rear, while ${\bf\hat x_i}$ is
directed normal to the orbital plane to the left of the spacecraft
velocity vector.  This reference frame is complicated by the fact that
the spacecraft position and orientation, or attitude, are functions of
time in $\cal E$, so the transformation matrix between $\cal I$ and
$\cal E$ is a function of time. Using CFI, at a particular time $t$
(expressed in UTC) we obtain the transformation matrix ${\bf T_{ie}}(t)$
transforming a vector from the instrument frame to the earth fixed
frame $\cal E$ and the inverse transform ${\bf T_{ei}}$.

To find the transformation from the alt-azimuth frame to the celestial
frame {\cal C}, we first transform from alt-azimuth to the earth fixed
frame and then transform the resulting vector from the earth fixed
frame to the celestial frame. The resulting transformation matrix is
thus

$$
\begin{equation}
{\bf T_{ac}} = {\bf T_{ec}} {\bf T_{ae}},
\end{equation}
$$

and its inverse, going from celestial coordinates to alt-azimuth coordinates, is

$$
\begin{equation}
{\bf T_{ca}} = {\bf T_{ac}}^{-1} = {\bf T_{ea}} {\bf T_{ce}}.
\end{equation}
$$

In each of the above frames we can represent vectors in a spherical coordinate
system, related to the corresponding cartesian coordinate system by the relations

$$
\begin{equation}
\begin{array}{rrl}
x_k &=& r_k \cos \theta_k \cos \phi_k,\\
y_k &=& r_k \cos \theta_k \sin \phi_k,\\
z_k &=& r_k \sin \theta_k,
\end{array}
\end{equation}
$$

where $k$ refers to the coordinate system, $\theta_k$ is the altitude
(or latitude), $\phi_k$ is the azimuth, and $r_k$ is range. In the
instrument frame, we follow convention and introduce the additional
director cosine representation of the spherical coordinates,
$$
\begin{equation}
\begin{array}{rrl}
\xi  &=& \sin \theta_b \cos \phi_i,\\
\eta &=& \sin \theta_b \sin \phi_i,
\end{array}
\end{equation}
$$

where $\theta_b = \frac{\pi}{2} - \theta_i$ is the angle from
boresight. In considering the finite beamwidth of the synthetic
antenna we need to integrate over the solid angle $d\Omega = \sin
\theta_b d\theta_b d\phi = \sin\theta_b J^{-1}(\theta_b,\phi)d\xi
d\eta$ subtended by the beam, where J is the Jacobian of the
transformation from $(\theta_b,\phi)$ to $(\xi,\eta)$. The Jacobian is

$$
\begin{eqnarray}
J(\theta_b,\phi) = \pp{(\xi,\eta)}{(\theta_b,\phi)} =
\left|
\begin{array}{cc}
\pp{\xi}{\theta_b}  & \pp{\xi}{\phi} \\
\pp{\eta}{\theta_b} & \pp{\eta}{\phi}
\end{array}
\right|
=
\left|
\begin{array}{cc}
\cos \theta_b \cos \phi & -\sin \theta_b \sin \phi\\
\cos \theta_b \sin \phi &  \sin \theta_b \cos \phi
\end{array}
\right| &=& \cos \theta_b \sin \theta_b\nonumber\\
&=& \sqrt{1-\xi^2-\eta^2} \sin\theta_b.
\end{eqnarray}
$$

and so the transformed solid angle increment is

$$
\begin{equation}
d\Omega = \sin \theta_b d\theta d\phi = \frac{d\xi d\eta}{\cos \theta_b} =
\frac{d\xi d\eta}{\sqrt{1 - \xi^2 - \eta^2}}.
\end{equation}
$$

It should be noted that in the above transformations we are concerned
about transforming directions rather than absolute positions, since we
assume that the celestial signal is at such a large distance that
origin shifts by distances on the order of the earth diameter do not
alter significantly the direction of a particular celestial source.

A fifth frame is termed the geographic frame, $\cal G$. This frame is
related to the instrument frame by a rotation about the ${\bf\hat
x_i}$ axis by a tilt angle $t$ to align the instrument ${\bf\hat
z_i}$ axis with the ${\bf\hat z_t}$ axis of the topocentric frame at
the satellite subpoint $O=(\vartheta_{sp}, \varphi_{sp})$, ${\cal A}_o
= {\cal A}(\vartheta_{sp}, \varphi_{sp}) = \cal G$. Thus, ${\bf\hat
x_i}$ is parallel to ${\bf\hat x_t}(\vartheta_{sp},\varphi_{sp})$.

As mentioned above, the alt-azimuth frame is the natural frame in
which to perform bistatic scattering calculation, since in this frame
relevant incidence and azimuth angles are readily computed. Therefore,
in performing scattering calculations for CIMR, we must establish a
local alt-azimuth frame for every point in the FOV at which we wish to
compute the scattered signal in the instrument frame. To this end, for
each $(\xi,\eta)$ in the director cosine coordinate system, we use CFI
to determine the satellite location in $\cal E$ and we then determine
the location on the earth reference ellipsoid at which the line of
sight (LOS) intersects the earth. We let $(x_t,y_t,z_t)$ be the
unknown target location on the earth's surface in $\cal E$,
$(x_s,y_s,z_s)$ be the instrument location $\cal E$, and we let
$(d_x,d_y,d_z)$ be the unit vector in the instrument look direction in
$\cal E$. Then if we let $R$ be the range from instrument to the
target, the target location is $(x_s+R d_x, y_s+R d_y, z_s+R d_z)$,
and it must satisfy the equation for an ellipsoid,

$$
\begin{equation}
\frac{(x_s + R d_x)^2}{A_e^2} + \frac{(y_s + R d_y)^2}{A_e^2} + \frac{(z_s + R d_z)^2}{B_e^2} = 1.
\end{equation}
$$

Expanding this equation and letting

$$
\begin{eqnarray}
A &=& \frac{d_x^2}{A_e^2} + \frac{d_y^2}{A_e^2} + \frac{d_z^2}{B_e^2},\\
B &=& \frac{2 x_s d_x}{A_e^2} + \frac{2 y_s d_y}{A_e^2} + \frac{2 z_s d_z}{B_e^2},\\
C &=& \frac{x_s^2}{A_e^2} + \frac{y_s^2}{A_e^2} + \frac{z_s^2}{B_e^2} - 1,
\end{eqnarray}
$$

the solution for R, the distance to the target, is

$$
\begin{equation}
R = -\frac{B}{2A} \pm \frac{\sqrt{B^2 - 4AC}}{2A}
\end{equation}
$$

If $B^2 - 4AC < 0$ or if $R < 0$ then the line of sight does not
intersect the earth surface, in which case the instrument receives
radiation directly from space. In the case of multiple positive
solutions, the smaller one corresponds the first intersection of
the line of sight with the earth surface, and the other is on the
other side of the earth.





