# Annex

##### Appendix A: TSM ROTATION MATRIX

As shown in {numref}`Conf_coordinate_system`, $\hat{z_l}$ is a normal unit vector to the local
facet defined as follows

$$
\hat{z_l}=\displaystyle\frac{-s_x\hat{x}-s_y\hat{y}+\hat{z}}{\sqrt{1+s_x^2+s_y^2}}
$$

where $s_x$ and $s_y$ represent the slopes in the x- and y-directions for the local facet coordinate, respectively, relative to the mean surface. In addition, $\hat{z_l}$ can be expressed in terms of zenith and
azimuth angles $\theta_n$ and $\phi_n$ of the local facet with respect to mean surface normal vector  $\hat{z}$ in the global coordinate system, by,

$$
\hat{z_l}=\sin{\theta_n}\cos{\phi_n}\hat{x}+\sin{\theta_n}\sin{\phi_n}\hat{y}+\cos{\theta_n}\hat{z}
$$

Assuming $\hat{x}$ points in the wind direction without loss of
generality, the local facet vector in the x-direction $\hat{x_l}$ can be
chosen to lie in the xz plane. By enforcing $\hat{x_l}$ to be orthogonal
to $\hat{z_l}$ , we define an angle $\beta$ to calculate $\hat{x_l}$ and $\hat{y_l}$ as :

$$
\beta=\tan^{-1}{(\tan{\theta_n}\cos{\phi_n})}
$$

Using $\beta$ defined in previous equation, $\hat{x_l}$ and $\hat{y_l}$ can be written as follows:

$$
\hat{x_l}=\displaystyle\frac{\hat{x}+s_x\hat{z}}{\sqrt{1+s_x^2}}=\cos{\beta}\hat{x}-\sin{\beta}\hat{z}
$$

$$
\hat{y_l}=\hat{z_l}\times\hat{x_l}
$$

Therefore, we can relate the unit vectors of the local
coordinate system with those of the global coordinate system
using a matrix, where the zenith and azimuth angles of the local normal vector
can be calculated using the following relationship:

$$
\theta_n=\displaystyle\cos^{-1}{\left(\frac{1}{\sqrt{s_x^2+s_y^2+1}}\right)}
$$

$$
\phi_n=\tan^{-1}{\left(\frac{s_y}{s_x}\right)}
$$

The electromagnetic wave vector $\bar{k}$ in the global coordinate
system can be related to that in the local coordinate system $\bar{k_l}$ by implementing an associated matrix function 
$\overline{\overline A}$, that is,

$$
\bar{k_l}=\overline{\overline Ak}
$$

Now, the relationship between the polarization basis vector
directions in the local and global coordinates can be determined.
For the two linear polarization unit vectors $(\hat{h},\hat{v})$ and
in the global coordinate system,

$$
\hat{h}=\displaystyle\frac{\bar{k}\times\bar{z}}{|\bar{k}\times\bar{z}|}
$$

$$
\hat{v}=\displaystyle\frac{\bar{h}\times\bar{k}}{|\bar{h}\times\bar{k}|}
$$

where $\hat{h}$ and $\hat{v}$ are the global unit vectors for horizontal and
vertical polarizations, respectively. Thus, for these unit vectors
in the local coordinate system,


$$
\hat{h_l}=\displaystyle\frac{\bar{k}_l\times\bar{z}_l}{|\bar{k}_l\times\bar{z}_l|}
$$

$$
\hat{v_l}=\displaystyle\frac{\bar{h}_l\times\bar{k}_l}{|\bar{h}_l\times\bar{k}_l|}
$$

Using the inner product, the cross-polarization angle $\alpha_r$  can
be obtained as

$$
\alpha_r=\cos^{-1}{(\hat{v}\cdot\hat{v_l})}=\cos^{-1}{(\hat{h}\cdot\hat{h_l})}
$$

or

$$
\alpha_r=\sin^{-1}{(\hat{v}\cdot\hat{h_l})}=\sin^{-1}{(-\hat{h}\cdot\hat{v_l})}
$$

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
$$

where $E_{hg}$ and $E_{vg}$ are the horizontally and vertically polarized electric field
components in the global polarization coordinate,
respectively. Therefore, the emissivity in the global
coordinate system can be now expressed in the matrix form
as

$$
\bar{e}_g=\overline{\overline{R}} \bar{e}_l
$$

where $\bar{e}_l$ and $\bar{e}_g$ are the Stokes vectors of the ocean surface
emissivity in the local and global coordinate systems, respectively. The rotation matrix $\overline{\overline{R}}$ for relating global
and local coordinate systems is:
 
 $$
\overline{\overline{R}} =
\begin{pmatrix}
\cos^2{\alpha_r} & \sin^2{\alpha_r} & \frac{1}{2}\sin{2\alpha_r} & 0 \\
\sin^2{\alpha_r} & \cos^2{\alpha_r} & -\frac{1}{2}\sin{2\alpha_r} & 0 \\
-\sin{2\alpha_r} & \sin{2\alpha_r} & \cos{2\alpha_r} & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$
The above form does not include
foam influences for simplicity.


##### Appendix B: SPM  polarimetric bistatic coefficients

The bistatic coefficients are exactly those given by Yueh et al
(1994a) in appendix 1,2 and 3. 


###### First-Order Scattering coefficients

The coefficients for the incoherent bistatic scattering
coefficients due to the first-order scattered fields are defined
as:

$$
\Gamma_{\alpha\beta\mu\nu}(k_x,k_y,k_xi,k_yi)
=f_{\alpha\beta}^{(1)}(\theta,\phi;\theta_i,\phi_i)f_{\mu\nu}^{(1)\star}(\theta,\phi;\theta_i,\phi_i)
$$

with

$$
\begin{matrix}
f_{hh}^{(1)}(\theta,\phi;\theta_i,\phi_i)=\displaystyle\frac{2k_{zi}(k_1^2-k_o^2)}{k_z+k_{1z}}\displaystyle\frac{1}{k_{zi}+k_{1zi}}\\
\\
\cdot\left(\displaystyle\frac{k_{xi}}{k_{\rho_i}}\displaystyle\frac{k_x}{k_{\rho}}+\displaystyle\frac{k_{yi}}{k_{\rho_i}}\displaystyle\frac{k_y}{k_{\rho}}\right)
\end{matrix}
$$

$$
\begin{matrix}
f_{hv}^{(1)}(\theta,\phi;\theta_i,\phi_i)=\displaystyle\frac{2k_{zi}(k_1^2-k_o^2)}{k_z+k_{1z}}\displaystyle\frac{k_{1zi}k_o}{k_1^2k_{zi}+k_o^2k_{1zi}}\\
\\
\cdot\left(-\displaystyle\frac{k_{yi}}{k_{\rho_i}}\displaystyle\frac{k_x}{k_{\rho}}+\displaystyle\frac{k_{xi}}{k_{\rho_i}}\displaystyle\frac{k_y}{k_{\rho}}\right)
\end{matrix}
$$

$$
\begin{matrix}
f_{vh}^{(1)}(\theta,\phi;\theta_i,\phi_i)=\displaystyle\frac{2k_{zi}(k_1^2-k_o^2)}{k_1^2k_z+k_o^2k_{1z}}\displaystyle\frac{k_{1z}k_o}{k_{zi}+k_{1zi}}\\
\\
\cdot\left(-\displaystyle\frac{k_{yi}}{k_{\rho_i}}\displaystyle\frac{k_x}{k_{\rho}}+\displaystyle\frac{k_{xi}}{k_{\rho_i}}\displaystyle\frac{k_y}{k_{\rho}}\right)
\end{matrix}
$$

$$
\begin{matrix}
f_{vh}^{(1)}(\theta,\phi;\theta_i,\phi_i)=\displaystyle\frac{2k_{zi}(k_1^2-k_o^2)}{k_1^2k_z+k_o^2k_{1z}}\displaystyle\frac{1}{k_1^2k_{zi}+k_o^2k_{1zi}}\\
\\
\cdot\left[k_1^2k_{\rho}k_{\rho_i}-k_o^2k_{1z}k_{1zi}\left(\displaystyle\frac{k_{xi}}{k_{\rho_i}}\displaystyle\frac{k_x}{k_{\rho}}+\displaystyle\frac{k_{yi}}{k_{\rho_i}}\displaystyle\frac{k_y}{k_{\rho}}\right)\right]
\end{matrix}
$$

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
$$ 

and $k_o$ and $k_1$ are the electromagnetic wavenumbers of the
free space and lower half space. In sea water with complex
dielectric constant $\epsilon_{sw}$, the electromagnetic
wavenumber is complex and given by:
$k_1=k_o\sqrt{\epsilon_{sw}}$


