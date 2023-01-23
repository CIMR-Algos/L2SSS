# Baseline Algorithm Definition



### Sea Surface Salinity retrieval from Satellite Antenna brightness temperature at L-band

The present algorithm aims at retrieving the surface salinity by finding the best-fit
solution to minimize the difference between the CIMR L-band Top of the Atmosphere brightness temperature Stokes vector data and a forward radiative transfer model. 

```{figure} Figure8rev.png
---
name: Figure8
---
Major signals received by a Space borne L-band radiometer
```



As illustrated in {numref}`Figure8`, several geophysical parameters other than seawater salinity and temperature contribute significantly to L-band $T_{B}$ measured by 
satellite sensors at antenna level (e.g., see {cite:t}`yueh2001error`; {cite:t}`Font2004`, {cite:t}`Reul2020`). 
To properly retrieve SSS, these contributions need to be accurately known and used in corrections of measured, or forward model simulations, of antenna $T_{B}$. 
They include:  
* the direct and sea surface reflected/scattered solar and sky emission ({cite:t}`LeVine2005a`; {cite:t}`Reul2007`; {cite:t}`Reul2008b`; {cite:t}`tenerelli2008earth`; {cite:t}`dinnat2008impact`), 
* the Faraday rotation in the ionosphere ({cite:t}`yueh2000estimates`; {cite:t}`le2002effect`; {cite:t}`vergely2014new`),  
* the impact of the atmosphere ({cite:t}`liebe1992atmospheric`; {cite:t}`skou2005band`; {cite:t}`wentz2016atmospheric`), and,
* the effect of sea surface roughness on L-band emissivity ({cite:t}`meissner2014emission`;{cite:t}`meissner2018salinity`; {cite:t}`yin2016roughness`; {cite:t}`yueh2010passive`; {cite:t}`yueh2014aquarius`).   
* the effect of the portion of the energy received due to land or sea ice in the CIMR antenna pattern when the main lobe is over water but close to land-sea or ice-sea 
transition (e.g., see {cite:t}`Reul2020`,{cite:t}`meissner2021SeaIce`)

The upwelling brightness temperatures above the atmosphere but below the ionosphere (before Faraday rotation) is referred hereafter to as the "Top of Atmosphere" brightness temperature and denoted $T_{tp}^{TOA}$ (with superscript "TOA") for upwelling signal in polarization $p$.
Considering all components of the scene brightness temperature at L-band, the complete model solution for $T_{tp}^{TOA}$, in the surface polarization basis, is:


$$
\left(\begin{matrix}
T_{th}^{TOA} \\ 
T_{tv}^{TOA} \\
U^{TOA} \\
V^{TOA}
\end{matrix}\right)=
\left(\begin{matrix}
T_{atm}^{up}+(τ_d τ_v )[T_{surf,h}^{tot}+R_{surf,h}^{tot}\cdot T_{atm}^{dw}+T_{sch}+T_{ssh}] \\
T_{atm}^{up}+(τ_d τ_v )[T_{surf,v}^{tot}+R_{surf,v}^{tot}\cdot T_{atm}^{dw}+T_{scv}+T_{ssv}] \\
(τ_d τ_v ) T_{erU} \\
(τ_d τ_v ) T_{erV} \\
\end{matrix}\right)
$$ (eq1)

where the only contribution to the third and fourth Stokes parameters in the surface polarization basis comes from the rough surface emission components, and in which:

| Notation | Definition | 
| :-: | :-: |
| $T_{atm}^{up}$ | Unpolarized upwelling brightness temperature of atmospheric 1-way emission [K]|
|$τ_d$ | 1-way atmospheric transmittance associated with molecular oxygen absorption [nd]|
|$τ_v$|	1-way atmospheric transmittance associated with water vapor absorption [nd]|
|$T_{surf,p}^{tot}$| p-pol brightness temperature of the total sea surface emission (specular+rough+foam) [K] |
|$R_{surf,p}^{tot}$|	reflectivity of the total sea surface (specular+rough+foam) in p-pol|
| $T_{atm}^{dw}$ | Unpolarized downwelling brightness temperature of atmospheric 1-way emission [K]|
|$T_{erU}$|	Third Stokes brightness temperature of rough surface emission (surface pol. Basis) [K]|
|$T_{erV}$|	Fourth Stokes brightness temperature of rough surface emission (surface pol. Basis) [K]|
|$T_{scp}$|	p-pol brightness temperature of scattered celestial sky radiation (surface pol. Basis) [K]|
|$T_{ssp}$|	p-pol brightness temperature of scattered solar radiation (sunglint) (surface pol. Basis) [K]|

In our algorithm (see dedicated subsection [Atmospheric contributions at L-band](#atmospheric-contributions-at-l-band)), the upwelling and downwelling atmospheric emission 
are assumed to  be equal :

$$T_{atm}^{up}=T_{atm}^{dw}=T_{ea}\tag{2}$$ (eq2)

where $T_{ea}$ is the unpolarized brightness temperature of vertically integrated atmospheric 1-way emission. 
In addition, the brightness temperature of the total sea surface emission, $T_{surf,p}^{tot}$ can be decomposed as follows:

$$T_{surf,p}^{tot}=T_s\cdot e_{surf,p}^{tot}=T_s\cdot\left[(1-F_f)\cdot(e_{sp}+e_{rp})\right]+T_{foam,p}=(1-F_f)\cdot(T_{esp}+T_{erp})+T_{foam,p}$$ (eq3)

in which:

| Notation | Definition | 
| :-: | :-: |
|$T_s$|	Sea Surface Temperature [K]|
|$e_{surf,p}^{tot}$| p (h or v)-pol total sea surface emission (specular+rough+foam) [K] |
|$F_f$|	Fractionnal area of sea surface covered by foam [nd]|
|$T_{esp}$|	p-pol brightness temperature of specular emission (surface pol. Basis) [K]|
|$T_{erp}$|	p-pol brightness temperature of rough surface emission (surface pol. Basis) [K]|
|$T_{foam,p}$| p-pol brightness temperature of the total foam-covered surface emission (surface pol. Basis) [K]|

and where we split the total sea surface brightness temperature $T_{surf,p}^{tot}$ into three components: 

* the brightness temperature from the non-foamy perfectly flat sea surface $(T_{esp})$, 
* the brightness temperature change induced by the non-foamy rough sea surface $(T_{erp})$, and, 
* the brightness temperature contrast induced by the totally foam-covered sea surface $(T_{foam,p})$. 

Note that the total surface reflectivity is related to the total surface emissivity by $R_{surf,p}^{tot}=1-e_{surf,p}^{tot}$.

Considering all components of the scene brightness temperature at L-band, the complete model solution for the upwelling brightness temperatures above the atmosphere but below the ionosphere (before Faraday rotation) in the surface polarization basis, is, therefore in H-polarization:

$$T_{th}^{TOA}=T_{ea}+(τ_d τ_v )[(1-F_f)\cdot(T_{esh}+T_{erh})+T_{foam,h}+R_{surf,h}^{tot}\cdot T_{ea}+T_{sch}+T_{ssh}]$$ (eq4)

and in V-polarization:

$$T_{tv}^{TOA}=T_{ea}+(τ_d τ_v )[(1-F_f)\cdot(T_{esv}+T_{erv})+T_{foam,v}+R_{surf,v}^{tot}\cdot T_{ea}+T_{scv}+T_{ssv}]$$ (eq5)

in which:

| Notation | Definition | 
| :-: | :-: |
|$T_{esh}$|	H-pol brightness temperature of specular emission (surface pol. Basis) [K]|
|$T_{erh}$|	H-pol brightness temperature of rough surface emission (surface pol. Basis) [K]|
|$T_{sch}$|	H-pol brightness temperature of scattered celestial sky radiation (surface pol. Basis) [K]|
|$T_{ssh}$|	H-pol brightness temperature of scattered solar radiation (sunglint) (surface pol. Basis) [K]|
|$T_{esv}$|	V-pol brightness temperature of specular emission (surface pol. Basis) [K]|
|$T_{erv}$|	V-pol brightness temperature of rough surface emission (surface pol. Basis) [K]|
|$T_{scv}$|	V-pol brightness temperature of scattered celestial sky radiation (surface pol. Basis) [K]|
|$T_{ssv}$|	V-pol brightness temperature of scattered solar radiation (sunglint) (surface pol. Basis) [K]|
|$T_{ea}$|	Unpolarized brightness temperature of atmospheric 1-way emission [K]|
|$R_h$|	Fresnel power reflection coefficient at the surface in H-pol|
|$R_v$|	Fresnel power reflection coefficient at the surface in V-pol|
|$e_{sh}$|	Perfectly flat sea surface emissivity in H-pol|
|$e_{sv}$|	Perfectly flat sea surface emissivity in V-pol
|$e_{rh}$|	Rough surface emissivity in H-pol|
|$e_{rv}$|	Rough surface emissivity in V-pol
|$T_{erU}$|	Third Stokes brightness temperature of rough surface emission (surface pol. Basis) [K]|
|$T_{erV}$|	Fourth Stokes brightness temperature of rough surface emission (surface pol. Basis) [K]|
|$F_f$|	Fractionnal area of sea surface covered by foam [nd]|
|$T_s$|	Sea Surface Temperature [K]|
|$U_{10}$|	10-m height Sea surface wind speed modulus [m/s]|


### Level-2 end to end algorithm functional flow diagram

#### Input Data 

```{figure} SketchL1BtoL1C.png
--- 
name: SketchL1BtoL1C
---
Level-1B to Level-1C L-band $T_B$ algorithm functional flow diagram. 

```


The inputs to the L2B SSS algorithm are the Level 1C $T_B$ at L-band in H-, V-, 3rd, and, 4th Stokes for the fore and aft views. 
As summarized in {numref}`SketchL1BtoL1C`, we assume here that the $T_Bs$ data are corrected for antenna spill-over and emissivity, sky and sun direct and earth reflected/scattered radiation,
Faraday rotation across the ionosphere, and that they are provided with a proper rotation of the Stokes parameter from the antenna polarization basis to the surface polarization basis 
(L1B processor).  In fine, we will use as input the Leve1b data after proper resampling (so-called Level1c).

It is understood that the corrections for (1) the sky and sun direct and earth reflected/scattered radiation, (2) the model for the 
rotation of the Stokes parameter due to Faraday rotation across the ionosphere and polarisation basis changes, 
as well as (3) the L1B resampling approach all shall be applied through the Level 1 algorithms and not in
 the present L2B SSS algorithm. Nevertheless,
  we will present hereafter how these contributions can be potentially modelled.

In addition, it is not clear at which level (1) or (2) the land- and ice-ocean transitions effects shall be corrected for on the $T_b$s 
(so called land sea contammination). Given that these effects require antenna-pattern -related information, it shall better be applied in the Level 1 algorithms.
We shall propose in version 2 of this ATBD some post-launch approaches to derive such corrections.

Another input data are the CIMR L2B SST and Wind products, properly resampled on the L-band acquisitions.


#### Retrieval algorithm

```{figure} SketchL1CtoL2SSS.png
--- 
name: Sketch1
---
Level-2 SSS end to end algorithm functional flow diagram. Here $SST^p$, $SSS^p$, $U_{10}^p$, and $\Phi_w^p$ are *a priori values* for the SST, SSS, 10 meter height surface wind speed and direction, respectively. 

```


The principle of the proposed SSS retrieval algorithms rely on a forward radiative transfer modelling of the top of the atmosphere brightness $T_{tp}^{TOA}$ from first 
guess geophysical values (SSS, SST, $U_{10}$, etc...),and, the retrieval of the geophysical parameters (SSS, SWS, ..) from a minimization of the differences between the 
observed and modelled  $T_{tp}^{TOA}$. The Level-2 end to end algorithm functional flow diagram is shown in {numref}`Sketch1`.
The radiative transfer forward model which is needed is based on the following components:

-  a sea-water dielectric constant model at 1.4 GHz,
-  a perfectly flat, or specular, sea surface emission model,
-  a surface roughness and foam-induced correction model,
-  a Radiative Transfer Model for Atmospheric corrections,
-  a scattering model to correct for sea surface scattered Solar and celestial radiation, and, 
-  a correction for geometric rotation from surface polarization basis to antenna polarization basis,
-  and, to model the $T_B$ at antenna level, a model to correct for Faraday rotation in the ionosphere.

We review these forward model components and corrections in the following subsections.

### Models for the Dielectric Constant of seawater at 1.4 GHZ

#### Mathematical Description
 
 The {cite:t}`zhou2021seawater`'s Debye model for the seawater dielectric
constant is used in the present algorithm, will be refered to as "GW2020", and can be expressed by:

 $$ε_{sw}(f, S, T)=\displaystyle ε_{\infty}+\frac{(ε_{s-dw}(T)R_{sw-dw}(S,T)-ε_{\infty})}{1+i \omega \tau(T)}-i\frac{\sigma(f,S,T)}{\omega ε_0}$$ (eq6)

where $f$ is the electromagnetic frequency ([Hz]), $T$ and $S$ are the temperature ([degree celsius]) and salinity ([pss]) of seawater, respectively; $ε_0$ is the dielectric
constant of free space; $ε_{s-dw}(T)$ is the static dielectric
constant of distilled water, given by:

$$ε_{s-dw}(T)=88.0516-4.01796\times10^{-1}\cdot T-5.1027\times10^{-5}\cdot T^2+2.55892\times10^{-5}\cdot T^3$$ (eq7)

and $\tau(T)$  is the relaxation time of distilled water:

$$\tau(T)=1.75030\times10^{-11}-6.12993\times10^{-13}\cdot T +1.24504\times10^{-14}\cdot T^2-1.14927\times10^{-16}\cdot T^3$$ (eq8)

$R_{sw-dw}(S,T)$ is an additional factor in the static dielectric constant of seawater due to the presence of ions, given by: 

$$
R_{sw-dw}(S,T)=\displaystyle 1-S\cdot ( 3.97185\times10^{-3}-2.49205\times10^{-5}\cdot T-4.27558\times10^{-5}\cdot S +3.92825\times10^{-7}\cdot S\cdot T+4.15350\times10^{-7}\cdot S^2)
$$(eq9)

Note that $\sigma(f,S,T)$ needs to be nulled at $S=0$ since the conductivity of distilled water is close to 0. The expression of 
$\sigma(f,S,T)$ given in {cite:t}`zhou2021seawater` is:

$$\sigma(f,S,T)=\sigma(f,S,0)\cdot R_{\sigma}(f,S,T)$$ (eq10)

where for f=1.4 GHz,

$$\sigma(f,S,0)=9.50470\times10^{-2}\cdot S -4.30858\times10^{-4}\cdot S^2+2.16182\times10^{-6}\cdot S^3$$ (eq11)

and

$$R_{\sigma}(f,S,T)=1+T\cdot(3.76017\times10^{-2} + 6.32830\times10^{-5}\cdot T +4.83420\times10^{-7}\cdot T^2 − 3.97484\times10^{-4}\cdot·S+6.26522\times10^{-6}\cdot S^2)$$ (eq12)

#### Functionnal flow diagram

```{figure} FFD_dielec.png
--- 
name: FFD_dielec
---
Dielectric Constant model Flow Diagram. Input data are Temperature $T$ [°C], salinity $S$ [pss], and, radiometer 
electromagnetic frequency $f$ [Hz]. Output are the real and imaginary parts of $ε_{sw}(f, S, T)$.
```

#### Assumption and limitations

The search for a model for the dielectric constant of sea water at 1.4 GHz accurate enough to promote improvements
in the retrieval of is not yet complete. There are at least two challenges. One is that making measurements that are
consistent with an accuracy of the salinity product of better than 0.2 pss is very hard. At CIMR OZA of 53° and 
for an SST of 25°C, an accuracy of 0.2 pss corresponds to radiometric accuracy of ~0.16 K for a measurement at 1.4 GHz (see {numref}`T0esv_GSW2020_LBand`). 
Assuming equal error, $\Delta$, in the real and imaginary parts of the
dielectric constant, an accuracy of about $\Delta$=0.25% in the
measurement of the dielectric constant is required (at 35 psu
and 25°C) to have an error of less than 0.1 K in $T_B$. The
current measurement accuracy of the GW2020 measurements at this
temperature and salinity is about 0.35% ({cite:t}`lang2016accurate`). So, there is yet a
need for improvement, and if the goal is eventually to achieve
0.1 psu, even more progress is needed.

In the meantime, a problem with many available dielectric constant models is the use
of the models outside of their range of validity. There are no
physical restrictions, which prevent using any of these models
at any frequency, salinity, or temperature, but all the models
discussed here are based on measurements of a finite range in
*S* and *T* and use mathematical functions (usually polynomials)
to fit the unknown parameters to the data in this range. The
fits are unconstrained outside of the range of the data. This
is evident in the case of the KS model. As found by {cite:t}`LeVine2022`, the real part of some models 
diverges strongly from the measurements for high ($T$ > 30°C) and low temperatures
($T$ <5°C).



### Perfectly flat or 'specular' sea surface emission


```{figure} Flat_Sea_Rad.png
--- 
name: Flat_Sea_Rad
---
Front view of the PALS radiometer instrument measuring the brightness temperature emitted by a perfectly flat surface of a saltwater pond. L-band radiometric measurements were made over
 a salinity range between 25 and 40 pss and a temperature range of 8.5°C to 32°C (from {cite:t}`Wilson2004`). 
```


#### Mathematical Description

The dependence of the microwave brigthness temperature emitted by the sea surface $T_{B}$ on SSS is contained in the emissivity, $e$: $T_{B} = T \times e$, 
where $\it{T}$ is the sea surface temperature. The emissivity $e$ is a quantity that depends on physical and chemical properties of the water (e.g. salinity and temperature),
 observational conditions (incidence angle, electromagnetic frequency, polarization), as well as on the sea surface roughness. 

For a perfectly flat ocean surface the scattered electric and magnetic fields may be expressed in terms of the incident fields. The reflected electric field 
components $(E_{h}^{'},E_{v}^{'})$  are related to the incident components $(E_{h},E_{v})$ by the diagonal matrix equation:

$$
\begin{pmatrix}
E_{h}^{'}(\theta_s,\phi_s) \\ 
E_{v}^{'}(\theta_s,\phi_s)
\end{pmatrix}=
\begin{pmatrix}
R_{hh}^{(0)} & 0 \\ 
0 & R_{vv}^{(0)}   
\end{pmatrix}
\begin{pmatrix}
E_{h}(\theta_s,\phi_s-180°) \\ 
E_{v}(\theta_s,\phi_s-180°)
\end{pmatrix}$$	(eq14)

where $(\theta_s,\phi_s)$ is the specular reflection direction for radiation incident from direction $(\theta_s,\phi_s-180°$). The superscripts on the reflection coefficients indicate that they correpond to zero order expansion in surface slope, i.e., the flat surface reflection. The flat surface reflection coefficients on the preceeding matrix are given by the Fresnel equations:

$$
R_{vv}^{(0)} (\theta_s,f,S,T_s)=\displaystyle\frac{\sqrt{ε_{sw}(f,S,T_s)-\sin^2⁡{\theta_s}}-ε_{sw}(f,S,T_s) \cos\theta_s}{\sqrt{ε_{sw}(f,S,T_s)-\sin^2{\theta_s}}+ε_{sw}(f,S,T_s) \cos{\theta}}
$$ (eq15) 

for vertical polarization, and,

$$
R_{hh}^{(0)} (\theta_s,f,S,T_s)=\displaystyle\frac{\sqrt{ε_{sw}(f,S,T_s)-\sin^2⁡\theta}-\cos\theta_s}{\sqrt{ε_{sw}(f,S,T_s)-\sin^2⁡θ_s}+\cos\theta_s}
$$ (eq16)  

for horizontal polarization.

In the expression above, $ε_{sw}(f, S, T_s)$ is the dielectric constant of sea water at electromagnetic frequency, $\it{f}$,
for a water body with salinity $\it{S}$ and temperature, $\it{T_s}$.
The Mueller-Stokes Matrix for Fresnel's reflection equation is:

$$
T'=
\begin{pmatrix}
T_{h}^{'} \\ 
T_{v}^{'} \\
U^{'} \\
V^{'}
\end{pmatrix}=
\begin{pmatrix}
|R_{hh}^{(0)}|^2 \delta^2 & 0 & 0 & 0 \\ 
0 & |R_{vv}^{(0)}|^2 \delta^2 & 0 & 0 \\
0 & 0 & \Re (R_{hh}^{(0)}(R_{vv}^{(0)})^{\ast})  & \Im{(R_{hh}^{(0)}(R_{vv}^{(0)})^{\ast})} \\
0 & 0 & -\Im (R_{hh}^{(0)}(R_{vv}^{(0)})^{\ast}) & \Re{(R_{hh}^{(0)}(R_{vv}^{(0)})^{\ast})}
\end{pmatrix}
\begin{pmatrix}
T_{h} \\ 
T_{v} \\
U \\
V
\end{pmatrix}
$$ (eq17)

where $\delta$ is the Kroneker delta, $\Re$ and $\Im$ are the real and imaginary part, 
respectively. For a perfectly flat ocean surface with salinity, $\it{S}$, temperature, 
$\it{T_s}$, and observed at incidence angle $\theta$, the emissivity at polarization, 
$\it{p}$ (horizontal or vertical), and electromagnetic frequency, $\it{f}$, 
(note that we quote the center of a microwave frequency bandwidth associated with
 a given radiometer) is given by {cite:t}`peake1959interaction`:

$$e_{sp}^{(0)} (\theta_s,f,S,T_s)=1-|R_{pp}^{(0)} (\theta_s,f,S,T_s)|^2$$ (eq18)

The specular brightness temperature emitted by the sea surface in horizontal polarization is then

$$T_{esh}^{(0)} (\theta_s,f,S,T_s)=T_{s}[1-|R_{hh}^{(0)} (\theta_s,f,S,T_s)|^2]$$ (eq19)

and in vertical polarization:

$$T_{esv}^{(0)} (\theta_s,f,S,T_s)=T_{s}[1-|R_{vv}^{(0)} (\theta_s,f,S,T_s)|^2]$$ (eq20)

where $R_{pp}^{(0)}$  is the Fresnel reflection coefficient given above.


As shown in {numref}`Figure3a`, the specular brightness temperature sensitivity to SSS $\partial T_{esp}/\partial SSS$ increases with decreasing electromagnetic frequency, peaking at ~1 GHz (L-band) and with increasing incidence angle. 
As the frequency band 1.400 to 1.427 GHz is protected for radio-astronomy observation, it has been used for SSS remote sensing.

Given a model for $ε_{sw}(f, S, T)$, in its simplest form, SSS remote sensing, therefore, consists of measuring/estimating the L-band $T_{esp}$ emitted by the
 perfectly flat ocean surface together with an auxilliary SST. The intersection of the two values on a graph such as shown in {numref}`T0esv_GSW2020_LBand` can then be used to retrieve SSS. 

We used the laboratory-measurement based GSW2020's model for the sea water dielectric constant at L-band to simulate the changes in the specular sea surface brightness temperatures at 1.4 GHz, 
at V- and H-polarization and for the the CIMR  nominal incidence angle of 53°. The results are shown as a function of sea surface salinity for different representative sea surface temperature 
values in {numref}`T0esv_GSW2020_LBand`.




```{figure} T0esv_GSW2020_LBand.png 
```

```{figure} T0esh_GSW2020_LBand.png
---
name: T0esv_GSW2020_LBand
---
Specular sea surface brightness temperatures at 1.4 GHz, at V- (a) and H- (b) polarization, for the CIMR nominal OZA of 53° and as a function of sea surface salinity (x-axis) 
for different representative sea surface temperature values (colors). The black and gray histograms represent the normalized distribution of historical in situ SSS observation at global scale and 
in the Arctic, respectively.

```


As found, the sensitivity of $T_B$ to SSS is quasi-linear for a given SST. $|\partial T_{B}/\partial SSS|$ is greater in V-polarization than in H-polarization and increases 
with increasing SST. The sensitivity of V-pol $T_B$ to SSS is dropping from -0.93K/pss at $T_s=30°C$ to -0.26K/pss 
at $T_s=0°C$.  In Artic conditions, $\partial T_{B}/\partial SSS$ ranges in V-polarization from -0.26 K/pss $(T_s=0°C)$ to -0.36 K/pss $(T_s=5°C)$. With an expected CIMR L-band radiometer 
NEDT~0.3 K, one can therefore expect instrumental noise errors in instantaneous single polarization recordings of~0.3 pss in the tropics ($T_s=30°C$), and ~1 pss 
in cold seas ($T_s=5°C$). 


#### Functionnal flow diagram

```{figure} FFD_fresnel.png
--- 
name: FFD_flatsea
---
Specular Sea Surface Emission model Flow Diagram. Input data are the real and imaginary parts of the sea water 
dielectric constant $ε_{sw}$, the radiometer incidence angle $\theta_s$ and the sea surface temperature $T$.
 Output data are the specular brightness temperature $T_{esv}^{(0)}$ and $T_{esh}^{(0)}$ in vertical and horizontal 
 polarisation, respectively.
```


###  Surface roughness-induced emission model at L-band ###


#### Physics of the problem 

At a given microwave frequency, the total surface emissivity $E_p$ can be modeled with a specular part $e_{sp}^{(0)} (\theta_s,S,T_s)$ and a part caused by ocean roughness $e_{rp}^{rough}$. In order to measure sea surface salinity with the required accuracy it is necessary to remove the ocean surface roughness signal from the observed CIMR brightness temperatures. This requires an accurate
knowledge of the signal itself as well as the ocean surface wind speed. Wind-induced surface waves are the primary contributor to
ocean surface roughness, with internal waves, wind-current
interactions, and ship wakes to be important but secondary
contributors ({cite:t}`Gasiewski1994`). Wind-induced waves can be divided into two
primary scales ({cite:t}`yueh1997`):  

1. The **large-scale waves** cause the local surface incidence angle to differ from the effective earth incidence
angle, and mix vertical and horizontal polarizations ({cite:t}`Gasiewski1994`), and,
 
2. The **small-scale gravity-capillary** waves riding on top the large-scale gravity waves ({cite:t}`john:zhang99`):  the small-scale waves modify the specular surface reflection (or emission) through the bistatic scattering of the radiation incident
upon the ocean surface. 


#### Two-scale Approach

##### Mathematical description

The two-scale ocean surface emissivity model approximates ocean surface emission statistics using two spectral scales by considering small-scale capillary waves as riding 
on the top of large-scale gravity waves ({cite:t}`yueh1997`). The total thermal emission is calculated by integration of the rough surface spectral emission
from each statistically tilted facet, wherein the tilt is governed by the large-scale gravity wave spectrum. In the two-scale
model, the full Stokes vector for the emissivity from a tilted facet is calculated in the local polarization coordinate system
for convenience and then transformed into the global (or Earth-centric) polarization coordinate basis. A rotation matrix $\overline{\overline{R}}$ is
used to convert the Stokes emission vector defined in the local polarization basis into that of the global polarization basis [{cite:t}`Johnson2006`].
The calculation can be written as


$$
e_{rp}^{rough}=\overline{e}_g=\overline{\overline{R}} \cdot [(1-F_r)\overline{e}_l+F_r \overline{e}_F]
$$ (eq21)

where $\overline{e}_l$ and $\overline{e}_g$ are the Stokes emissivity vectors in the local
and global coordinates, respectively, $F_r$ is the ocean foam
fractional coverage of the facet, and $\overline{e}_F$ is the emissivity vector
for a fully foam-covered surface facet. The Stokes emissivity
vector in the local facet coordinate system is defined as

$$
\overline{e_l} \equiv
\begin{pmatrix}
e_{v,l} \\
e_{h,l} \\
e_{U,l} \\
e_{V,l} \\
\end{pmatrix}
=\displaystyle\frac{\overline{T}_{B,l}}{T_S}
$$ (eq22)

 where $e_{v,l}, e_{h,l}, e_{U,l}$ and $e_{V,l}$ are the ocean surface emissivities for vertical polarization, horizontal polarization, and third and
fourth Stokes emissivity components in the local facet-normal coordinate system, respectively. The third and fourth Stokes
components represent the in-phase and quadrature cross-correlations between the upwelling vertical and horizontal polarized electric-field components, respectively.
The vector $\overline{T}_{B,l}$ is the Stokes vector of upwelling microwave thermal emission propagating at angles $(\theta_l,\phi_l)$ from the facet under the
condition of zero downwelling radiation, and $T_s$ is the surface physical temperature associated with microwave penetration depths of ~1 cm 
(average penetration depth at L-band). 
 
 The rotation matrix $\overline{\overline{R}}$ is
 
 $$
\overline{\overline{R}} =
\begin{pmatrix}
\cos^2{\alpha_r} & \sin^2{\alpha_r} & \frac{1}{2}\sin{2\alpha_r} & 0 \\
\sin^2{\alpha_r} & \cos^2{\alpha_r} & -\frac{1}{2}\sin{2\alpha_r} & 0 \\
-\sin{2\alpha_r} & \sin{2\alpha_r} & \cos{2\alpha_r} & 0 \\
0 & 0 & 0 & 1
\end{pmatrix}
$$ (eq23)
 
 where $\alpha_r$ is the polarization rotation angle between the local and global polarization systems defined in 
 {numref}`Conf_coordinate_system` and [Appendix A](#a-name-appendixa-a-appendix-a-tsm-rotation-matrix)
 This matrix is a function of the surface slopes and observation direction. A derivation of the rotation matrix is provided in [Appendix A](#a-name-appendixa-a-appendix-a-tsm-rotation-matrix).

```{figure} Conf_coordinate_system.png
--- 
name: Conf_coordinate_system
---
Configuration and coordinate system used in this ATBD.
```



The Stokes emissivity vector for small-scale emission is effectively averaged over the slope distributions of the large-scale
waves. In addition to the polarization basis rotation angle, the tilt angles due to large-scale waves also affect the
projected facet area in the direction of observation. In effect, the emission from tilted facets analyzed using the small-scale
emission theory is weighted by the subtended solid angle of the large-scale slanted surface as viewed by the observer ({cite:t}`yueh1997`). This
effect is considered by multiplying by a factor of $1-S_x\tan{\theta_o}$ in the global emissivity integrand for the integration over the
slope distributions of the large-scale waves. The derivation of this factor can be found in [Appendix B](#a-name-appendixb-a-appendix-b-tsm-projected-facet-area).
 
 Accordingly, the two-scale ocean surface emissivity vector $\overline{e}$ is calculated as
 
 $$
 \overline{e_{rough}}(\theta_o,\phi_o)=\displaystyle\int_{-\infty}^{\infty}\int_{-\infty}^{\cot{\theta_o}} \overline{e_g}(\theta_o,\phi_o;S_x,S_y)\times (1-S_x\tan{\theta_o}) P(S'_x,S'_y)dS_xdS_y
 $$  (eq24)
 
where $S_x$ and $S_y$ are the surface slopes in along- and across-radiometer
look directions, respectively, $\theta_o$ is the Earth observation
angle with respect to the local zenith direction of the mean surface, and $\phi_o$ is the relative azimuth angle between
wind and radiometer look directions.  $S_x$ and $S_y$ are calculated from the slopes in the upwind and crosswind directions along with the azimuth angle $\phi_o$:

$$
\begin{pmatrix}
S_x' \\
S_y' \\
\end{pmatrix}=
\begin{pmatrix}
\cos{\phi_o} & -\sin{\phi_o} \\
\sin{\phi_o} & \cos{\phi_o} \\
\end{pmatrix}
\cdot
\begin{pmatrix}
S_x \\
S_y \\
\end{pmatrix}
$$ (eq25)

where $S_x'$ and $S_y'$  are the surface slopes in upwind and crosswind
coordinate system directions, respectively. In this ATBD, the
upwind direction is fixed along the negative x-axis (i.e., the wind direction vector points in the positive x-direction).
A description of the geometry can be found in {numref}`Conf_coordinate_system`. The
integration over the upwind direction in the two-scale ocean surface emissivity is limited to $\cot{\theta_o}$ 
to preclude slopes that impart shadowing by large-scale waves in the direction of the observer.


The two-scale model consists of five distinct physical modules (see {numref}`TSM_Flowchart`):

- Sea water dielectric constant model (already provided in previous section),
- Large Scale wave corrections,
- Ocean Wave Height Spectrum,
- Small-scale wave perturbation,
- Foam Influences,


Each of these four last modules is discussed and clarified relative to the published literature described in the
following.

##### Corrections of local surface incidence angle due to Large Scale wave Slopes 

Here, following {cite:t}`LeeGasiewski2022`, the large scale waves slope probability distribution function $P(S'_x,S'_y)$ is based upon 
measurements by {cite:t}`Cox1954` and modeled
 using the slope variance described by the truncated Gram–Charlier series of coefficients obtained from this publication. This
probability density function (pdf) describes an anisotropic
ocean surface:

$$
\begin{matrix}
P(S'_x,S'_y) & = &\displaystyle\frac{1}{2\pi\sigma_u\sigma_c} \exp\left[-\frac{S'^{2}_x}{2\sigma^2_u}-\frac{S'^{2}_y}{2\sigma^2_c}\right] \\
    & & \times \displaystyle\begin{bmatrix}
                1-\displaystyle\frac{1}{2}c_1(\frac{S'^{2}_x}{\sigma^2_u}-1)\frac{S'_y}{\sigma_c}-\frac{1}{6}c_2(\frac{S'^{3}_y}{\sigma^3_c}-3\frac{S'_y}{\sigma_c}) \\
		+\displaystyle\frac{1}{24}c_3(\frac{S'^{4}_x}{\sigma^4_u}-6\frac{S'^{2}_x}{\sigma^2_u}+3)  \\
		+\displaystyle\frac{1}{4}c_4(\frac{S'^{2}_x}{\sigma^2_u}-1)(\frac{S'^{2}_y}{\sigma^2_c}-1) \\
		+\displaystyle\frac{1}{24}c_5(\frac{S'^{4}_y}{\sigma^4_c}-6\frac{S'^{2}_y}{\sigma^2_c}+3)  \\
				\end{bmatrix}
\end{matrix}
$$ (eq26)

where $c_1$ and $c_2$ are the skewness coefficients, and $c_3$,
$c_4$, and $c_5$ are the peakedness coefficients. These coefficients for the sea surface slope PDF are provided as function of $U_{12.5}$, the wind speed at 12.5 m above the ocean surfacesee Table :

| Coefficients |
| :-: |
|  $c_1=0.01-0.0086U_{12.5}$ |
|  $c_2=0.04-0.0330U_{12.5}$ |
|  $c_3=0.4$ |
|  $c_4=0.12$ |
|  $c_5=0.23$ |


The coefficients $\sigma_u$ and $\sigma_c$  are the root-mean-square (rms)
slopes for the upwind and crosswind directions, respectively,
which are calculated from the long-wave height spectrum
as

$$
\sigma^2_u=\int_0^{ku}\int_0^{2\pi}k_{\rho}^3 \cos{\phi}^2 W(k_{\rho},\phi)d\phi dk_{\rho}
$$ (eq27)

$$
\sigma^ 2_c=\int_0^{ku}\int_0^{2\pi}k_{\rho}^3 \sin{\phi}^2 W(k_{\rho},\phi)d\phi dk_{\rho}
$$ (eq28)

where $W(k_{\rho},\phi)$ is the directional ocean surface wave height spectrum
(discussed in next subsection), $k_{\rho}$  is the ocean surface
radial wavenumber, $k_u$ is the long-wave cutoff wavenumber
used to discriminate between the small-scale and large-scale
regimes, and $\phi$ is the wave azimuthal direction relative to
the wind. In previous studies ({cite:t}`john:zhang99`; {cite:t}`Johnson2006`,{cite:t}`LeeGasiewski2022`), the cutoff wavenumber
$k_u$ has been selected to be between $k_o/5$ and $k_o/10$, where $k_o$ is the electromagnetic wavenumber.
Determination of the cutoff wavenumbers (i.e., $k_l$ and $k_u$ in the TSM) is important because they determine the magnitude of
the small-scale bistatic surface scattering contribution. Physically, the calculated contribution of bistatic surface scattering is much more sensitive 
to $k_l$ than $k_u$ because $k_l$ prescribes the
somewhat critical boundary between the short- and long-wave
portions of the spectrum, while $k_u$ needs to be selected large
enough to ensure convergence of the SPM equations. Following {cite:t}`LeeGasiewski2022`, we set initial valuez of $k_l=k_o/10$ and $k_u=8k_o$.


##### Ocean Wave Height Spectrum Model

In the present algorithm, we use the semi-empirical spectrum model developed by V. N. Kudryavtsev and co-authors ({cite:t}`Kudryavtsev1999`,
{cite:t}`Kudryavtsev2003`,{cite:t}`Kudryavtsev2005`; Appendix A in {cite:t}`Yurovskaya2013`). The spatial spectrum of curvatures for short waves is calculated by solving the energy balance equation 
taking into account the wind effect, resonant nonlinear wave‐wave interactions, viscous dissipation, wave breaking, and generation of short waves by longer wave
breaking. The equation includes several parameters whose values are chosen by the best fit to the experimental data.
The Kudryavtsev spectrum model proposed in the first papers ({cite:t}`Kudryavtsev1999`, {cite:t}`Kudryavtsev2003`) had a gap in
the gravity‐capillary range (see Fig. 4 in {cite:t}`Kudryavtsev2003`). In 2013, the spectrum was improved using
new experimental data. Data of a full‐scale experiment on stereographic measuring of the two‐dimensional
spectrum with wavelengths from several millimeters to several centimeters are presented in the paper
by {cite:p}`Yurovskaya2013`. Based on the data of this experiment, the Kudryavtsev spectrum was modified.
The measurements were carried out at wind speeds $U_{10}$ from 5 to 15 m/s. The wind fetch in the wave model is taken into account in the same way as in the 
{cite:p}`Elfouhaily1997` model (see section on Low-wavenumber spectrum in Appendix C). The long‐wavelength part of the spectrum is 
calculated by the model given in {cite:t}`Donelan1985`. The transition between long‐ and short‐wavelength parts of the spectrum is described by the same exponential factor 
$F_{p}$ which is provided in Appendix C as it was in {cite:p}`Elfouhaily1997`. 
The spatial spectrum of curvatures is presented as:

$$
B(\vec{k},U_{10})=B_{lw}(\vec{k},U_{10},c_p/U_{10})\times F_{p} + B_{sw}(\vec{k},U_{10})\times (1-F_{p})
$$ (eq29)


where $B_{lw}$ and $B_{sw}$ are the respective curvature spectrum contributions from low and high wavenumbers and where $c_p/U_{10}$ is the wave age. 
The result of solving the energy balance equation is an asymmetric spectrum of surface waves, which shows
that downwind propagation of a small ripple is more likely than the upwind one. The long‐wavelength spectrum
in the paper by {cite:t}`Donelan1985` also has an asymmetric spreading function.
A detailed description of the spectrum can be found in {cite:t}`Yurovskaya2013`. We provide how the main parts of the spectral model 
for $B_{lw}$ and $B_{sw}$ were derived in [Appendix C](#a-name-appendixc-a-appendix-c-sea-surface-height-spectral-model).




##### Small-Scale Perturbation Model


Following the initial formulation of the small-scale perturbation model by {cite:t}`yueh1997`, a computationally efficient and
physically intuitive reformulation of the model by {cite:t}`john:zhang99` was developed and is used in this algorithm.
In this model, the local surface emissivity $e_l$ can be calculated based on the Fresnel reflection coefficients incorporated into the small-scale surface emissivity perturbation model
 ({cite:t}`yueh1997`, 1997; {cite:t}`Johnson2006`). By this process, the local sea surface emissivity vector can
be written as a sum of Fresnel emissivities and the emissivity
perturbation $\Delta \bar{e_{ss}}$ caused by small-scale roughness, namely,

$$
\bar{e_{l}}=
\left(
\begin{bmatrix}
1-|R_{vv}^{(0)}|^2 \\
1-|R_{hh}^{(0)}|^2 \\
0 \\
0 \\
\end{bmatrix}
-\Delta \bar{e_{ss}}
\right)
$$ (eq30)

where $R_{vv}^{(0)}$ and $R_{hh}^{(0)}$ are the Fresnel reflection coefficients
at vertical and horizontal polarizations, respectively. 

Sea surface microwave emission for a given frequency and viewing geometry depends upon the sea water dielectric constant as well as sea surface roughness and sea foam. 
From detailed analyses presented in {cite:t}`john:zhang99`, the sea roughness contribution to the surface emissivity $\Delta \bar{e_{ss}}$ can be treated as the product of an electromagnetic weighting function and the sea surface roughness spectrum integrated over all surface wavelengths. The weighting function shows resonance peaks in the neighborhood of surface wavelengths with scales comparable to the electromagnetic wavelength (i.e., 21 cm for the L-band):
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
$$ (eq31)

where $k_l$ and $k_u$  represent the lower and upper cutoff spectrum
wavenumbers for short ocean waves. The functions $g_i$ (i = v,
h, U, and V ) are the second-order weighting functions from
the small perturbation method, and the detailed information
and formulation of $g_i$ are provided in  [Appendix D](#a-name-appendixd-a-appendix-d-spm-polarimetric-bistatic-coefficients).

##### Hdrodynamic modulation factor

As discussed previously, $\bar{e_{l}}$ can be expressed in terms
of Fresnel reflectivities with small-scale emissivity perturbations.
It is well-known that short gravity-capillary waves are
statistically concentrated on the leeward side of large gravity
waves ({cite:t}`yueh1997`,{cite:t}`Gasiewski1994`,{cite:t}`Johnson2006`). Accordingly, a hydrodynamic modulation
factor *h* to describe this effect can be introduced following {cite:t}`LeeGasiewski2022` as a function of the ratio between upwind surface slope $s'_x$ and upwind rms
surface slope $s_u$, that is,

$$
h=\left(1-h_{a}\tanh{\left(\frac{s'x}{s_u}h_b\right)}\right)
$$ (eqHM1)

where $s_u$ is the standard deviation of the upwind slope,
as defined in [Equation 26](#equation-eq27). The coefficients $h_a$ and $h_b$ determine the
shape and magnitude of the modulation and are empirically tuned parameters with default initial values of 1.0. The
utility of the hyperbolic tangent hydrodynamic modulation
function [Equation 26](#equation-eqHM1) stems from its ability to impose
the necessary up/downwind asymmetry on the capillary wave
emission spectrum. For positive $h_a$ and $h_b$, gravity capillary waves
on the leeward side of gravity waves are enhanced, with ([Equation 29](#equation-eq30)
becoming

$$
\bar{e_{l}}=
\left(
\begin{bmatrix}
1-|R_{vv}^{(0)}|^2 \\
1-|R_{hh}^{(0)}|^2 \\
0 \\
0 \\
\end{bmatrix}
-\Delta \bar{e_{ss} h}
\right)
$$ (eq30b)

Finally, the effective ocean surface emissivity vector for the
four Stokes components is calculated according to [Equation 20](#equation-eq21) and [Equation 23](#equation-eq24).

#### Two scale Roughness-induced emissivity model tuning using Aquarius and SMAP observations 

Satellite observations of Aquarius and SMAP were used for model development and validation. The joint U.S./Argentinian Aquarius/SAC-D mission started from
June 10, 2011 with data available starting at the end of August 2011,
and has ended by June 7, 2015 ({cite:t}`LeVine2018`). Its primary science
objectives were well achieved ({cite:t}`meissner2018salinity`). During the period
of service, Aquarius radiometer (working at 1.413 GHz) continuously
measured the first three Stokes parameters of microwave radiation from
the ocean surface. Three anenna beams operating at incidence angles of
about 29$^{\circ}$, 38$^{\circ}$ and 46$^{\circ}$, respectively, were used for data acquisition
({cite:t}`yueh2014aquarius`). The Aquarius and SMAP V5 GMFs ({cite:t}`meissner2018salinity`) are used here for validation of our TSM.
For the validation of our TSM shown in the figure below, note that the old {cite:t}`Kudryavtsev1999`'s spectrum has been used and only a gaussian large scale slope PDF has been used.


```{figure} GMF_vs_TSM_nofoam.png
--- 
name: GMF_vs_TSM
---
Isotropic Excess ocean emissivities as a function of the wind speed at three Aquarius (column 1–3) incident beams for horizontal (red color) and vertical (blue color) polarizations. The curves with filled and open circles represent the satellite observations and TSM model results without foam, respectively. The temperature and salinity of sea surface in simulations are 20°C and 35 psu, respectively. 
```


As observed, the proposed TSM in its basic configuration, i.e., with untuned model results generally well match
 the isotropic roughness impact on emissivty at L-band as reported from Aquarius and SMAP observations,
  for all three beams and associated incidence angles. In order to predict more accurate zeroth-, first-, and
second-harmonic emissivities, the hydrodynamic modulation
function, ocean foam coverage and emissivity models, and the small-scale emissivity perturbation
model will be tuned to the SMAP and Aquarius  emissivity vector harmonic database over 0–15-m/s wind speeds using a
some tunable parameters, all of which are independent of wind speed and will be described in version 2 of this ATBD.

#### Functionnal flow diagram

```{figure} TSM_Flowchart.png
--- 
name: TSM_Flowchart
---
Flowchart of the two-scale microwave rough ocean surface emissivity model used in this ATBD. Sketch adapted from {cite:t}`LeeGasiewski2022`.  
```

### Sea Foam emissivity modelling at L-band ###

#### Physics of the problem ####



```{figure} foamlayersketch.png
--- 
name: foamlayersketch
---
Geometrical configuration for thermal emission from foam covered ocean. The foam layer is region
1 and is absorptive and scattering. Region 2 is air bubbles embedded in sea water and is absorptive. (From {cite:t}`guo2001applications`)
```


Although foam generated by breaking waves typically covers only a few percent of the sea surface, it has a profound effect on the average microwave brightness of the ocean surface ({cite:t}`droppleman1970`). 
For surface wind speeds greater than 15 m/s, foam-induced effects may provide as much as half of the total sea surface signature to an orbiting microwave radiometer  ({cite:t}`https://doi.org/10.1029/96JC03760`).
At L-band, WISE ({cite:t}`Camps2004`) and FROG ({cite:t}`Camps2005a`) experiments have provided detailed L-band emissivity measurements of the sea foam over a wide range of incidence angles and salinities at both polarizations. 
Although foam as a weaker impact at 1.4 GHz than at higher frequencies, it was shown that the presence of foam also increases the emitted brightness temperature at L-band, since it acts as a transition layer that adapts the wave 
impedance of the two media: water and air. The increase depends on the fraction of the sea surface covered by foam and its thickness, which can be parameterized in terms of the local wind strength, but it depends as well on other 
factors, such as the air-sea temperature difference, the sea water temperature, the fetch, etc… FROG 2003 experiments revealed that at a salinity of 37 psu, the foam-induced emissivity increase is ~0.007 per mm of 
foam thickness (extrapolated at nadir), increasing with increasing incidence angles at vertical polarization, and decreasing with increasing incidence angles at horizontal polarization.  
According to the model developed by {cite:t}`Reul2003`, for a 12 m/s wind speed, one should expect in average a coverage-weighted foam thickness of about 0.5 cm: this translates to an increase in brightness temperature of about 0.2 K at 
an SST of about 15°C. At 20 m/s, the calculation predicts a 0.5 K increases: this might have a non negligible impact for salinity retrieval accuracy in high wind.

In {cite:t}`Camps2005a`, it was shown that the emissivity model proposed by {cite:t}`Reul2003` correctly predicts the measured foam emissivities at L-band provided some auxiliary parameter describing the foam-water system are tuned. 
The purpose of this section is to document this forward foam emissivity model, which is used here to provide foam impact corrections in this version of the SSS retrieval algorithm.
As proposed by {cite:t}`Reul2003`, foam formations contribute to the total sea surface brigthness temperature  measured by a radiometer as function of wind speed $U_{10}$ following:

$$T_{foam,p}=T_{foam}\cdot \int e_{foam,p}^{typ}(f,p,\theta_i,t_{foam}) F_f(U_{10},t_{foam}) d t_{foam} $$ (eq32)

where

- $f$, $p$ and $\theta_i$ are the receiving electromagnetic frequency, polarization and incidence angle of the radiometer respectively,

- $F_{f}(U_{10},t_{foam})$ is the fraction of sea surface area covered by whitecaps with thickness $t_{foam}$ at 10-meter height wind speed $U_{10}$,

- $T_{foam}$ is the physical tempearture of foam, usually assumed the same as the bulk sea  surface temperature: $T_{foam}=T_s$ and,

- $e_{foam,p}^{typ}$  is the emissivity of typical sea foam-layer with thickness $t_{foam}$.

This model is used in the present algorithm to provide foam impact corrections for CIMR. It contains two submodels: one to parametrize the emissivity of typical sea foam-layer with thickness $e_{foam,p}^{typ}$, 
namely and the second to model the fraction $F_f(U_{10},\delta)$ of sea surface area covered by whitecaps with thickness $\delta$ at 10-meter height wind speed $U_{10}$. Both of them are successively detailed hereafter.

#### Brightness temperature modeling of the foam-water system #####


Foam and whitecaps belong to the class of colloidal systems that
include two phases: atmosphere gases and sea water. In the present
work, we model foam layers at the ocean surface as media of
densely packed sticky spherical air bubbles, coated with thin
seawater coating following the approach of {cite:t}`guo2001applications`. The dipole approximation model developed by
{cite:t}`dombrovskiy1992microwave` is then used to
describe the effective permittivity of the system.

Following  {cite:t}`guo2001applications`, we assumed that foam on
the ocean surface is composed of nearly spherical coated bubbles
described by an outer radius $r$, made of an air core with
permittivity $\varepsilon_a$, surrounded by a shell of sea water
with thickness $\delta$ and permittivity $\varepsilon_w$. The foam
covered ocean is modeled by the succession of three media (see {numref}`foamlayersketch`):
- the air (region 0), 
- a foam layer defined as a region of effective permittivity $\varepsilon_{N\alpha}$ with a layer thickness $t_{foam}$
(region 1), and,
- the underlying seawater with some air bubbles (region 2) with permittivity $\varepsilon_{W}$.

 Boundaries between each region are assumed flat.

The brightness temperature of the foam-water system at incidence angle $\theta_i$ and polarization $p=h$ (horizontal) or $v$
(vertical) is then equal to

$$
 e_{foam,p}^{typ}(f,\theta_i,t_{foam})=1-|R_{foam,p}^{typ}(f,\theta_i,t_{foam})|^2
$$ (eq33)

where $T_s$ is the foam layer physical temperature, the
coefficient $R_{foam,p}^{typ}$ is the spectral reflection coefficient of
the  foam layer medium with the effective dielectric constant
$\varepsilon_{N\alpha}$ and is given by:

$$
R_{foam,p}^{typ}(f,\theta_i,t_{foam})=\displaystyle\frac{R_{01}^{p}(\theta_i)e^{-2i\psi}+R_{12}^{p}(\theta_i)}
{e^{-2i\psi}+R_{01}^{p}(\theta_i)R_{12}^{p}(\theta_i)}
$$ (eq34)

where $\psi$ is an attenuation factor that depends on the foam
layer thickness $t_{foam}$, the electromagnetic wavelength $\lambda_o$, and
the effective permittivity $\varepsilon_{N\alpha}$:

$$
\psi=\frac{2\pi t_{foam}}{\lambda_o} \sqrt{\varepsilon_{N\alpha}-sin^2
\theta_i}
$$ (eq35)

where $\lambda_o$ is the electromagnetic wavelength.  $R_{01}^{p}$ are the
Fresnel reflection coefficients between air (region 0) and foam
(region 1):

$$
 R_{01}^h(\theta_i)=\displaystyle\frac{\cos(\theta_i)-\sqrt{\varepsilon_{N\alpha}-\sin^2(\theta_i)}}{\cos(\theta_i)+\sqrt{\varepsilon_{N\alpha}-\sin^2(\theta_i)}}
$$ (eq36)

$$
R_{01}^v(\theta_i)=\displaystyle\frac{\varepsilon_{N\alpha}\cos(\theta_i)-\sqrt{\varepsilon_{N\alpha}-\sin^2(\theta_i)}}{\varepsilon_{N\alpha}\cos(\theta_i)+\sqrt{\varepsilon_{N\alpha}-\sin^2(\theta_i)}} \\
$$ (eq37)

and $R_{12}^{\gamma}$ are the Fresnel reflection coefficients
between foam (region 1) and water (region 2):

$$
\begin{array}{c}
 R_{12}^h(\theta_i)=\displaystyle\frac{\sqrt{\varepsilon_{N\alpha}-\sin^2(\theta_i)}-\sqrt{\varepsilon_{w}-\sin^2(\theta_i)}}{\sqrt{\varepsilon_{N\alpha}-\sin^2(\theta_i)}+\sqrt{\varepsilon_{w}-\sin^2(\theta_i)}}\\
 \\
 R_{12}^v(\theta_i)=\displaystyle\frac{\varepsilon_{w}\sqrt{\varepsilon_{N\alpha}-\sin^2(\theta_i)}-\varepsilon_{N\alpha}\sqrt{\varepsilon_{w}-\sin^2(\theta_i)}}{\varepsilon_{w}\sqrt{\varepsilon_{N\alpha}-\sin^2(\theta_i)}+\varepsilon_{N\alpha}\sqrt{\varepsilon_{w}-\sin^2(\theta_i)}}\\
\end{array}
$$ (eq38)

Region 2 consists of air bubbles embedded in the ocean background
and is assumed to be absorptive. To solve the previous system of
equations, one need to define an effective permittivity for region
1, namely $\varepsilon_{N\alpha}$, and for region 2, namely
$\varepsilon_{w}$.

#### Effective permittivity of sea Foam formations at L-band #####

The main parameter of the previous multi-layer emissivity model
for foam is the effective permittivity $\varepsilon_{N\alpha}$ of
the foam-layer considered. To define this parameter, the
well-known, from molecular optics, $\it{Lorenz-Lorentz}$ and
$\it{Hulst}$ equations can be used and modified for the
polydispersed system of bubbles. The first formula take into
account $\it{dipole}$ $\it{interaction}$ of bubbles in a close-packed
dispersed system (the quasi static approximation). The Hulst
equations describe the contribution of the $\it{multipole}$ $\it{moment}$
of bubbles into effective permittivity of the system. Spectral
calculations by {cite:t}`cherny1998passive` show that
first resonant electromagnetic effects by Hulst's mechanism occur
for bubbles radius $a\sim\lambda_o/4$. At L-band $(\lambda_o=21
cm)$, this corresponds to bubble diameters of order 10 cm. Such
very large bubbles are extremely rare at the sea surface and therefore, the
multipole mechanism may be  neglected at L-band  for
which  bubbles might be considered dipole only. In the present
algorithm, we use the dipole approximation model developed by
{cite:t}`dombrovskiy1992microwave` to describe the
effective permittivity of the system. It involves use of a
modification of the Lorenz-Lorentz equation and yields the
following simple formula for the complex effective permittivity
$\varepsilon_{N\alpha}$ of a foam-layer :

$$
    \varepsilon_{N\alpha}(z)=\displaystyle\frac{1+\displaystyle\frac{8}{3}\pi\overline{N\alpha(z)}}{1-\displaystyle\frac{4}{3}\pi\overline{N\alpha(z)}}
$$ (eq39)
    
where
    
$$
    \overline{N\alpha(z)}=\displaystyle\frac{\kappa\displaystyle\int\alpha(r)p_f(r,z)dr}{\displaystyle\frac{4}{3}\int
r^3p_f(r,z)dr}
$$ (eq40)

where $N$ is the volume concentration of the bubbles,
$\alpha(r)$ is the complex polarizability of a single bubble with
external radius $r$, $\kappa$ is the so-called stickiness
parameter and $p_f(r,z)$ is the normalized size-distribution
function of the bubbles as function of depth $z$. In natural media such as foam, the
densely packed particles can have adhesive forces that make them
adhere to form aggregates. This effect is accounted for in the
model through the introduction of the $\it{stickiness}$ $\it{parameter}$
\textbf{$\kappa$}, also called packing coefficient of the bubbles
which is inversely proportional to the strength of the attractive
force between bubbles ({cite:t}`Zurk:95`).
According to {cite:t}`dombrovskiy1992microwave`, the
complex polarizability depends on the external radius of bubbles
$r$, the complex permittivity of the shell medium (salt water)
$\varepsilon_w$, and the bubble's filling factor $q=1-\delta/r$
following:

$$
    \alpha(r)=r^3\displaystyle\frac{(\varepsilon_w-1)(2\varepsilon_w+1)(1-q^3)}{(\varepsilon_w+2)(2\varepsilon_w+1)(1-q^3)+9\varepsilon_w q^3}
$$ (eq41)

 where $q=1-\delta/r$ is the so-called bubble's filling factor.
Experimental measurements on stable foam reveal that the effective
permittivity might be dependent on the vertical position within
the foam layer, i.e,
$\varepsilon_{N\alpha}=\varepsilon_{N\alpha}(z)$. In the simplest
case, the foam-water system may be modelled as a succession of
elementary foam-layers, each of them having an homogeneous
effective dielectric constant. However, the exact dependence of
such function with vertical, which depends on the vertical
distribution of the bubble's size is very poorly known. It is very
likely that the vertical distribution of the bubble's size
$p_f(r,z)$ is function of the intensity and scale of the
underlying breaking event. Moreover it will certainly strongly
evolve during a transient breaking event. Nevertheless, in order
to keep a tractable number of parameters in the present model, we
choose to consider a uniform vertical distribution of bubbles
sizes $p_f(r,z)=p_f(r)$ within the foam layer.

 The foam void fraction (i.e., the ratio of the volume of air to the total
volume of the foam) depends on the distribution of the bubble's
filling factor $q$. The distribution of bubbles radii
$p_f(r)$ together with the distribution of coating thicknesses
$f(\delta)$ therefore determines the foam layer void fraction.  In
the present simplified model, we fixed the value of the shell
thickness $\delta$ but  the outer bubble radius $r$ is randomly
distributed. According to {cite:t}`dombrovskii1979calculation`, this
approximation reflects an experimentadombrovskii1979calculationlly established fact for an
emulsion layer of foam (young foam) but it requires verification
for a foam with honeycomb structure (aged foam). Numerous
observations of oceanic bubble size distributions are reported in
the literature based on acoustic, photographic, optical, and
holographic methods [e.g., see {cite:t}`wu1988variations`]. Currently, it
is not clear how to parameterize the ocean surface bubble size
distribution. Following {cite:t}`dombrovskii1979calculation` and
{cite:t}`dombrovskiy1992microwave`, we used a $\gamma$
distribution for the size distribution function of the bubbles:

$$
p_f(r)=\frac{A^{B+1}}{\Gamma(B+1)}r^B e^{-Ar}
$$ (eq42)

where $A$ and $B$ are parameters of the distribution with
$r_p=A/B$ being the most probable radius.

Finally, to calculate $\varepsilon_{w}$, a simple physical model based on
induced dipoles is used. Let $\varepsilon_{sw}$ denote the
permittivity of the seawater, $f_a$ the fractional volume occupied
by the air bubbles. Then the effective permittivity
$\varepsilon_{w}$, is given by the Maxwell-Garnett mixing formula
(see {cite:t}`guo2001applications`):

$$
\varepsilon_w=\varepsilon_{sw}\frac{1+2f_a y}{1-f_a y}
$$ (eq43)

where

$$
y=\frac{1-\varepsilon_{sw}}{1+2\varepsilon_{sw}} \\
$$ (eq44)

with $\varepsilon_{sw}$, the sea-water permittivity, derived for
L-band using the GW2020's model. Note that
the effective permittivity $\varepsilon_{w}$ here does not include
scattering attenuation which is small due to the fact that the
seawater is heavily absorptive.

According to our simplified model, the emissivity induced by a typical sea foam layer at L-band is a function of:

$$
e_{foam,p}^{typ}=function(\theta_i,f,p,T_s,r_p,\delta,\kappa,t_{foam},f_a,SSS,SST)
$$ (eq45)

where $\theta_i$ is the radiometer incidence angle, $f$ the E.M.
frequency, $p$ is an index denoting the signal Polarization, $T_s$
is the foam physical temperature, $r_p$ is the average bubble
radius, $\delta$ is the  bubble's water coating thickness,
$\kappa$ is the bubble's packing coefficient, $t_{foam}$ is the foam
layer thickness, $f_a$ is the void fraction beneath the foam
layer, and finally, $SSS$ and $SST$ are the sea surface salinity
and temperature respectively.

#### Foam coverage model #####

In {cite:t}`Reul2003`, it was shown that the fractional sea surface covered by foam-layers with thicknesses between $h$ and $h+dh$ at wind speed $U_{10}$, namely, the term $F(U_{10},h)dh$, can be decomposed as follows:

$$
dF(U_{10},h)=F(U_{10},h)dh= dF_c(U_{10},h)+ dF_s(U_{10},h)
$$ (eq46)

where $dF_c$ and $dF_s$ are the contributions to the coverage of actively breaking crests or active foam and of the passive foam, or static-foam formations (see {cite:t}`Monahan1986` for detailed terminology), respectively.

The model which is used for these two terms is a modified form of that described in detail in {cite:t}`Reul2003`, where the following empirical expression for $dF_c(U_{10},h)$ was derived:

$$
dF_c(U_{10},h)=[2.9\times 10^{-5}\cdot U_{10}^{3} \cdot \sqrt{h}\cdot e^{-4.48\sqrt{h}}  dh]\times e^{\alpha_c\Delta T-\beta_c}	
$$ (eq47)

where $U_{10}$ is the 10-meter height wind speed, $\alpha_c$ and $\beta_c$ are numerical constants and $\Delta T$ is the air-sea surface temperature difference. Instead of using this form directly, however, we begin with empirical distribution functions for foam-generating breaker length per unit area per unit breaker speed interval as derived in [13] and then generalize these equations to accommodate improvement in the foam coverage distributions.

The breaker length distribution function is a modified form of that derived from measurements of {cite:t}`Melville2002`,

$$
\Lambda(U_{10},c)=\displaystyle\tilde{A}\cdot\left(\frac{U_{10}}{10}\right)^3\times 3.3\times10^{-4}\cdot e^{\displaystyle -0.64\tilde{B}\left(\frac{c}{U_{10}}\right)}
$$ (eq48)

where $\tilde{A}$  and $\tilde{B}$  are constants to be specified. This distribution function differs from the empirical form of {cite:t}`Melville2002` in that the exponent is a function of wave age rather than breaker phase speed.

Using the preceding formulation of the crest length distribution function, we can write the crest and static foam incremental coverages in terms of wind speed $U_{10}$, breaker phase speed $c$c, and air-sea temperature difference $\Delta T$, respectively, as:

$$
dF_c(U_{10},c,\Delta T)=\displaystyle\left[\frac{2\pi a_{1}}{g}c^2\Lambda(U_{10},c)dc\right]\times e^{\alpha_c\Delta T-\beta_c}	
$$ (eq49)

$$
dF_s(U_{10},c,\Delta T)=\displaystyle\left[\frac{2\pi a_{2}}{g}c^2\Lambda(U_{10},c)dc\right]\times e^{\alpha_s\Delta T-\beta_s}	
$$ (eq50)

The final exponentials in the two previous equations are atmospheric stability correction factors, which have a significant impact on the foam coverage. The free parameters in these correction factors are given fixed values. 
The constants $a_{1}$  and $a_{2}$  in the above equations are constants that reflect the persistence time of the foam layers, which is typically much larger for static than for crest foam.

In the modified formulation, we note that the incremental foam fractional coverage for both static and crest foam is a function of

- generating breaking front speed $c$,

- the 10 m wind speed  $U_{10}$, and

- the air-sea temperature difference $\Delta T$.

The parameters $\alpha_c$ and $\beta_c$  of the thermal correction factors were determined in {cite:t}`Reul2003` for both 'crest-foam' and 'static-foam' by best fitting the model to {cite:t}`Monahan1989`'s empirical laws. Using a least-square method, the determined numerical values for  $\alpha$ and $\beta$ are: $\alpha_c$= 0.198 and  $\beta_c$  = 0.91 for 'crest-foam coverage', and $\alpha_s$= = 0.086 and  $\beta_s$= 0.38 for 'static-foam coverage'.

To compute the total contribution of foam to the measured brightness temperature, we must determine the distribution of foam as a function of characteristic foam thickness, 
from which time dependence has been removed by assuming that foam layers associated with fronts moving at a given speed have equal probability of being at any stage of development. Using this assumption together with a simple model for the time dependence of foam layer depth, we obtain for crest foam the depth

$$
\overline{\delta}_{\tau^{\star}}=\displaystyle\frac{0.4c^2}{g}
$$ (eq51)

and for static foam, we obtain:

$$
\overline{\delta}_{\tau}=\displaystyle\frac{0.4c}{2\pi a}\left[\frac{5c}{2g}+\tau'\cdot\left(1-e^{\frac{c}{g\tau'}(2\pi a-5)}\right)\right]
$$ (eq52)

In the above equations, $g$ is the acceleration of gravity and $c$ is the breaker phase speed, and $\tau'$  is the exponential decay time of the foam depth after
 the mean duration time of the breaking events (nominally taken to be 3.8 s for salt water). These expressions can be used to transform the differential foam 
 coverage expressions into expressions for the incremental coverage per unit foam thickness.



### Atmospheric contributions at L-band  


#### Physics of the problem

In the L-band, the effect of atmospheric scattering as well as clouds, and water vapor attenuation are all small thanks to relatively long electromagnetic wavelength (~21 cm) in comparison with typical atmospheric hydrometeor 
size ({cite:t}`yueh2001error`). However, the effect of atmospheric emission must be corrected to achieve accurate salinity retrievals. This contribution includes both upwelling and downwelling components of atmospheric radiation
 (the latter is reflected from the surface).  The atmosphere emits and absorbs L-band radiation mostly due to molecular oxygen (O2) with small contributions from water vapor, cloud liquid water, and rain
  ({cite:t}`blume1978measurement`; {cite:t}`ulaby1982microwave`; {cite:t}`blume1982passive`, {cite:t}`swift1983considerations`, {cite:t}`rosenkranz1998water`). 
  The net atmospheric emission varies from about 2.4 to 2.8 K and is proportional to atmospheric pressure ({cite:t}`yueh2001error`). The approach adopted to correct SMOS, Aquarius, and SMAP, $T_B$ for atmospheric impacts 
  consists in using the conventional radiative transfer theory to model atmospheric attenuation and emission (e.g., {cite:t}`liebe1992atmospheric`). Atmospheric temperature and pressure profiles are needed in the transfer 
  model and are usually obtained from atmospheric forecast models such as those provided by the ECMWF, or the NCEP. In general, the atmospheric impacts in the L-band can be corrected to within 0.3 K error 
  ({cite:t}`meissner2018salinity`). To derive atmospheric contribution at L-band, we shall rely on:

-	Auxilliary atmospheric parameters (pressure, temperature, relative humidity, cloud water,..) are obtained from ECMWF atmospheric model 3-hourly fields at multiple altitudes.

-	Attenuation rates and atmospheric absorption are estimated on N-layers along the vertical from ground to satellite altitude using the Milimeter Wave Propagation Model and finally 
integrated along the path from target to satellite.

##### Electromagnetic Wave Propagation through the Atmosphere

At L-band, the attenuation of radiation propagating through the
atmosphere is mostly associated wth absorption, so we will assume that
scattering is negligible.  To begin the development, the electric
field components in the plane normal to the propagation direction may
be written as

$$
\begin{eqnarray}
E_h(r,t) &=& \Re\left\{\hat{E}_h(r,t) \right\} = \Re\left\{a_h e^{i(k r - \omega t - \phi_h)}\right\},\\
E_v(r,t) &=& \Re\left\{\hat{E}_v(r,t) \right\} = \Re\left\{a_v e^{i(k r - \omega t - \phi_v)}\right\},
\end{eqnarray}
$$ (eq53)

where $\phi_h$ and $\phi_v$ are constant phases, $r$ is distance along
the propagation path, $k$ is wavenumber magnitude, $\omega$ is the
radiation frequency, $t$ is time, and the real amplitudes $a_h(r)$ and
$a_v(r)$ are functions of propagation distance $r$.  In general as the
radiation propagates through the atmosphere it experiences changes in
the dielectric properties of the medium.

We will assume that the amplitude coefficients satisfy the simple
ordinary different equations

$$
\begin{eqnarray}
\frac{d a_h}{dr} = -b(r)a_h(r),\label{eq_dah}\\
\frac{da_v}{dr} = -b(r)a_v(r).\label{eq_dav}
\end{eqnarray}
$$(eq54)

where $r$ is distance along propagation path.  In reality, the
amplitude attenuation rate $b(r)$ is a function of space and time and
may be computed using molecular oxygen ($O_2$) and water vapor
concentration profiles along with atmospheric physical temperature and
pressure profiles. The attenuation rate, or attenuation coefficient,
$b$ (typically expressed in units of nepers per meter), generally is a
complicated function of atmospheric state (i.e. pressure, temperature,
water content in various forms, etc.).  Multiplying first equation in ([53](#equation-eq54)) by
$a_h$ and second by $a_v$ and rearranging yields

$$
\begin{eqnarray}
\frac{d a_h^2}{dr} = -2 b(r)a_h^2(r),\label{eq_dah2}\\
\frac{d a_v^2}{dr} = -2 b(r)a_v^2(r).\label{eq_dav2}
\end{eqnarray}
$$ (eq55)

Similarly, multiplying first equation in ([53](#equation-eq54))  by $a_v$ and second by $a_h$ and adding yields

$$
\begin{eqnarray}
\frac{d a_va_h}{dr} = -2 b(r)a_va_h(r).\label{eq_davh}
\end{eqnarray}
$$ (eq56)

If the attenuation rate $b$ is independent of spatial location and
time, and if we let the electric field amplitudes at some reference
location (at which $r=0$) be $a_{vo}$ and $a_{ho}$, then

$$
\begin{eqnarray}
a_h(r) = a_{ho}e^{-b r},
a_v(r) = a_{vo}e^{-b r},
\end{eqnarray}
$$ (eq57)

so that

$$
\begin{eqnarray}
E_h(r,t) &=& \Re\left\{\hat{E}_h(r,t) \right\} = \Re\left\{a_{ho} e^{i(k r - \omega t - \phi_h) - b r}\right\},\\
E_v(r,t) &=& \Re\left\{\hat{E}_v(r,t) \right\} = \Re\left\{a_{vo} e^{i(k r - \omega t - \phi_v) - b r}\right\}.
\end{eqnarray}
$$ (eq58)

where $a_{ho}$ and $a_{vo}$ are the real amplitudes of the components
at some reference location $r=0$.  We may also represent this
attenuation rate as an imaginary part of the wavenumber magnitude $k$,
so that we let

$$
\begin{eqnarray}
\tilde{k} = k + i b = \tilde{k}_r + i \tilde{k}_i,
\end{eqnarray}
$$ (eq59)

where now $\Re(\tilde{k})$ gives the propagation speed in terms of
$\omega$ while $\Im(\tilde{k})$ gives the attenuation rate.

$$
\begin{eqnarray}
E_h(r,t) &=& \Re\left\{\hat{E}_h(r,t) \right\} = \Re\left\{a_{ho} e^{i(\tilde{k} r - \omega t - \phi_h)}\right\},\\
E_v(r,t) &=& \Re\left\{\hat{E}_v(r,t) \right\} = \Re\left\{a_{vo} e^{i(\tilde{k} r - \omega t - \phi_v)}\right\}.
\end{eqnarray}
$$ (eq60)

The complex wavenumber magnitude $\tilde{k}$ is the key quantity
required to model the radiation propagation (including curvature in
propagation path) and attenuation along the path, and here we use the
Millimeter-wave Propagation model of {cite:t}`Liebe1993` (hereafter referred
to as the MPM93), which is a refined version of the model presented in
{cite:t}`Liebe1989`, to compute $\tilde{k}$ as a function of atmospheric
state along the radiation propagation path. As $\tilde{k}$ depends
mostly on molecular oxygen concentration and, to a lesser extent,
water vapor concentration, the key atmospheric variables required are
dry air pressure and water vapor mixing ratio.
The Millimeter-wave Propagation model provides an explicit expression
for the so-called {\it complex index of refraction}, denoted by $N$,
which is defined so that

$$
\begin{eqnarray}
\tilde{k} = \tilde{k}_r + i \tilde{k}_i = k + i b = k + k N = k + k (N_0 + N'_0) + i N'',
\end{eqnarray}
$$ (eq61)

where $N_0$, $N'$, and $N''$ are all real quantities that depend upon
the atmospheric state.  $N_0$ is independent of frequency while $N'$
and $N''$ depend on frequency, in general. The real real and imaginary
parts of complex index of refraction are related to the real and
imaginary parts of the complex wavenumber by the equations

$$
\begin{eqnarray}
\tilde{k}_r &=& k \left(1 + N_0 + N'\right),\\
\tilde{k}_i &=& b(r) = k N''.
\end{eqnarray}
$$ (eq62)

Thus, $N''$ corresponds to the loss portion of $\tilde{k}$ while
$N_0+N'$ corresponds to the propagation speed portion of $\tilde{k}$.
It follows that the corresponding *amplitude* attenuation rate,
$b(r)$, expressed in nepers per meter, is

$$
\begin{eqnarray}
b(r) = kN'' = \left(\frac{2\pi \nu}{c}\right) N'' \quad \mbox{[Np m$^{-1}$]},
\end{eqnarray}
$$ (eq63)

and the *power* attenuation rate is

$$
\begin{eqnarray}
a(r) = 2b(r) = 2kN'' = \left(\frac{4\pi \nu}{c}\right) N'' \quad \mbox{[Np~m$^{-1}$]},
\end{eqnarray}
$$ (eq64)

where $c$ is the speed of light in a vacuum (m~s$^{-1}$) and $\nu$ is
the electromagnetic frequency in hertz.  The index of refraction as
provided by the MPM93, $\tilde{N}=\tilde{N}_0+\tilde{N'}+\tilde{N}''$,
is expressed in ppm (i.e., it is scaled by $10^6$) so that for $b(r)$
in nepers per meter, we have

$$
\begin{eqnarray}
\tilde{k}_r(r) = b(r) = kN'' = \left(\frac{2\pi \nu}{c}\right) \cdot 10^{-6} \cdot
\tilde{N}''
\quad
\mbox{[Np m$^{-1}$]},
\end{eqnarray}
$$ (eq65)

or for $b(r)$ in dB km$^{-1}$ with $\nu$ in gigahertz,

$$
\begin{eqnarray}
b(r) = kN'' = \left(\frac{2\pi \nu}{c}\right)
\left(\frac{10}{\log_e(10)}\frac{\mbox{dB}}{\mbox{Np}}\right)
\left(10^3\frac{\mbox{m}}{\mbox{km}}\right)
\left(10^9\frac{\mbox{Hz}}{\mbox{GHz}}\right)
\cdot 10^{-6}
\cdot \tilde{N}''
\approx
0.0910\nu\tilde{N}''
\quad
\mbox{[dB km$^{-1}$]}.
\end{eqnarray}
$$ (eq66)

The real part of the complex wavenumber, $\tilde{k}_r(r)$, is

$$
\begin{eqnarray}
\tilde{k}_r(r)
=
\frac{360}{2\pi}
\left(\frac{\mbox{deg}}{\mbox{rad}}\right)
\left(\frac{2\pi\nu}{c}\right)
\left(10^9\frac{\mbox{Hz}}{\mbox{GHz}}\right)
\left(10^3\frac{\mbox{m}}{\mbox{km}}\right)
\left[1 + \cdot 10^{-6} \cdot \left(\tilde{N}_0 + \tilde{N}'\right) \right]
\quad
\mbox{[deg km$^{-1}$]}.
\end{eqnarray}
$$ (eq67)

The variable part of the preceding expression for $\tilde{k}_r(r)$ is
called the *phase dispersion*, $\beta$, and is approximately

$$
\begin{eqnarray}
\beta
=
1.2\nu\left(\tilde{N}_0 + \tilde{N}'\right)
\quad
\mbox{[deg km$^{-1}$]},
\end{eqnarray}
$$ (eq68)

with $\nu$ expressed in gigahertz and $\tilde{N}_0$ and $\tilde{N}'$
expressed in ppm.  From  $\tilde{k}_r$ we can derive the wave
propagation speed $\tilde{c}(r)$:

$$
\begin{eqnarray}
\tilde{c}(r)
=
1000
\cdot
\frac{\omega}{\tilde{k}_r(r)}
\left(\frac{\mbox{deg~s$^{-1}$}}{\mbox{deg km$^{-1}$}}\right)
=
1000
\cdot
\frac{360\nu}{\tilde{k}_r(r)}
\left(\frac{\mbox{deg~s$^{-1}$}}{\mbox{deg km$^{-1}$}}\right)
=
c
\left[1 + 10^{-6} \cdot \left(\tilde{N}_0 + \tilde{N}'\right) \right]^{-1}
\quad
\mbox{[m s$^{-1}$]},
\end{eqnarray}
$$ (eq69)

where as before $c$ is expressed in meters per second and
$\tilde{N}_0$ and $\tilde{N}'$ are expressed in ppm as in the MPM93.
If we were concerned about propagation path curvature, we would need
to use this expression to derive the propagation path for each point
in director cosine coordinates.  But we will assume that at L-band
such curvature effects are negligible.  At L-band, the attenuation
rate $b(r)$ is associated mostly with the long tails in the
distributions of absorption associated with resonant absorption lines
of molecular oxygen and water vapor at much higher
frequencies. Scattering by cloud droplets, ice, and raindrops is
relatively minor at L-band.  Next we will examine the formulation of
$\tilde{N}_0$ and $\tilde{N}'$ in the MPM93 and then relate the total
refractive index $\tilde{N}$ to the Stokes vector, which is our
primary concern.

##### Formulation of the Atmospheric Refractive Index

As formulated in the MPM93, the total index of refraction is the sum
of indices of refraction owing to molecular oxygen ($ \tilde{N}_D$),
water vapor ($ \tilde{N}_V$), and cloud water ($ \tilde{N}_W$) and ice
($ \tilde{N}_I$), so that

$$
\begin{eqnarray}
\tilde{N}(T_r,e,p_d,\nu) = \tilde{N}_0 + \tilde{N}' + i\tilde{N}''
= \tilde{N}_D(T_r,e,p_d,\nu) +\tilde{N}_V(T_r,e,p_d,\nu) + \tilde{N}_W(T_r,\rho_w,\nu) + \tilde{N}_I(T_r,\rho_i,\nu)
\quad\mbox{[ppm]},\nonumber
\end{eqnarray}
$$  (eq70)

where $p_d$ and $e$ are the partial pressures of dry air and water
vapor (hPa), respectively; $\rho_w$ and $\rho_i$ are the liquid water ice
densities (g.m$^{-3}$), respectively; $T_r$ is the *reciprocal
  temperature*, defined by

$$
\begin{eqnarray}
T_r = \frac{300}{T_p},
\end{eqnarray}
$$ (eq71)

where $T_p$ is the physical temperature in kelvin.  The indices of
refraction are expressed in the MPM93 in terms of the reciprocal
temperature, partial pressures of dry air and water vapor (which is a
function of relative humidity and temperature), and cloud water and
ice densities (not mixing ratios).  The molecular oxygen concentration
is a function of the partial pressure of dry air, $p_d$ (expressed in
hPa), which is approximately equal to the total pressure $p_t$ minus
the partial pressure of water vapor $e$:

$$
\begin{eqnarray}
p_d = p_t - e,
\end{eqnarray}
$$  (eq72)

Following {cite:t}`Liebe1989`, we introduce the following approximation to
the water saturation vapor pressure over a plane surface of water:

$$\begin{eqnarray}
e_s(T_r) = 2.408\times 10^{11}T_r^5 e^{-22.644 \cdot T_r},
\end{eqnarray}
$$ (eq73)

Using this approximation, the partial pressure of water vapor in
hectopascals is

$$
\begin{eqnarray}
e(T_r,r) = e_s(T_r)\frac{r}{100} = \frac{r}{100} \cdot 2.408\times 10^{11}T_r^5 e^{-22.644 \cdot T_r},
\end{eqnarray}
$$ (eq74)

where $r$ is the relative humidity in percent. The dry air index of refraction $\tilde{N}_D$ consists of a
nondispersive term $N_d$, a nonresonant term $N_n$, and a sum over
relevant molecular oxygen absorption lines:

$$
\begin{eqnarray}
\tilde{N}_D(T_r,e,p_d,\nu) = \tilde{N}_d + \tilde{N}_n + \displaystyle\sum_{k=1}^{44} S_kF_k
\quad\mbox{[ppm]}.
\label{eq_n_D_tilde}
\end{eqnarray}
$$ (eq75)

The nondispersive term is

$$
\begin{eqnarray}
\tilde{N}_d = 0.2588 p_dT_r
\quad\mbox{[ppm]},
\end{eqnarray}
$$ (eq76)

and the nonresonant term is given by the sum of a relaxation spectrum
term $S_\circ F_\circ$ and a term associated with pressure-induced $N_2$
absorption (which makes a small contribution above 100~GHz), $S_nF_n$:

$$
\begin{eqnarray}
\tilde{N}_n = S_\circ F_\circ + i S_n F_n
\quad\mbox{[ppm]},
\end{eqnarray}
$$ (eq77)

with

$$
\begin{eqnarray}
S_\circ &=& 6.14\times10^{-5} p_d T_r^2\quad\mbox{[ppm]},\\
F_\circ &=& -\nu\left[\nu+i0.56\times10^{-3}(p_d+e)T_r.^{0.8}\right]^{-1},\\
S_n     &=& 1.4\times10^{-12} p_d^2T_r^{3.5}\quad\mbox{[ppm]},\\
F_n     &=& \nu\left[1 + 1.93\times10^{-5}\nu^{1.5}\right]^{-1}.
\end{eqnarray}
$$ (eq78)

In the sum over absorption lines, $\displaystyle\sum_{k=1}^{44}
S_kF_k$, the individual line strengths are given by

$$
\begin{eqnarray}
S_k = 1\times10^{-6}\frac{a_1}{\nu_k}p_dT_r^3\exp{\left[a_2(1-T_r)\right]}\quad\mbox{[ppm]},
\end{eqnarray}
$$ (eq79)

where $a_1$ and $a_2$ are constants for each line. The line form (or
line spreading) factor, known as the Van Vleck-Weisskopf function, is
given by

$$
\begin{eqnarray}
F_k = \nu\left[\frac{1-i\delta_k}{\nu_k-\nu-i\gamma_k} - \frac{1+i\delta_k}{\nu_k+\nu+i\gamma_k}\right],
\end{eqnarray}
$$ (eq80)

with the overlap parameter $\delta_k$ given by

$$
\begin{eqnarray}
\delta_k = 1\times10^{-3}\left(a_5+a_6T_r\right)\left(p_d+e\right)T_r^{0.8},
\end{eqnarray}
$$ (eq81)

where $a_5$ and $a_6$ are constants for each line. The width parameter
$\gamma_k$ is given by

$$
\begin{eqnarray}
\gamma_k = \left(\tilde{\gamma}_k^2 + 2.25 \times 10^{-6}\right)^{1/2}
\quad\mbox{[GHz]},
\end{eqnarray}
$$ (eq82)

with

$$
\begin{eqnarray}
\tilde{\gamma}_k = 1\times10^{-3}a_3\left(p_dT_r^{a_4} + 1.1 e T_r\right)
\quad\mbox{[GHz]},
\end{eqnarray}
$$ (eq83)

where $a_3$ and $a_4$ are constants for each line. The difference
between $\gamma_k$ and $\tilde{\gamma}_k$ is related to the Zeeman
effect in which absorption lines are split into multiple lines.

The index of refraction for water vapor is the sum of a nondispersive
term, a continuum term, and a sum over 35 resonant absorption lines
(including one pseudo-line at 1780~GHz). At L-band the continuum term is negligible so that

$$
\begin{eqnarray}
\tilde{N}_V \approx \tilde{N}_v + \displaystyle\sum_{k=1}^{35}
S^{(v)}_kF^{(v)}_k \quad\mbox{[ppm]}.
\label{eq_n_V_tilde}
\end{eqnarray}
$$ (eq84)

The nondispersive term is

$$
\begin{eqnarray}
\tilde{N}_v(T_r,e,p_d,\nu) = \left(4.163T_r + 0.239\right) e T_r
\quad\mbox{[ppm]}.
\end{eqnarray}
$$ (eq85)

The absorption line strength in the sum over the lines is

$$
\begin{eqnarray}
S^{(v)}_k = \left(\frac{b_1}{\nu_k}\right) e T_r^{3.5}\exp{\left[b_2\left(1-T_r\right)\right]},
\end{eqnarray}
$$ (eq86)

where $b_1$ and $b_2$ are constants defined for each line $k$.  The
line form function has the same form as for oxygen except that there
is no line overlap effect:

$$
\begin{eqnarray}
F^{(v)}_k = \nu\left[\frac{1}{\nu^{(v)}_k-\nu-i\gamma^{(v)}_k} - \frac{1}{\nu^{(v)}_k+\nu+i\gamma^{(v)}_k}\right].
\end{eqnarray}
$$ (eq87)

For a total pressure above 0.7~hPa, the width of a pressure-broadened
vapor line, $\gamma^{(v)}_k$, is given by

$$
\begin{eqnarray}
\gamma^{(v)}_k = 1\times10^{-3} b_3 \left(b_4 e(e_s(T_r),r) T_r^{b_6} + p_dT_r^{b_5}\right)
\quad\mbox{[GHz]},
\end{eqnarray}
$$ (eq88)

where $b_3$, $b_4$, $b_5$ and $b_6$ are constants defined for each
line $k$.  Below 0.7 hPa doppler broadening can be significant, so
that in this case $\gamma^{(v)}_k$ is modified to be

$$
\begin{eqnarray}
\gamma^{(v)}_k = 0.535\tilde{\gamma}^{(v)}_k + \left(0.217\left(\tilde{\gamma}^{(v)}_k\right)^2 + \gamma_D^2\right)^{1.2}
\quad\mbox{[GHz]},
\end{eqnarray}
$$ (eq89)

where $\tilde{\gamma}^{(v)}_k$ is given by

$$
\begin{eqnarray}
\tilde{\gamma}^{(v)}_k = 1\times10^{-3} b_3 \left(b_4 e(e_s(T_r),r) T_r^{b_6} + p_dT_r^{b_5}\right)
\quad\mbox{[GHz]}
\end{eqnarray}
$$ (eq90)

and the Doppler width $\gamma_D$ is

$$
\begin{eqnarray}
\gamma_D = 1.46\times 10^{-6}\nu^{(v)}_k T_r^{1/2}
\quad\mbox{[GHz]}.
\end{eqnarray}
$$ (eq91)

The liquid water index of refraction is given by

$$
\begin{eqnarray}
\tilde{N}_W(T_r,\rho_w,\nu) &=& 1.5 \rho_w \left(\frac{\epsilon_w-1}{\epsilon_w+2}\right)
\quad\mbox{[ppm]},
\label{eq_n_W_tilde}
\end{eqnarray}
$$ (eq92)

which is a function of both the cloud water density $\rho_w$ (g~m$^{-3}$)
and the complex permittivity of pure water $\epsilon_w$. The complex
permittivity of pure water, in turn, is approximated by a double-Debye
fit to measured data,

$$
\begin{eqnarray}
\epsilon_w = \epsilon_0 - f\left(\frac{\epsilon_0-\epsilon_1}{\nu + i\gamma_1} + \frac{\epsilon_1-\epsilon_2}{\nu + i\gamma_2}\right),
\end{eqnarray}
$$ (eq93)

with

$$
\begin{eqnarray}
        \epsilon_0 &=& 77.66 + 103.3(T_r-1),\\
        \epsilon_1 &=& 0.0671 \epsilon_0,\\
        \epsilon_2 &=& 3.52 - 7.52(T_r-1),\\
          \gamma_1 &=& 20.2 - 146.4(T_r-1) + 316(T_r-1)^2\quad\mbox{[GHz]},\\
          \gamma_2 &=& 39.8 \gamma1\quad\mbox{[GHz]}.
\end{eqnarray}
$$ (eq94)

Here, $\epsilon_0$ and $\epsilon_1$ are the static and high-frequency
permittivities, respectively; $\epsilon_2$ is a model parameter which
has a slight temperature dependence that has been removed in
{cite:t}`Liebe1993` in order to avoid unphysical behavior for supercooled
water above 100~GHz. As we do not consider liquid water effects below
freezing, we retain this dependence. The parameters $\gamma_1$ and
$\gamma_2$ are two relaxation frequencies expressed in gigahertz;
$\rho_w$ is the liquid water density (g.m$^{-3}$) and $\nu$ is the
radiation frequency in gigahertz. The index of refraction for ice is
given by

$$
\begin{eqnarray}
\tilde{N}_I(T_r,\rho_i,\nu) &=& 1.5\left(\frac{\rho_i}{0.916}\right)\left(\frac{\epsilon_i-1}{\epsilon_i+2}\right)
\quad\mbox{[ppm]},
\label{eq_n_I_tilde}
\end{eqnarray}
$$ (eq95)

where $\rho_i$ is the ice water density (g~m$^{-3}$). The permittivity of
ice, $\epsilon_i$ is given as a function of electromagnetic frequency
and reciprocal temperature by

$$
\begin{eqnarray}
\epsilon_i = 3.15 + i\left(\frac{a_i}{\nu} + b_i\nu\right),
\end{eqnarray}
$$ (eq96)

where

$$
\begin{eqnarray}
a_i &=& (T_r-0.171)\exp{\left[17.0 - 22.1 T_r\right]}\quad\mbox{[GHz]},\\
b_i &=& 1\times 10^{-5}\left[\left(\frac{0.233}{1-0.993/T_r}\right)^2 + \frac{6.33}{T_r} - 1.31\right]\quad\mbox{[GHz$^{-1}$]}.
\end{eqnarray}
$$ (eq97)

##### Propagation Effects on the Stokes Vector

For a deterministic signal, the coefficients $a_h(r)$ and $a_v(r)$ may
be considered to be functions of distance, but for a non-deterministic
signal these coefficients must be considered to be random variables,
and in this case we are primarily interested in the second moments of
the electric field vector. As such, we are concerned with the effect
of the atmosphere on the Stokes vector,

$$
\begin{eqnarray}
\left(
\begin{matrix}
I\\
Q\\
U\\
V\\
\end{matrix}
\right) =
{\cal K}
\left(
\begin{matrix}
\left<E_v E_v^* + E_h E_h^*\right>\\
\left<E_v E_v^* - E_h E_h^*\right>\\
2\Re{\left<E_v E_h^*\right>}\\
2\Im{\left<E_v E_h^*\right>}\\
\end{matrix}
\right)
=
\left(
\begin{matrix}
\left<a_v^2+a_h^2\right>\\
\left<a_v^2-a_h^2\right>\\
2\left<a_va_h\cos(\delta_0)\right>\\
2\left<a_va_h\sin(\delta_0)\right>\\
\end{matrix}
\right),
\end{eqnarray}
$$ (eq98)

where ${\cal K} = \frac{\lambda^2}{k_b\eta_0 B}$ is a constant coefficient that depends upon the receiver
bandwidth $B$, and the electromagnetic wavelength $\lambda$.

The amplitude components are now random variables with a polarized and
a non-polarized component.  We now assume that the atmospheric
attenuation rate $b(r)$ is not a random variable, so that the
attenuation coefficients may be extracted from the ensemble
averages. In this case, [Equations 54 ](#equation-eq55), and
[Equations 55 ](#equation-eq56) become, respectively,

$$
\begin{eqnarray}
\frac{d\left<a_h^2\right>}{dr}  &=& -2 b(r)\left<a_h^2(r)\right>,\label{eq_dah2r}\\
\frac{d\left<a_v^2\right>}{dr}  &=& -2 b(r)\left<a_v^2(r)\right>,\label{eq_dav2r}\\
\frac{d\left<a_va_h\right>}{dr} &=& -2 b(r)\left<a_va_h(r)\right>.\label{eq_davhr}
\end{eqnarray}
$$ (eq99)

It follows directly from [Equations 54 ](#equation-eq55), and
[Equations 55 ](#equation-eq56)  that the Stokes vector components satisfy the
attenuation equations

$$
\begin{eqnarray}
\frac{d}{dr}
\left(
\begin{matrix}
I(r)\\
Q(r)\\
U(r)\\
V(r)\\
\end{matrix}
\right)
=
-2 b(r)
\left(
\begin{matrix}
I(r)\\
Q(r)\\
U(r)\\
V(r)\\
\end{matrix}
\right).\label{eq_satt_i}
\end{eqnarray}
$$ (eq100)

The modified Stokes vector satisfies a similar equation:

$$
\begin{eqnarray}
\frac{d}{dr}
\left(
\begin{matrix}
T_h(r)\\
T_v(r)\\
U(r)\\
V(r)\\
\end{matrix}
\right)
=
-2 b(r)
\left(
\begin{matrix}
T_h(r)\\
T_v(r)\\
U(r)\\
V(r)\\
\end{matrix}
\right).\label{eq_satt_thtv}
\end{eqnarray}
$$ (eq101)


##### Atmospheric Emission


The dispersion relation for electromagnetic radiation in free space is

$$
\begin{eqnarray}
\omega = c k = \frac{2 \pi c}{\lambda}, 
\end{eqnarray}
$$ (eq102)

where $c$ is the speed of light in free space (m~s$^{-1}$), $\omega$
(rad~Hz) and $\lambda$ (m) are the electromagnetic radian frequency
and wavelength, respectively. Thus,

$$
\begin{eqnarray}
\frac{\partial \lambda}{\partial \omega}
= -\frac{2 \pi c}{\omega^2}
= \frac{(2 \pi c) \lambda^2}{(2 \pi c)^2}
= \frac{\lambda^2}{2 \pi c}.\label{eq_lam_om}
\end{eqnarray}
$$ (eq103)

To compute the atmospheric emission we assume that the atmosphere is
characterized by an emissivity $\epsilon(r)$ varying along the path $r$ across the atmosphere, and that its emission is
unpolarized and well-approximated by the Rayleigh-Jeans approximation
for blackbody emission multiplied by the (dimensionless) emissivity
$\epsilon$

$$
\begin{eqnarray}
I = \epsilon\frac{2\pi c k_b}{\lambda^4}
T_{p}\cdot\left(\frac{\partial \lambda}{\partial \omega}\right),
\label{eq_rjl}
\end{eqnarray}
$$ (eq104)

where $I$ is the emitted power (specifically, the spectral radiance or
brightness) with units (W~m$^{-2}$sr$^{-1}$Hz$^{-1}$), $T_{p}$ is the
physical temperature. Using ([102](#equation-eq103)), ([103](#equation-eq104)) becomes

$$
\begin{eqnarray}
I = \epsilon\frac{k_b}{\lambda^2}T_{p}
\end{eqnarray}
$$ (eq105)

In the above, $k_b$ is Boltzmann's constant
(1.38×10$^{-23}$~J~K$^{-1}$).  The emissivity accounts for the fact
that the atmosphere emits much less radiation than a blackbody at the
same physical temperature, and at L-band this emissivity is quite
close to zero but not completely negligible compared with the signal
variations owing to SSS variations. With this modified Rayleigh-Jeans
form, the unpolarized atmospheric emission leads to the following
ordinary differential equations for the Stokes vector,

$$\begin{eqnarray}
\frac{\partial }{\partial r}
\left(
\begin{matrix}
I(r)\\
Q(r)\\
U(r)\\
V(r)\\
\end{matrix}
\right)
=
\epsilon(r)
\left(
\begin{matrix}
T_p(r)\\
0\\
0\\
0\\
\end{matrix}
\right),\label{eq_sem_i}
\end{eqnarray}
$$ (eq106)

or

$$
\begin{eqnarray}
\frac{\partial }{\partial r}
\left(
\begin{matrix}
T_h(r)\\
T_v(r)\\
U(r)\\
V(r)\\
\end{matrix}
\right)
=
\epsilon(r)
\left(
\begin{matrix}
T_p(r)\\
T_p(r)\\
0\\
0\\
\end{matrix}
\right).\label{eq_sem_thtv}
\end{eqnarray}
$$ (eq107)

where the emissivity $\epsilon=\epsilon(r)$ is a function of
location along the propagation path, and the factor $k_b/\lambda^2$
does not appear because the Stokes elements implicitly contain this
factor in their definition in terms of moments of the electric field.


##### Combining Attenuation and Emission

Combining the atmospheric attenuation ([99](#equation-eq100))
([100](#equation-eq101))) and emission ([105](#equation-eq106))
([106](#equation-eq107)))  equations, we have

$$
\begin{eqnarray}
\frac{\partial }{\partial r}
\left(
\begin{matrix}
I(r)\\
Q(r)\\
U(r)\\
V(r)\\
\end{matrix}
\right)
=
-2 b(r)
\left(
\begin{matrix}
I(r)\\
Q(r)\\
U(r)\\
V(r)\\
\end{matrix}
\right)
+
\epsilon(r)
\left(
\begin{matrix}
2 T_p(r)\\
0\\
0\\
0\\
\end{matrix}
\right),\label{eq_rad_i}
\end{eqnarray}
$$ (eq108)

or

$$
\begin{eqnarray}
\frac{\partial }{\partial r}
\left(
\begin{matrix}
T_h(r)\\
T_v(r)\\
U(r)\\
V(r)\\
\end{matrix}
\right)
=
-2 b(r)
\left(
\begin{matrix}
T_h(r)\\
T_v(r)\\
U(r)\\
V(r)\\
\end{matrix}
\right)
+
\epsilon(r)
\left(
\begin{matrix}
T_p(r)\\
T_p(r)\\
0\\
0\\
\end{matrix}
\right).\label{eq_rad_thtv}
\end{eqnarray}
$$ (eq109)

Interestingly, with the assumptions we have made, the third and fourth
Stokes parameters are affected by attenuation but not by emission.

Having established the ordinary differential equation the describes
the change in the Stokes vector along the propagation path, we now
turn to the problem of implementing a solution procedure that is
practical for scene modeling. The straightforward approach is to
ignore refraction and to integrate along linear paths through the
atmosphere, and this is the approach we shall take.

##### Relating Attenuation to Emission

In general radiative transfer scattering may be important, so that
attenuation may involve both absorption and scattering. At L-band,
however, attenuation is dominated by absorption, even in the presence
of cloud droplets and ice, so we may assume that the emissivity is
equal to the *power* attenuation rate (twice the amplitude
attenuation rate):


$$
\begin{eqnarray}
\epsilon(r) \approx 2 b(r).
\end{eqnarray}
$$ (eq110)

Given the similar forms for all Stokes vector components, in what
follows, when we will discuss attenuation, we will use the compact Stokes
vector notation:

$$\begin{eqnarray}
\frac{\partial \bf T}{\partial r} = -2 b(r){\bf T} + b(r){\bf T}_p,
\end{eqnarray}
$$ (eq111)

where ${\bf T}$ is the modified Stokes vector (with orthogonal linear
components rather than $I$ and $Q$ above) and

$$
\begin{eqnarray}
{\bf T}_p =
\epsilon(r)
\left(
\begin{matrix}
T_p(r)\\
T_p(r)\\
0\\
0\\
\end{matrix}
\right).
\end{eqnarray}
$$ (eq112)

We will be concerned about propagation of radiation in the atmosphere
at some zenith angle $\theta$ from the vertical direction, so we let
$r=z \sec\theta$ be the path length in the propagation direction.  We
assume that, for a given location in director cosine coordinates, we
can compute the propagation path. As noted in the development above,
in general this path will be curved if the propagation speed $c$
(related to the real part of $\tilde{k}$) is a function of
position. However, we will neglect this curvature in what follows.  At
each point of the director cosine grid for which the line-of-sight
intersects the earth, the zenith angle is assumed to be identical to
the target incidence angle.  At each point of the director cosine grid
for which the line-of-sight does not intersect the earth, the signal
entering the radiometer will be equal to the sum of the atmospheric
attenuated sky/sun/moon noise plus the integrated atmospheric emission
along the path. In this case, the zenith angle can be computed
directly in the geographic frame.  In general, refraction should be
taken into account when computing the integration path, but for L-band
this refraction is expected to be negligible and so we will neglect it
in what follows.

##### Integration of the Radiative Transfer Equation

As mentioned above, our main concern is attenuation and emission along
the propagation path, where the *amplitude* attenuation
coefficient is given by

$$
\begin{eqnarray}
b(r) = kN'' = 1 \times 10^{-6}\left(\frac{2\pi \nu}{c}\right)
\Im{\left\{\tilde{N}\right\}}\quad\mbox{[Np~m$^{-1}$]},
\end{eqnarray}
$$ (eq113)

As before, $c$ is the speed of light in a vacuum in meters per second,
$\nu$ is the electromagnetic frequency in hertz and $\tilde{N}$ is the
sum of the individual indices of refraction for dry air, water vapor,
cloud water, and cloud ice as given by [Equation 74](#equation-eq75),
[Equation 83](#equation-eq84), [Equation 91](#equation-eq92) and [Equation 94](#equation-eq95),
respectively:

$$
\begin{eqnarray}
\tilde{N}(T_r,e,p_d,\rho_w,\rho_i,\nu) = \tilde{N}_D(T_r,e,p_d,\nu) + \tilde{N}_V(T_r,e,p_d,\nu) + \tilde{N}_W(T_r,\rho_w,\nu) + \tilde{N}_I(T_r,\rho_i,\nu)\quad\mbox{[ppm]}.
\end{eqnarray}
$$ (eq114)

Here we have included explicit dependence of each index of refraction
on atmospheric parameters $T_r$,$e$,$p_d$,$\rho_w$ and $\rho_i$.  In
the forward model we typically take the atmospheric pressure $p$
(derived from geopotential height as a function of pressure),
temperature $T$, relative humidity $r$, and cloud water mixing ratio
fields $q_c$ from an atmospheric model such as the ECMWF or NCEP GFS.  From the fields in the atmospheric model analyses one can
compute the water vapor pressure $e$ from relative humidity $r$ and
temperature $T$; we then compute the partial pressure of dry air,
$p_d$ from $e$ and $p$. We assume that water is entirely ice below
freezing and entirely liquid above freezing, with liquid and ice water
densities being $\rho_w=\rho_i=\rho_d q_c$, respectively, in their
appropriate temperature ranges.

Having obtained $T$, $p_d$, $e$, $r$ $\rho_w$ and $\rho_i$, we then
call four functions implementing the above equations to
compute the indices of refraction owing to dry air, water vapor, and
liquid and ice water.  The resulting refractive indices can then be used
to compute the attenuation rates in units of nepers per meter.

The solution procedure involves interpolation of the attenuation rates
derived from the atmospheric model fields onto a uniformly spaced grid
with $N_a$ layers with layer bottom heights $z_k$ ranging from
$z_s=0$~km through $z_t=15$~km with a $\Delta{z}=200$~m grid interval
in geopotential height $z$. The *power* attenuation rates, denoted
by $a_D(z)$, $a_V(z)$, $a_W(z)$ and $a_I(z)$, are twice the
corresponding amplitude attenuation rates given by $b_D(z)$, $b_V(z)$,
$b_W(z)$ and $b_I(z)$. Using these power attenuation rates, we then
compute the total absorption for each of these components along *vertical* paths from the bottom of the domain to each level.  For
simplicity, here we first form the total power attenuation rate,
$a(z)$, which is the sum of the individual attenuation rates of dry
air, water vapor, and liquid and ice water:

$$
\begin{eqnarray}
a(z)
&=& 2b(r) = 2kN'' = a_D(z) + a_V(z) + a_W(z) + a_I(z)]\nonumber\\
&=& 2\times10^{-6}\left(\frac{2\pi \nu}{c}\right)\Im{\left\{\tilde{N}\right\}}\nonumber\\
&=& 2\times10^{-6}\left(\frac{2\pi \nu}{c}\right)\Im{\left\{\tilde{N}_D(T_r,e,p_d,\nu) + \tilde{N}_V(T_r,e,p_d,\nu) + \tilde{N}_W(T_r,\rho_w,\nu) + \tilde{N}_I(T_r,\rho_i,\nu)\right\}}\quad\mbox{[Np~m$^{-1}$]}.\nonumber
\end{eqnarray}
$$ (eq115)

The integrated attenuation from the surface at height $z_s$ to height
$z$ is

$$
\begin{eqnarray}
A^{(u)}(z) \doteq A(z_s,z) = \displaystyle\int_{z'=z_s}^{z} a(z')\,dz',
\end{eqnarray}
$$ (eq116)

where the superscript denotes integration from below up to height $z$.
Similarly, the integrated attenuation from height $z$ to the top of
the atmosphere is

$$
\begin{eqnarray}
A^{(d)}(z) \doteq A(z,z_t) = \displaystyle\int_{z'=z}^{z_t} a(z')\,dz'\quad\mbox{[Np]},
\end{eqnarray}
$$ (eq117)

where the superscript denotes integration from above down to height
$z$.  The corresponding power transmittances for radiation propagating
upward from below and downward from above are, respectively,

$$\begin{eqnarray}
\tau^{(u)}(z) &\doteq& \tau(z_s,z) = \exp{\left[-A^{(u)}(z)\right]} = \exp{\left[-\displaystyle\int_{z'=z_s}^{z} a(z')\,dz'\right]},\\
\tau^{(d)}(z) &\doteq& \tau(z,z_t) = \exp{\left[-A^{(d)}(z)\right]} = \exp{\left[-\displaystyle\int_{z'=z}^{z_t} a(z')\,dz'\right]}.
\end{eqnarray}
$$ (eq118)

In discrete form the integrated upward and downward attenuation
factors (in nepers) are, respectively,

$$
\begin{eqnarray}
A^{(u)}(z_m) &=& \displaystyle\sum_{k=1}^{m} a(z_k)\Delta{z}\quad\mbox{[Np]},\\
A^{(d)}(z_m) &=& \displaystyle\sum_{k=m}^{N_a} a(z_k)\Delta{z}\quad\mbox{[Np]},
\end{eqnarray}
$$  (eq119)

where $A^{(u)}(z_m)$ and $A^{(u)}(z_m)$ have units of nepers since
they are attenuation rates integrated over small height ranges.  Here
the subscript $m$ is an index into the vector of vertical layers that
form a discrete vertical grid with layer center heights $z_m$. Except
at near grazing zenith angles, an arbitrary path will not traverse a
horizontal distance of more than a few kilometers, and if we assume
that the atmosphere on this spatial scale is horizontally uniform,
then the total attenuation along a path at zenith angle $\theta$ may
be obtained from the preceding total attenuation values by
multiplication by $\sec\theta$. The resulting zenith transmissivity
between the surface and layer $k$ is then

$$
\begin{eqnarray}
\tau^{(u)}(z_m) &=& \tau(z_s,z_m) = \exp{\left[-A^{(u)}_m\right]}.
\end{eqnarray}
$$ (eq120)

In terms of the individual contributions by dry air, water vapor,
cloud water and cloud ice, the total transmissivity from the surface
to the top of layer $z_m$ is

$$
\begin{eqnarray}
\tau^{(u)}(z_m) &\doteq& \tau(z_s,z_m) = \exp{\left[-A^{(u)}(z_m)\right]} = \tau_D^{(u)}(z_m)\tau^{(u)}_V(z_m)\tau^{(u)}_W(z_m)\tau^{(u)}_I(z_m),
\end{eqnarray}
$$ (eq121)

while the total transmissivity from the top of the atmosphere to the base of layer $z_m$ is

$$
\begin{eqnarray}
\tau^{(d)}(z_m) &\doteq& \tau(z_s,z_m) = \exp{\left[-A^{(d)}(z_m)\right]} = \tau_D^{(d)}(z_m)\tau^{(d)}_V(z_m)\tau^{(d)}_W(z_m)\tau^{(d)}_I(z_m).
\end{eqnarray}
$$ (eq122)

The transmissivities at zenith angle $\theta$ are obtained by raising
these zenith transmissivities to the negative $\sec\theta$ power:

$$
\begin{eqnarray}
\tau^{(u)}(z_m,\theta) &\doteq& \tau(z_s,z_m,\theta) = \exp{\left[-A^{(u)}(z_m)\sec\theta\right]} = \left[\tau_D^{(u)}(z_m)\tau^{(u)}_V(z_m)\tau^{(u)}_W(z_m)\tau^{(u)}_I(z_m)\right]^{\sec\theta},\\
\tau^{(d)}(z_m,\theta) &\doteq& \tau(z_s,z_m,\theta) = \exp{\left[-A^{(d)}(z_m)\sec\theta\right]} = \left[\tau_D^{(d)}(z_m)\tau^{(d)}_V(z_m)\tau^{(d)}_W(z_m)\tau^{(d)}_I(z_m)\right]^{\sec\theta}.
\end{eqnarray}
$$ (eq123)

To complete the model we must account for atmospheric emission.  The
(assumed to be unpolarized) brightness temperature emitted by the
atmosphere per unit height interval at some height $z$ above the
ground (i.e., a brightness temperature *rate*) is, assuming that
the atmosphere is in thermal equilibrium,

$$
\begin{eqnarray}
\hat{T}_{ba}(z) &=& a(z) T_p(z),
\label{eq_atm_tb_rate}
\end{eqnarray}
$$ (eq124)

where $T_p(z_k)$ is the physical temperature at height $z_k$ and
$\hat{T}_{bd}(z_k)$ is a brightness temperature {\it rate} with
units of kelvin per meter.  The total brightness temperature rate of
atmospheric emission at the height $z_k$ is, considering all
components,

$$
\begin{eqnarray}
\hat{T}_{ba}(z_k) = \hat{T}_{bd}(z_k)+\hat{T}_{bv}(z_k)+\hat{T}_{bw}(z_k)+\hat{T}_{bi}(z_k).
\end{eqnarray}
$$(eq125)

We can then vertically integrate this total brightness temperature
rate to derive the upwelling attenuated atmospheric brightness
temperature at the top of the atmosphere:

$$
\begin{eqnarray}
T^{toa}_{ba} = \displaystyle\int\limits_{z'=z_s}^{z_t} \hat{T}_{ba}(z')\exp{\left[-\int\limits_{z''=z'}^{z_t}a(z'')\,dz''\right]}\,dz'
\end{eqnarray}
$$(eq126)

which in discrete form becomes

$$
\begin{eqnarray}
T^{toa}_{ba} = \displaystyle\sum_{k=1}^{m} \tau^{(d)}(z_k)T_{bak} = \displaystyle\sum_{k=1}^{m} \tau(z_k,t_t)T_{bak}
= \displaystyle\sum_{k=1}^{m} \tau(z_k,t_t)a(z_k)T_p(z_k)\Delta{z},
\end{eqnarray}
$$(eq127)

where

$$
 \begin{eqnarray}
T_{bak} \approx a(z_k)T_p(z_k)\Delta{z}
\end{eqnarray}
$$ (eq128)

is the unattenuated incremental brightness temperature of emission in
layer $k$ (with units of kelvin).  Similarly, the zenith downwelling
brightness temperature at the bottom of the atmosphere is

$$
\begin{eqnarray}
T^{boa}_{ba} = \displaystyle\int\limits_{z'=z_s}^{z_t} \hat{T}_{ba}(z')\exp{\left[-\int\limits_{z''=z_s}^{z'}a(z'')\,dz''\right]}\,dz'
\end{eqnarray}
$$ (eq129)

which in discrete form becomes

$$
\begin{eqnarray}
T^{boa}_{ba} = \displaystyle\sum_{k=1}^{m} \tau^{(u)}(z_k)T_{bak} = \displaystyle\sum_{k=1}^{m} \tau(z_s,z_k)T_{bak}
= \displaystyle\sum_{k=1}^{m} \tau(z_s,z_k) a(z_k)T_p(z_k)\Delta{z}.
\end{eqnarray}
$$ (eq130)

Although $T^{toa}_{ba}$ and $T^{boa}_{ba}$ do not differ much, they
are not identical. Since we must model scenes for a satellite radiometer, we also need the self-attenuated atmospheric emission at
the flight level and in an arbitrary direction whithin the antenna patterns.  In general this is
a complicated problem because certain directions may involve
near-grazing incidence, or even propagation parallel to the ground, in
which case refraction and earth curvature effects may become important
even at L-band. Here we choose to simplify the problem by neglecting
refraction and earth curvature effects in the propagation path
modeling. With this simplification, computing the brightness
temperature at antenna level $z_a$ and some zenith angle $\theta$
owing to self-attenuated atmospheric emission reduces to one of two
possible integrals, depending upon whether the path is directed upward
or downward towards the earth's surface:

$$
\begin{equation}
T_{ba}(z)
=
\left\{
\begin{array}{ll}
\sec\theta\displaystyle\int\limits_{z'=z_a}^{z_t} \hat{T}_{ba}(z')\exp{\left[-\sec\theta\int\limits_{z''=z_a}^{z'}a(z'')\,dz''\right]}\,dz',\quad0^\circ<\theta<90^\circ,\\
\sec\theta\displaystyle\int\limits_{z'=z_s}^{z_a} \hat{T}_{ba}(z')\exp{\left[-\sec\theta\int\limits_{z''=z'}^{z_a}a(z'')\,dz''\right]}\,dz',\quad90^\circ<\theta<180^\circ,\\
\end{array}
\right.
\label{eq_atm1}
\end{equation}
$$ (eq131)

where $\hat{T}_{ba}(z')$ is given by [Equation 123](#equation-eq124) evaluated at
height $z'$ above the ground. It is important to realize that this is
a significant simplification, since in general we would have to
integrate along curved paths over the curved surface of the earth, and
such curved paths may both decrease and increase in elevation, or even
remain parallel to the ground, leading to very large brightness
temperatures that may approach the physical atmospheric temperature.

Even with the simplified linear paths, it is not practical to
precompute the preceding integrals for all possible antenna heights
and zenith angles, and it is also not practical to compute such
integrals for each director cosine gridpoint as part of the scene
brightness modeling for each measured Stokes vector.  Therefore, a
further approximation is necessary for practical implementation. The
approach we take is to extract the zenith angle dependence from the
inner attenuation integrations by expanding the exponentials in series
of powers of the integrated path attenuation. Doing so will lead to
approximate expressions for the self-attenuated atmospheric emission
brightness temperatures in powers of $\sec\theta$, with coefficients
that result from integrations along vertical paths only.  Such
approximate expressions are expected to be reasonable except for
nearly horizontal paths.  Using

$$
\begin{eqnarray}
e^x = 1 + x + {\cal O}(x^2),
\end{eqnarray}
$$ (eq132)

we have

$$
\begin{eqnarray}
\exp{\left[-\sec\theta\int\limits_{z''=z_a}^{z'}a(z'')\,dz''\right]} &\approx& 1 - \sec\theta\int\limits_{z''=z_a}^{z'}a(z'')\,dz'',\\
\exp{\left[-\sec\theta\int\limits_{z''=z'}^{z_a}a(z'')\,dz''\right]} &\approx& 1 - \sec\theta\int\limits_{z''=z'}^{z_a'}a(z'')\,dz''.
\end{eqnarray}
$$ (eq133)

Substituting the preceding two approximate transmissivity expressions
into (\ref{eq_atm1}), we obtain the approximate brightness temperature
at the antenna height,

$$
\begin{equation}
T_{bad}(z)
\approx
\left\{
\begin{array}{ll}
\sec\theta\left[\displaystyle\int\limits_{z'=z_a}^{z_t} \hat{T}_{ba}(z')\,dz'\right]-\sec^2\theta\left[\displaystyle\int\limits_{z'=z_a}^{z_t} \hat{T}_{ba}(z')\int\limits_{z''=z_a}^{z'}a(z'')\,dz''\,dz'\right],\quad0^\circ<\theta<90^\circ,\\
\sec\theta\left[\displaystyle\int\limits_{z'=z_s}^{z_a} \hat{T}_{ba}(z')\,dz'\right]-\sec^2\theta\left[\displaystyle\int\limits_{z'=z_s}^{z_a} \hat{T}_{ba}(z')\int\limits_{z''=z'}^{z_a}a(z'')\,dz''\,dz'\right],\quad90^\circ<\theta<180^\circ.\\
\end{array}
\right.
\label{eq_atm2}
\end{equation}
$$ (eq134)

As may be seen, each approximate expression consists of the difference
between the unattenuated integrated emission brightness temperature
and a term involving attenuation of the brightness along the
path. Since all zenith angle dependence has been removed from the
integrals, we may precompute these integrals for a range of antenna
elevations and store the results for later calculations at arbitrary
zenith angles. In particular, we precompute the following four
integrals (each with units of kelvin):

$$
\begin{eqnarray}
\tilde{T}^{(0)}_{bad}(z_a) &=& \displaystyle\int\limits_{z'=z_a}^{z_t} \hat{T}_{ba}(z')\,dz',\label{eq_atm3a}\\
\tilde{T}^{(0)}_{bau}(z_a) &=& \displaystyle\int\limits_{z'=z_s}^{z_a} \hat{T}_{ba}(z')\,dz',\label{eq_atm3b}\\
\tilde{T}^{(1)}_{bad}(z_a) &=& \displaystyle\int\limits_{z'=z_a}^{z_t} \hat{T}_{ba}(z')\int\limits_{z''=z_a}^{z'}a(z'')\,dz''\,dz',\label{eq_atm3c}\\
\tilde{T}^{(1)}_{bau}(z_a) &=& \displaystyle\int\limits_{z'=z_s}^{z_a} \hat{T}_{ba}(z')\int\limits_{z''=z'}^{z_a}a(z'')\,dz''\,dz'.\label{eq_atm3d}
\end{eqnarray}
$$ (eq135)

These first two functions  are
the unattenuated *zenith* brightness temperatures associated with
emission from above and below the antenna, respectively; the last two
functions  represent the extent
of brightness temperature attenuation for emission from above and
below, respectively, that would be obtained at *zenith*. For
the algorithm, one can compute these four functions for a range of discrete
heights $z_m$ over the horizontal and temporal domain for which we
obtained gridded atmospheric data.  Then, in the scene modeling
process we interpolated these functions to the antenna height and then
computed the actual unpolarized attenuated atmospheric emission
brightness temperatures at the antenna for arbitrary zenith angle
$\theta$, where $\theta$ is a function of director cosine coordinates
as well as of the antenna orientation. The final self-attenuated
unpolarized atmospheric emission brightness temperatures incident at
the antenna are

$$
\begin{equation}
T_{ba}(z,\theta(\xi,\eta))
\approx
\left\{
\begin{array}{ll}
\sec\theta\tilde{T}^{(0)}_{bad}(z_a) - \sec^2\theta\tilde{T}^{(1)}_{bad}(z_a) ,\quad0^\circ<\theta<90^\circ,\\
\sec\theta\tilde{T}^{(0)}_{bau}(z_a) - \sec^2\theta\tilde{T}^{(1)}_{bau}(z_a),\quad90^\circ<\theta<180^\circ,\\
\end{array}
\right.
\label{eq_atm4}
\end{equation}
$$ (eq136)

where $(\xi,\eta)$ are director cosine coordinates in the front
half-space of the antenna. The final expression constitutes the
model for self-attenuated L-band atmospheric brightness temperature
incident at the antenna. This brightness temperature should be applied
equally to both all components of the Stokes vector before the antenna gain pattern is applied to compute
the final impact on the antenna temperature. This approximate equation
has been found to agree very well with the exact calculation (for
linear paths). The difference between this approximate and the exact solutions is less
than 0.1~K up to about 86$^\circ$ away from zenith for both upwelling
and downwelling radiation.  The approximation breaks down badly beyond 89$^\circ$ and
therefore should not be used in this zenith angle range. In fact, at
large zenith angles path and earth curvature may become significant as
well, so that this simplified linear path/flat earth model may
introduce significant error.


#### Approximate atmospheric emission and absorption

 Following the simpified single layer atmospheric model used in the ESA SMOS Level 2 Ocean Salinity Processor, the atmospheric contributions are further 
 approximated by the following formulation in  which the emission and absorption are expressed purely in 
 terms of air surface temperature $T_o$, surface pressure $P_s$, and total column water vapor $V$. 

In terms of these quantities, the vertically integrated absorption owing to modelcular oxygen and water vapor are, respectively:

$$A_d=A_d^{(u)}=A_d^{(d)}=10^{-6}\cdot (C_{aao}^{(0)}+C_{aao}^{(1)} T_o+C_{aao}^{(2)} P_s+C_{aao}^{(3)} T_o^2+C_{aao}^{(4)} P_s^2+C_{aao}^{(5)} T_o\cdot P_s )$$ (eq137)

and

$$A_v=A_v^{(u)}=A_v^{(d)}=10^{-6}\cdot (C_{aav}^{(0)}+C_{aav}^{(1)} P_s+C_{aav}^{(2)} V)$$ (eq138)

Where the numerical values for coefficients in this mono-layer model are from the papers of {cite:t}`liebe1987millimeter` and of {cite:t}`liebe1992atmospheric`.

$$
\begin{matrix}
C_{aao}^{(0)} &= & 8033.3 \\ 
C_{aao}^{(1)} & = & -103.999 \\
C_{aao}^{(2)} & = & 28.2992 \\
C_{aao}^{(3)} & = & 0.2626 \\
C_{aao}^{(4)} & = & 0.0064 \\
C_{aao}^{(5)} & = & -0.0942
\end{matrix}
$$ (eq139)

and

$$
\begin{matrix}
C_{aav}^{(0)} & = & -151.7150 \\
C_{aav}^{(1)} & = & 0.1554  \\
C_{aav}^{(2)} & = & 3.5406
\end{matrix}
$$ (eq140)

The corresponding 1-way atmosphereic transmittances associated with molecular oxygen absorption and water vapor along a line of sight at angle $θ_s$ from nadir are:

$$τ_d=\exp{[-A_d \sec{⁡θ_s}]}$$ (eq141)

$$τ_v=\exp{[-A_v \sec{⁡θ_s}]}$$ (eq142)

with this formulation, the surface brightness temperature after passage through the atmosphere $T_{B}^{'}$ is related to the unattenuated brightness temperature $T_B$  by:

$$T_{B}^{'}=(τ_{d} τ_{v})T_{B}$$ (eq143)

The upwelling and downwelling atmospheric emission are assumed to be equal and take the following form at nadir for the oxygen and water vapor contributions, respectively:

$$T_{bad}=A_d \cdot[T_o-C_{aeo}^{(0)}-C_{aeo}^{(1)}T_o-C_{aeo}^{(2)} P_{s}-C_{aeo}^{(3)}T_o^2-C_{aeo}^{(4)} P_s^2-C_{aeo}^{(5)} T_{o} P_{s}]$$ (eq144)

and

$$T_{bav}=A_v \cdot [T_o-C_{aev}^{(0)}-C_{aev}^{(1)} P_{s}-C_{aev}^{(2)} V]$$ (eq145)

where

$$
\begin{matrix}
C_{aeo}^{(0)} &= & -0.7789 \\ 
C_{aeo}^{(1)} & = & 0.1376 \\
C_{aeo}^{(2)} & = & -0.0011 \\
C_{aeo}^{(3)} & = & -1.1578\times 10^{-4}\\
C_{aeo}^{(4)} & = & 1.2847 \times 10^{-6} \\
C_{aeo}^{(5)} & = & -1.1133\times 10^{-5}
\end{matrix}
$$ (eq146)

and

$$
\begin{matrix}
C_{aev}^{(0)} & = & 8.1637 \\
C_{aev}^{(1)} & = & 2.4235 \times 10^{-4} \\
C_{aev}^{(2)} & = & 0.0337
\end{matrix}
$$ (eq147)

and the total atmospheric emission brightness temperature at nadir (unpolarized) is:

$$T_{ea}=T_{bad}+T_{bav}$$ (eq148)

Along a path at angle $θ_s$ from  nadir, the  unpolarized brightness temperature of atmospheric 1-way emission  is:

$$T_{ea}(\theta_{s})=\sec⁡\theta_s [T_{bad}+T_{bav}]$$  (eq149)


#### Functionnal flow diagram

```{figure} FFD_atmo.png
--- 
name: FFD_atmo
---
Flowchart of the atmospheric radiative transfer model used in this ATBD. Input data are the 
air surface temperature $T_o$, surface pressure $P_s$, and total column water vapor $V$ and the radiometer incidence angle $θ_s$. Output data are 
the 1-way atmospheric transmittances associated with molecular oxygen absorption $τ_d$ and water vapor $τ_v$
 along a line of sight at angle $θ_s$ from nadir and $T_{ea}$, the unpolarized brightness temperature of atmospheric 1-way emission.
```

### Sea Surface Scattered Solar (Sunglint) contributions ###


#### General Formulation

At L-band, the sun is also a very hot thermal source with effective temperatures on the order of $10^6$ degrees during active periods of the solar cycle ({cite:t}`LeVine2005a`; {cite:t}`Reul2007`) 
with even higher amplitude emission is reached during solar flares. Even though the sun is relatively small in angular extent, it is such a strong source of radiation at L-band that solar effects remain an 
important potential sources of uncertainty on salinity retrieval ({cite:t}`LeVine2005a`; {cite:t}`Reul2007`). 	For a real-aperture radiometer such as the Aquarius, SMAP and CIMR radiometers, 
sun glint (i.e. reflection at the sea surface) impacts the measurements through its contribution to the antenna temperature. Two distinct mechanisms may contribute to the solar radiation intercepted by a radiometer antenna: one is the reflection of solar radiation by the earth-surface (sun glitter or sun glint effects) and the other is the direct leakage into the antenna. 
Here, we only focus on the modelling for the reflected contamination over the ocean, direct contaminations being addressed by the Level 1 processor.

Experimental evidences of the strong sun glitter impacts on the passive microwave sensing of the ocean using L-band radiometers was first given by {cite:t}`swift1` in 1974, who analysed 
the forward scattering of sun microwave radiation from the Cape Code Canal in Massachusetts.
 Data were collected at 1.4, 4.0, and 7.5 GHz for horizontal and vertical polarisation at a fixed nadir viewing angle of 40°. As the sun passed through the main beam of the antennas, 
 Swift found that the excess temperature due to reflected solar radiation increased dramatically with decreasing frequency and was polarization dependent. The sun was found to be such 
 a dominating source at 1.4 GHz that the horizontally polarized component saturated the radiometer.
As shown by {cite:t}`Wentz1978`, these sun-glitter effects might be modelled using approximate scattering models to compute the forward scattering of the sun radiations from
 the rough water surface. Sun glitter does not occur frequently in practice. However, when it does, this phenomenon may have severe effects on the brightness temperature signals 
 measured by spaceborne L- band radiometers. Because the antenna boresight of the rotating real-aperture radiometer CIMR will enters the day side of the Earth’s terminator
  (i.e., the moving curve that divides the daylight side and the dark night side on Earth), the solar effects on the antenna brightness can be of much higher amplitude for this mission, potentially exceeding 15 K.  
  Observations with large sun glint shall be flagged in the CIMR salinity retrievals but small sunglint signal can be corrected for.

 If an incremental rough sea surface area *𝑑𝐴* located within the CIMR antenna field of view is illuminated by the sun radiation along the direction of the
  unit vector $\vec{n}_o$, part of the intercepted energy might be scattered in the direction $\vec{n}_s$, i.e., toward the
  radiometer antenna. The solar energy scattered by $dA$ in the direction $\vec{n}_s$ at time *𝑡* and polarization *p* is represented by the radiometric temperatures 
 
 $$
T_{ssp}(\vec{n}_s,t)= \frac{1}{4π\cos⁡{(θ_s)}}\displaystyle\int_{0}^{2\pi}\int_0^{\beta_{sun}/2} [σ_{pp}(\vec{n}_o,\vec{n}_s)+σ_{pq} (\vec{n}_o,\vec{n}_s)]T_{sun}(t,\vec{n}_o)dΩ_{o}
$$ (eq149a)

where $p$ and $q$ represent the polarizations $H$ or $V$, and $σ_{pp}(\vec{n}_o,\vec{n}_s),σ_{pq}(\vec{n}_o,\vec{n}_s)$ are the bistatic scattering cross-sections of the 
rough sea surface at 1.4 GHz, at scattered direction $\vec{n}_s$ and incident direction $\vec{n}_o$. The scattering elevation angle is denoted $\theta_s$.
The integration limits are over the solid angle subtended by the sun where $\beta_{sun}/2$ is the angular radius of the sun as viewed from the earth. 
At 1.4 GHz, $\beta_{sun}/2$ ≈ 0.293°, which is 10% greater than the optical angular radius ({cite:t}`doi:10.1080/00207216608937868`). $T_{sun}(t,\vec{n}_o)$ is the brightness temperature of the sun at 1.4 GHz in the direction $\vec{n}_o$, 
and at time *t*. This equation shows that in order to estimate the contamination due to sun glint temperature at a given CIMR pixel with node corresponding to position *T* on the earth surface, 
determined by the latitude  $\phi$ and longitude $\psi$ of the observer, and at a given time $t$, the following parameters are needed :

1. $\vec{n}_s$: the direction (incidence and azimuth angles) of sun radiations at the considered earth surface position and time $T(\phi,\psi,t)$,
2. $\vec{n}_o$: the direction (incidence and azimuth angles) of observation from CIMR at target $T(\phi,\psi,t)$,
3. $T_{sun}(t,\vec{n}_o)$: the brightness temperature of the sun at 1.4 GHz in the direction $\vec{n}_o$ and at time *t*, and,
4. $σ_{hh}(\vec{n}_o,\vec{n}_s),σ_{vv}(\vec{n}_o,\vec{n}_s),σ_{vh}(\vec{n}_o,\vec{n}_s),σ_{hv}(\vec{n}_o,\vec{n}_s)$ : the bistatic scattering coefficients of the sea surface for HH, VV, VH and HV polarizations, respectively, 
at scattered direction $\vec{n}_s$, incident direction $\vec{n}_o$, and corresponding to the sea state conditions at target $T(\phi,\psi,t)$. 

Parameters 1) can be obtained from accurate ephemerides and parameters 2) are easily deduced from CIMR observation geometry. The main difficulties in estimating $T_{ssp}(\vec{n}_s,t)$ therefore consist in providing accurate estimates 
for the brightness temperature of the sun at 1.4 GHz and for the sea surface bistatic coefficients at L-band. The brightness temperature of the sun at 1.4 GHz being considered here as an auxiliary parameter, 
we only focussed on the physical description of the bistatic coefficients model. 

An additional model simplification is used to estimate the amount of solar energy scattered by the sea surface and impinging the CIMR antenna. We assumed than within the solid angle subtended by the sun as seen from any of the 
observed terrestrial targets, the local sun direction $\vec{n}_o$ is almost constant, so that, at any target *𝑇*, the radiometric sun glint temperatures $T_{ssp}(\vec{n}_s,t)$  of a sunglint Stokes vector component, can be approximated 
locally at polarisation $p$ and top of the atmosphere, by:

$$
T_{ssp}(θ_o,\phi_o,\theta_s,\phi_s )\simeq (τ_d τ_v ) \frac{\bar{T}_{sun}(t)Ω_{sun}}{4π\cos⁡{(θ_s)}}\cdot [σ_{pp}(θ_o,\phi_o,\theta_s,\phi_s )+σ_{pq} (\theta_o,\phi_o,\theta_s,\phi_s)]
$$ (eq150)

where $T_{sun}$ is the brightness temperature of the sun averaged over the solar disc at 1.4 GHz and at time  $t$, $\Omega_{sun}=2\pi\left[1-\cos{(\frac{\beta}{2})}\right]=8.2 \times 10^{-5} sr$ is the solid angle of the sun at L-band,
 The incidence and azimuth angles from the scattering surface toward the sun are $θ_o$ and $\phi_o$, respectively, 
and the corresponding angles towards the satellite are $θ_s$ and $\phi_s$. Atmospheric attenutaion on the downward path from the sun to 
the sea surface is accounted for by the factor $τ_d τ_v$ expressed in subsection [Atmospheric contributions at L-band](#atmospheric-contributions-at-l-band).


#### Bistatic scattering cross-sections of the  rough sea surface


Key components of the model for sunglint contributions in [Equation (150)](#equation-eq150)  are  the bistatic scattering cross-sections of the 
rough sea surface, $(σ_{pp},σ_{pq})$. Following the approach in {cite:t}`Reul2007`, we used here the Kirchhoff model (KA) which yields the following expression for the
bistatic scattering cross section $\sigma^o_{\alpha\alpha_o}$ for
scattering of the incoming plane waves of polarization $\alpha_o$ into
the outgoing plane waves of polarization $\alpha$:

$$
\begin{equation}
\begin{array}{l}
    \sigma_{\alpha\alpha_o}({\bf k_s},{\bf k_o})=\displaystyle\frac{1}{\pi}\left|\frac{2q_s
    q_o}{q_s+q_o}K_{\alpha\alpha_o}({\bf k_s},
    {\bf k_o})\right|^2e^{\displaystyle-(q_s+q_o)^2\rho(0,0)}\cdot I_K.
    \\ \\
    \end{array}
\label{eq_ssa1_1}
\end{equation}
$$ (eq151)

In the preceding, ${\bf k_o}$ and ${\bf k_s}$ are the incident and
scattered radiation wavenumber vectors, respectively, and may be
expressed in component form as

$$
\begin{eqnarray}
{\bf k_o}/k &=& (\sin\theta_o\cos\phi_o){\bf\hat{x}} + (\sin\theta_o\sin\phi_o){\bf\hat{y}} + (\cos\theta_o){\bf\hat{z}},\\
{\bf k_s}/k &=& (\sin\theta_s\cos\phi_s){\bf\hat{x}} + (\sin\theta_o\sin\phi_o){\bf\hat{y}} + (\cos\theta_o){\bf\hat{z}}.
\end{eqnarray}
$$ (eq152)

where $({\bf\hat{x}}, {\bf\hat{y}}, {\bf\hat{z}})$ are
basis vectors for a local cartesian coordinate system centered at the
scattering surface and $k$ is the wavenumber vector magnitude.

The kernel functions $K_{\alpha\alpha_\circ}(\bf{k_{s}},\bf{k_o})$
are functions of both the scattering geometry and the dielectric
constant. Explicit expressions for these kernel functions may be found in
 Analytical expression of these functions 
for the Kirchhoff Approximation (KA) can be found in  {cite:t}`Voronovich2001` and are provided in [Appendix E](#appendix-e-efficient-implementation-of-bistatic-scattering-coefficients).


The Kirchhoff Integral $I_K$ is given in cartesian coordinates by

$$
\begin{equation}
I_K=\displaystyle\int\limits_{-\infty}^{\infty}\displaystyle\int\limits_{-\infty}^{\infty}
\left\{e^{\displaystyle\left[(q_s+q_o)^2\rho({\bf x})\right]}-1\right\}
e^{\displaystyle\left[-i({\bf k_s}-{\bf k_o}) \cdot
{\bf x}\right]}dxdy.
\label{eq_KI_1}
\end{equation}
$$ (eq153)

The integration domain extends from ${-\infty}$ to $+{\infty}$ in each
dimension. The vector $\bf{x}$ is the horizontal displacement and the
integral is evaluated over all possible displacements in the
horizontal plane.  $q_s={\bf{\hat{z}_e}} \cdot {\bf{k_{s}}}$ and
$q_\circ=-{\bf{\hat{z}_e}} \cdot {\bf{k_o}}$ are the vertical
projections of the scattered and incident wavevectors, respectively.

 The dielectric constant for seawater at L-band is obtained from GW2020's model. 
The sea surface elevation function is assumed to be a Gaussian random process, and the correlation function of the ocean surface elevation, $\rho(x)$, is obtained from the Fourier transform 
of the directional roughness spectrum $W(k)$, which here is given by the wave spectrum model of {cite:t}`Kudryavtsev1999` ({cite:t}`Kudryavtsev2003`,{cite:t}`Kudryavtsev2005`; 
Appendix A in {cite:t}`Yurovskaya2013`). In the present algorithm, only the isotropic part of the spectrum is considered.

#### Functionnal flow diagram

```{figure} FFD_sunglint.png
--- 
name: FFD_sunglint
---
Flowchart of the sunglint model used in this ATBD. Input data are (i) the incidence ($\theta_s$) and azimuth ($\phi_s$) angles of 
the sun radiations at the considered earth surface position (latitude $\phi$ and longitude $\psi$); (ii) the incidence ($\theta_o$) and azimuth  ($\phi_o$) angles in the direction of
 observation from CIMR at target $T(\phi,\psi,t)$; (iii) $T_{sun}(t,\vec{n}_o)$: the brightness temperature of the sun at 1.4 GHz in the direction $\vec{n}_o$ and at 
 time *t*, and, the SSS, SST, wind speed $U_{10}$, wind direction $\phi_w$ and inverse wave age $\Omega$. Output are the brightness tempearture of the scattered sun radiations towards the radiometer $T_{ssp}$.
```



### Sea Surface scattered celestial sky radiation contribution ###

#### Physics of the Problem

For a flat ocean the contribution of the reflected galactic radiation to the antenna temperature
is given by integrating radiation from the galactic sources and reflected at the ocean surface
over the antenna gain pattern. Location and strength of the galactic sources at L-band are taken from the galactic map [{cite:t}`LeVine2004`; {cite:t}`Reul2008b`; {cite:t}`Dinnat2008a`], which was derived
 from radio-astronomy observations. In actuality, bistatic scattering from a rough ocean will result in galactic radiation entering the mainlobe of the antenna from many different directions. 
 In effect, a rough ocean surface tends to add additional spatial smoothing to
the perfectly flat sea surface reflected sky signal. Modeling of this effect is based on the geometric optics (GO) approach, in which the rough surface is modeled as a collection of tilted facets
 with each facet acting as an independent
specular reflector.  A crucial input to the GO model is the distribution of the slopes of the tilted facets of the rough
ocean surface, which depends on the surface wind speed $U_{10}$. At L-band frequencies, Aquarius, SMAP and SMOS algorithms use the slope variance which represents about a 50% reduction in the slope
 variance from the classic Cox and Munk experiment ({cite:t}`Cox1954`), which measured the ocean sun glitter distribution.

Radiation from the galactic background ({cite:t}`LeVine2004`) includes Cosmic Microwave Background (CMB) radiation, which is constant in space and time at 2.7 K, plus hydrogen line emission and
 continuum radiation from extraterrestrial sources. Both are variable across the sky and can affect the measured brightness values by up to 2 – 3 K in general. The total contribution can however be
  more than 12 K in the direction of the plane of the galaxy even when smoothed by the aperture of large antennas like CIMR ({cite:t}`tenerelli2008earth`). Galactic radiation reflects at the sea surface 
  into the satellite radiometer aperture, but can be corrected using data obtained from all sky surveys using L-band radiometers ({cite:t}`LeVine2004`; {cite:t}`Dinnat2008a`; {cite:t}`tenerelli2008earth`;
   {cite:t}`Reul2008b`).  
	In our algorithm we use a similar celestial sky radiation map as is now used in the SMOS level 2 processor but adapted for the central frequency and bandwidth of the CIMR L-band chanels. 
	The GO electromagnetic scattering model is used to quantify the proportion and direction of reflection at the sea surface into the satellite radiometer aperture. We can uniquely represent 
	the rough sea surface scattered sky radiation as a function of six variables:
$T_{scp}\rightarrow T_{scp} (\alpha_s,\delta_s,\theta_s,\psi_{uh},U_{10},φ_{w})$
where

| Variable | Physical Quantity |
| --- | --- |
| $\alpha_s$ | Specular right ascension [deg] |
| $\delta_s$ | Specular declination [deg] |
| $\theta_s$ | Scattered incidence angle [deg]|
| $\psi_{uh}$| Orientation angle [deg]|
| $U_{10}$   | 10 m height surface wind speed [m/s] |
| $\phi_w$   |Wind direction relative to North [deg]|

Table: variables used in the scattering model used for sky radiation


```{figure} galactic_glint.png
--- 
name: Galactic_LBand
---
Map of the incident Total power from sky radiation at L-band including CMB, Hi-line (integrated over a 23 MHz radiometer bandwidth centered on 1.4 GHz) and continuum contributions
```

	

The approach used to model the sea surface scattered sky brightness towards the radiometer integrates the sea surface bistatic scattering coefficients at the radiometer frequency over the
 incident sky brightness temperatures at 1.4 GHz. Consider the case of unpolarized celestial radiation incident at the rough ocean surface scalar brightness temperature $T_{sky}$. The
incidence and azumith angles of the incident radiation are $\theta_o$ and $\phi_o$, respectively.  Assuming a simple exponential model for
attenuation, the total scattered signal at the surface for component $p$ in the direction $(\theta_s,\phi_s)$ reduces to

$$
\begin{equation}
T_{scp}(\theta_s,\phi_s, U_{10}, \varphi_w) =
\frac{1}{4\pi\cos{\theta_s}} \int_{\Omega_o} \left[
\sigma_{pp}(\Omega_o)+ \sigma_{pq}(\Omega_o) \right]
T_{sky}(\Omega_o) e^{-\tau\sec\theta_o(\omega_o)}\,d\Omega_o
\end{equation}
$$ (eq:sg1)

This can be written more explicitly as

$$
\begin{eqnarray}
T_{scp}(\theta_s,\phi_s, U_{10}, \varphi_w)
=
\frac{1}{4\pi\cos{\theta_s}}
\displaystyle\int\limits_{0}^{\pi/2}
\displaystyle\int\limits_{0}^{2\pi}
\left[
\sigma_{pp}(\theta_o,\phi_o)+
\sigma_{pq}(\theta_o,\phi_o)
\right] T_{sky}
\sin\theta_o\,d\phi_o d\theta_o,
\end{eqnarray}
$$ (eq:sg2)

where the dependence of the scattered signal and cross sections on the
scattering direction is implicit. The total reflectivity at
polarization $p$ by

$$\begin{eqnarray}
\Gamma_p
=
\frac{1}{4\pi\cos{\theta_s}}
\displaystyle\int\limits_{0}^{\pi/2}
\displaystyle\int\limits_{0}^{2\pi}
\left[
\sigma_{pp}(\theta_o,\phi_o) +
\sigma_{pq}(\theta_o,\phi_o)
\right]
\sin\theta_o\,d\phi_o d\theta_o.
\end{eqnarray}
$$ (eq:sg3)

For a perfectly flat surface, this expression reduces to the Fresnel power reflection coefficients $|R_{hh}^{(0)}|^2$ and
$|R_{vv}^{(0)}|^2$ , given by:

$$ 
\begin{equation}
\begin{array}{lll}
|R_{hh}^{(0)}(S,T_s,\theta_s)|^2=\left|\displaystyle\frac{\cos{\theta_s}-\sqrt{\epsilon_{sw}(S,T_s)-\sin^2{\theta_s}}}{\cos{\theta_s}+\sqrt{\epsilon_{sw}(S,T_s)-\sin^2{\theta_s}}}\right|^2\\
\\
|R_{vv}^{(0)}(S,T_s,\theta_s)|^2=\left|\displaystyle\frac{\epsilon_{sw}(S,T_s)\cos{\theta_s}-\sqrt{\epsilon_{sw}(S,T_s)-\sin^2{\theta_s}}}{\epsilon_{sw}(S,T_s)\cos{\theta_s}+\sqrt{\epsilon_{sw}(S,T_s)-\sin^2{\theta_s}}}\right|^2\\
\end{array}
\end{equation}
$$ (eq:sg4)

where $\epsilon_{sw}(S,T_s)$ is the dielectric constant for seawater. $S$ is the salinity and $T_s$ is
the sea surface temperature.  Note there is no cross-pol reflectivity
in the flat-surface case, and the reflected signal is obtained by
integrating over the entire upper hemisphere $\Omega_o$:

$$
\begin{eqnarray}
T_{fcp}(\theta_s)
&=&
\displaystyle\int\limits_{\Omega_o}
|R_{pp}^{(0)}(S,T_s,\theta_s)|^2 \delta^2(\Omega_s-\Omega_o) T_{sky}(\theta_o,\phi_o)
\,d\Omega_o\\
&=&
\displaystyle\int\limits_{0}^{\pi/2}
\displaystyle\int\limits_{0}^{2\pi}
|R_{pp}^{(0)}(S,T_s,\theta_s)|^2 \left(\frac{\delta(\theta_s-\theta_o)}{\sin\theta_o}\right)\delta(\phi_s-\phi_o+\pi) T_{sky}(\theta_o,\phi_o)\sin\theta_o\,d\phi_o d\theta_o\\
&=&
\displaystyle\int\limits_{0}^{\pi/2}
\displaystyle\int\limits_{0}^{2\pi}
|R_{pp}^{(0)}(S,T_s,\theta_s)|^2 \delta(\theta_s-\theta_o)\delta(\phi_s-\phi_o+\pi) T_{sky}(\theta_o,\phi_o)\,d\phi_o d\theta_o\\
&=&
|R_{pp}^{(0)}(S,T_s,\theta_s)|^2 T_{sky}(\theta_s,\phi_s-\pi),
\end{eqnarray}
$$ (eq:sg5)

where the Dirac delta function $\delta(\theta_s-\theta_o)$ must be
normalized by the Jacobian of the transformation, $\sin\theta_o$,
between solid angle increment and integration increment $d\phi_o
d\theta_o$, so that

$$
\begin{eqnarray}
\delta^2(\Omega_s-\Omega_o) = \frac{1}{\sin\theta_o}\delta(\theta_s-\theta_o)\delta(\phi_s-\phi_o+\pi).
\end{eqnarray}
$$ (eq:sg6)


#### Expression of the sky glint in specular sky coordinates

To obtain the total scattered signal in a given direction, we must
integrate the brightness temperature contributions from waves incident
at the target from all directions over the upper hemisphere, so that
at polarization $p$, the total scattered signal is

$$
\begin{equation}       
{T}_{scp}(\theta_s,\phi_s, U_{10}, \varphi_w) =
\frac{1}{4\pi\cos{\theta_s}}
\displaystyle\int_{\Omega_o}
\left[
T^{g}_p(\Omega_o)
\sigma_{pp}+
T^{g}_q(\Omega_o)
\sigma_{pq}
\right]
\,d\Omega_o
\end{equation}
$$ (eq:sg7)

where $\Omega_o$ refers to angular position in the upper
hemisphere. Below we only consider the integrated surface scattered
signal owing to waves incidence from all directions, so we simplify
the notation and let ${T}_{scp}(\theta_s,\phi_s, U_{10},
\varphi_w) \rightarrow T^{gs}_p(\theta_s,\phi_s, U_{10}, \varphi'_w)$
where $\varphi'_w=\varphi_w-\phi_s$ is the wind direction relative to
the radiometer azimuth. Now since the noise distribution over the
upper hemisphere is a function of target position on earth and time,
the rough surface scattered celestial signal at polarization $p$,
${T}^{gs}_{p}$, is a function of latitude $\vartheta_g$, longitude
$\varphi_g$, and time $t$ as well as scattering incidence and azimuth
angles, wind speed and wind direction, so that in general we have
additional dependence on latitude, longitude, and time:

The upper hemisphere pole corresponds to the unit normal to the earth
surface at the target latitude and longitude. Denoting the right
ascension and declination of the projection of this point in the
celestial frame by $(\alpha_n, \delta_n)$, we can remove the explicit
dependence on time by introducing $(\alpha_n, \delta_n)$ as
independent variables and expressing the scattered celestial noise as
$\overline{T}^{gs}_{p}(\alpha_n,\delta_n,\theta_s,\phi_s,U_{10},\varphi'_w)$.

However, this respresentation is not optimal for representing the
functional dependence of the scattered signal, since we know that the
dominant source of scattered signal is associated with noise in the
specular direction. Therefore, we seek to represent the scattered
signal in terms of the location in the sky of the specular direction,
which we denote $(\alpha_s,\delta_s)$.  In order to represent the
scattering solution in terms of these variables, we must find a
mapping between $(\alpha_s,\delta_s)$ and $(\alpha_n,\delta_n)$. This
mapping will necessarily involve $\theta_s$ and $\phi_s$, so that we
can write the mapping function as

$$
\begin{eqnarray}
T: (\alpha_n,\delta_n,\theta_s,\phi_s) \rightarrow (\alpha_s,\delta_s,\theta_s,\psi_{uh}),
\end{eqnarray}
$$ (eq:sg8)

where $\theta_s$ is the incidence angle of the specular direction in
the upper hemisphere, and where we have introduced the angle
$\psi_{uh}$, which represents the orientation of the upper hemisphere
at the specular point $(\alpha_s,\delta_s)$. The mapping operator $T$
can be seen to be that function which rotates the unit normal vector
in the upper hemisphere frame into the unit vector in the specular
direction.  $\psi_{uh}$ must be defined so as to allow construction of
an inverse mapping operator $T^{-1}$ that maps a specular direction
$(\alpha_s,\delta_s)$ uniquely into an upper hemisphere unit normal
$(\alpha_n,\delta_n)$. To facilitate a definition of $\psi_{uh}$ we
first establish alt-azimuth coordinate systems and associated basis
vectors in both the upper hemisphere and celestial frames along the
line of sight in the specular direction. The basis vectors are
analogous to horizontal and vertical polarization basis vectors used
to describe electromagnetic plane waves. In the upper hemisphere
frame, which is the topocentric frame whose origin is the surface
target, we define the 'horizontal' basis vector ${\bf\hat{h}^u} =
{\bf\hat{n}^u} \times {\bf\hat{r}}/||{\bf\hat{n}^u} \times {\bf\hat{r}}||$, where ${\bf\hat{n}^u}$ is the unit normal to the
surface at the target and ${\bf\hat{r}}$ is directed outward towards
the specular direction from the target. Next, we define a 'vertical'
basis vector by ${\bf\hat{v}^u} = {\bf\hat{h}^u} \times
{\bf\hat{r}}/||{\bf \hat{h}^u} \times {\bf\hat{r}}||$. If we let
$\phi^u_s$ and $\theta^u_s$ be the specular azimuth and altitude,
respectively, of ${\bf\hat{r}}$ in the upper hemisphere frame, then we
have

$$
\begin{eqnarray}
{\bf\hat{h}^u} &=& -\sin\phi^u_s {\bf\hat{x}^u} + \cos\phi^u_s {\bf\hat{y}^u},\\
{\bf\hat{v}^u} &=& -\cos\phi^u_s\sin\theta^u_s {\bf\hat{x}^u} -\sin\phi^u_s\sin\theta^u_s
{\bf\hat{y}^u} + \cos\theta^u_s {\bf\hat{z}^u},
\end{eqnarray}
$$ (eq:sg9)

where ${\bf\hat{x}^u}$, ${\bf\hat{y}^u}$, and ${\bf\hat{z}^u}$ are basis vectors for
the topocentric earth frame that determines the upper hemisphere.
Analogous basis vectors can be defined in the celestial frame as

$$
\begin{eqnarray}
{\bf\hat{h}^c} &=& -\sin\alpha_s {\bf\hat{x}^c} + \cos\alpha_s {\bf\hat{y}^c},\\
{\bf\hat{v}^c} &=& -\cos\alpha_s\sin\delta_s {\bf\hat{x}^c} -\sin\alpha_s\sin\delta_s
{\bf\hat{y}^c} + \cos\delta_s {\bf\hat{z}^c},
\end{eqnarray}
$$ (eq:sg10)

where $\alpha_s$ and $\delta_s$ are the specular right ascension and
declination, respectively, of ${\bf\hat{r}}$ in the celestial frame.

If we denote the components of a vector normal to the line-of-sight in
the upper hemisphere alt-azimuth $({\bf h},{\bf v})$ frame by $(V^{hu},V^{vu})$,
then its components in the celestial alt-azimuth $({\bf h}-{\bf v}$) frame,
denoted by $(V^{hc},V^{vc})$, are

$$ 
\begin{eqnarray}
\left(
\begin{matrix}
V^{hc}\\
V^{vc}
\end{matrix}
\right)
=
\left(
\begin{matrix}
{\bf\hat{h}^c} \cdot {\bf\hat{h}^{u}} & {\bf\hat{h}^c} \cdot {\bf\hat{v}^{u}}\\
{\bf\hat{v}^c} \cdot {\bf\hat{h}^{u}} & {\bf\hat{v}^c} \cdot {\bf\hat{v}^{u}}\\
\end{matrix}
\right)
\left(
\begin{matrix}
V^{hu}\\
V^{vu}
\end{matrix}
\right)
\end{eqnarray}
$$ (eq:sg11)

It turns out that the preceding matrix is just a rotation matrix, so we can write
this transformation as

$$
\begin{eqnarray}
\left(
\begin{matrix}
V^{hc}\\
V^{vc}
\end{matrix}
\right)
=
\left(
\begin{matrix}
\cos\psi_{uh} & -\sin\psi_{uh}\\
\sin\psi_{uh} &  \cos\psi_{uh}\\
\end{matrix}
\right)
\left(
\begin{matrix}
V^{hu}\\
V^{vu}
\end{matrix}
\right)
\end{eqnarray}
$$ (eq:sg12)

where $\psi_{uh}$ is the angle one must rotate a vector defined in the
upper hemisphere alt-azimuth frame counterclockwise about the
line-of-sight in the specular direction to obtain the vector
components in the celestial sphere alt-azimuth frame.  Equivalently,
it is the angle one must rotate the alt-azimuth basis vectors
clockwise to obtain the basis vectors for the celestial alt-azimuth
frame. This angle is analogous to the Claassen angle in radiometry,
and, comparing the two previous equations, we see that an explicit
expression for it is:

$$ 
\begin{eqnarray}
\psi_{uh} = \tan^{-1}\left(\frac{-{\bf\hat{h}^c} \cdot {\bf \tilde{v}^{u}}}{{\bf\hat{h}^c} \cdot {\bf \tilde{h}^{u}}}\right)
\end{eqnarray}
$$(eq:sg13)

where ${\bf \tilde{h}^{u}}$ and ${\bf \tilde{v}^{u}}$ are basis vectors for the
upper hemisphere frame transformed into the celestial frame by
applying the transformation matrix $T_{ac}$:

$$
\begin{eqnarray}
{\bf \tilde{h}^{u}} &=& {\bf T_{ac}}{\bf\hat{h}^{u}},\\
{\bf \tilde{v}^{u}} &=& {\bf T_{ac}}{\bf\hat{v}^{u}}.
\end{eqnarray}
$$ (eq:sg14)

Note that the arc tangent is here defined such that it returns angles
in the full [0,360] range.  For convenience we repeat the definition
of ${\bf T_{ac}}$:

$$
\begin{equation}
{\bf T_{ac}}(\vartheta_g,\varphi_g,t) = {\bf T_{ec}}(H) {\bf T_{ae}}(\vartheta_g,\varphi_g),
\end{equation}
$$ (eq:sg15)

where

$$\begin{equation}
{\bf T_{ec}} = {\bf R_z}(-H) = {\bf R_z}(-G - \mu).
\end{equation}
$$ (eq:sg16)

The coordinate transformations above are defined in [Appendix F](#appendix-f-coordinate-systems-used-for-sky-glint).
  Given the definition of angle $\psi_{uh}$, it
evident that, given the specular location in the celestial sphere,
$(\alpha_s,\delta_s)$, and given the incidence angle in the specular
direction in the upper hemisphere along with the orientation angle
$\psi_{uh}$, rotating a vector from the specular direction by
$\theta_s$ in the direction $\psi_{uh}$ brings it into the direction
normal to the target, for which the position in the celestial
spherical coordinate system is $(\alpha_n,\delta_n)$. Once this normal
is computed, the latitude and longitude of the target is easily
derived using the time $t$, and from this location together with the
specular location in the sky given by $(\alpha_s,\delta_s)$, the
specular azimuth $\phi_s$ can be computed.  So we see that at some
time $t$ the complete representation of the scattering geometry is
given by the set of variables

$$ 
\begin{eqnarray}
\left\{\alpha_s,\delta_s,\theta_s,\psi_{uh}\right\}
\end{eqnarray}
$$ (eq:sg17)

where we have omitted the geophysical variables $w$ and $\varphi'_w$
that obviously enter into the full scattering problem. So the
functional form of the scattered signal in some scattering direction
$(\theta_s,\phi_s)$ (ignoring any wind direction effect) is

$$
\begin{eqnarray}
{T}_{scp} \rightarrow \tilde{T}_{scp}(\alpha_s,\delta_s,\theta_s,\psi_{uh},U_{10}),
\end{eqnarray}
$$ (eq:sg18)

so the final form for the celestial sky glint is

$$
\begin{equation}       
{T}_{scp}(\alpha_s,\delta_s,\theta_s,\psi_{uh},U_{10}) =
\frac{1}{4\pi\cos{\theta_s}}
\displaystyle\int_{\Omega_o}
\left[
T^{g}_p(\Omega_o)
\sigma_{pp}+
T^{g}_q(\Omega_o)
\sigma_{pq}
\right]
\,d\Omega_o
\end{equation}
$$ (eq:sg19)

or, for unpolarized incident sky radiation (which is assumed here),

$$
\begin{equation}       
{T}_{scp}(\alpha_s,\delta_s,\theta_s,\psi_{uh},U_{10}) =
\frac{1}{4\pi\cos{\theta_s}}
\displaystyle\int_{\Omega_o}
T^{g}_p(\Omega_o)
\left[\sigma_{pp}+\sigma_{pq}
\right]
\,d\Omega_o.
\end{equation}
$$ (eq:sg20)

#### Bistatic Scattering coefficients in the GO approximation


The scattered sky radiation is dominated by contributions around the specular directions, so that the 
scattering cross-sections model can be simplified using the Geometrical Optics approximation 
for the scattering cross sections (which is valid around the specular direction), for
scattering from polarization $q$ into polarization $p$. Before
introducing the model it is necessary to define the notation. The
polarization basis vectors for the incident and scattered waves in the
forward scattering alignment basis convention are

$$
\begin{eqnarray}
\bf{\hat{h}_i} &=& \hat{h}_{ix}{\bf \hat{x}}+ \hat{h}_{iy}{\bf \hat{y}} + \hat{h}_{iz}{\bf \hat{z}},\\
\bf{\hat{h}_s} &=& \hat{h}_{sx}{\bf \hat{x}}+ \hat{h}_{sy}{\bf \hat{y}} + \hat{h}_{sz}{\bf \hat{z}},\\
\bf{\hat{v}_i} &=& \hat{v}_{ix}{\bf \hat{x}}+ \hat{v}_{iy}{\bf \hat{y}} + \hat{v}_{iz}{\bf \hat{z}},\\
\bf{\hat{v}_s} &=& \hat{v}_{sx}{\bf \hat{x}}+ \hat{v}_{sy}{\bf \hat{y}} + \hat{v}_{sz}{\bf \hat{z}}.
\end{eqnarray}
$$ (eq:sg21)

with the horizontal polarization basis vectors given by

$$
\begin{eqnarray}
\hat{h}_{ix} &=& -\sin\overline{\phi}_o,\\
\hat{h}_{iy} &=& \cos\overline{\phi}_o,\\
\hat{h}_{iz} &=& 0,\\
\hat{h}_{sx} &=& -\sin\tilde{\phi}_s,\\
\hat{h}_{sy} &=& \cos\tilde{\phi}_s,\\
\hat{h}_{sz} &=& 0.
\end{eqnarray}
$$ (eq:sg22)

The vertical polarization basis vectors are given by

$$
\begin{eqnarray}
\hat{v}_{ix} &=& -\cos\theta_o\cos\overline{\phi}_o,\\
\hat{v}_{iy} &=& -\cos\theta_o\sin\overline{\phi}_o,\\
\hat{v}_{iz} &=& -\sin\theta_o,\\
\hat{v}_{sx} &=& -\cos\theta_s\cos\tilde{\phi}_s,\\
\hat{v}_{sy} &=& -\cos\theta_s\sin\tilde{\phi}_s,\\
\hat{v}_{sz} &=& -\sin\theta_s.
\end{eqnarray}
$$ (eq:sg23)

The unit vectors pointing outward from the origin towards the incident
and scattered wave directions are

$$
\begin{eqnarray}
{\bf\hat{k}_i} &=& -{\bf{k}_i}/k_0 = \hat{k}_{ix}{\bf \hat{x}}+ \hat{k}_{iy}{\bf \hat{y}} + \hat{k}_{iz}{\bf \hat{z}},\\
{\bf\hat{k}_s} &=& {\bf{k}_s}/k_0 = \hat{k}_{sx}{\bf \hat{x}}+ \hat{k}_{sy}{\bf \hat{y}} + \hat{k}_{sz}{\bf \hat{z}},
\end{eqnarray}
$$ (eq:sg24)

where $k_0$ is the wavenumber magnitude of the electromagnetic
radiation. Note the sign change in the definition of $\bf{\hat{k}_o}$
in terms of ${\bf{k}_o}$, which arises because $\bf{\hat{k}_o}$ points
away from the origin while the incoming wavevector $\bf{{k}_o}$ points
towards the origin (in the incident wave propagation direction).  The
preceding unit vectors have the following cartesian components:

$$
\begin{eqnarray}
\hat{k}_{ix} &=& \sin\theta_o\cos\tilde{\phi}_o,\\
\hat{k}_{iy} &=& \sin\theta_o\sin\tilde{\phi}_o,\\
\hat{k}_{iz} &=& \cos\theta_o,\\
\hat{k}_{sx} &=& \sin\theta_s\cos\tilde{\phi}_s,\\
\hat{k}_{sy} &=& \sin\theta_s\sin\tilde{\phi}_s,\\
\hat{k}_{sz} &=& \cos\theta_s.
\end{eqnarray}
$$ (eq:sg25)

We also define modified incident and scattered wavevector components
which are identical to the previously defined components except that
the incoming wavevector is flipped so that it points towards the
origin:

$$
\begin{eqnarray}
{\bf\hat{n}_i} &=& {\bf{k}_o}/k = \hat{n}_{ix}{\bf \hat{x}}+ \hat{n}_{iy}{\bf \hat{y}} + \hat{n}_{iz}{\bf \hat{z}},\\
{\bf\hat{n}_s} &=& {\bf{k}_s}/k = \hat{n}_{sx}{\bf \hat{x}}+ \hat{n}_{sy}{\bf \hat{y}} + \hat{n}_{sz}{\bf \hat{z}}.
\end{eqnarray}
$$ (eq:sg26)

The associated components are

$$
\begin{eqnarray}
\hat{n}_{ix} &=& -\hat{k}_{ix} = \sin\theta_o\cos\overline{\phi}_o,\\
\hat{n}_{iy} &=& -\hat{k}_{iy} = \sin\theta_o\sin\overline{\phi}_o,\\
\hat{n}_{iz} &=& -\hat{k}_{iz} = -\cos\theta_o,\\
\hat{n}_{sx} &=& \hat{k}_{sx},\\
\hat{n}_{sy} &=& \hat{k}_{sy},\\
\hat{n}_{sz} &=& \hat{k}_{sz}.
\end{eqnarray}
$$ (eq:sg27)

The components of the difference
between the scattered and incident wavevector components are

$$
\begin{eqnarray}
q_x    &=& k\left(\hat{k}_{ix}+\hat{k}_{sx}\right),\\
q_y    &=& k\left(\hat{k}_{iy}+\hat{k}_{sy}\right),\\
q_z    &=& k\left(\hat{k}_{iz}+\hat{k}_{sz}\right),\\
q      &=& \sqrt{q_x^2+q_y^2+q_z^2}.
\end{eqnarray}
$$ (eq:sg28)

In order to relate the scattering properties to the surface, we
need to relate the incident and scattered wave directions to the
specular surface slope. The cartesian components of the specular facet
normal vector are proportional to

$$
\begin{eqnarray}
s_{nx} &=& \left(\hat{k}_{ix}+\hat{k}_{sx}\right)/2,\\
s_{ny} &=& \left(\hat{k}_{iy}+\hat{k}_{sy}\right)/2,\\
s_{nz} &=& \left(\hat{k}_{iz}+\hat{k}_{sz}\right)/2.
\end{eqnarray}
$$ (eq:sg29)

When normalized by the magnitude

$$
\begin{eqnarray}
s_{nm} &=& \sqrt{s_{nx}^2+s_{ny}^2+s_{nz}^2}
\end{eqnarray}
$$ (eq:sg30)

the components become

$$
\begin{eqnarray}
\hat{s}_{nx} &=& s_{nx} / s_{nm},\\
\hat{s}_{ny} &=& s_{ny} / s_{nm},\\
\hat{s}_{nz} &=& s_{nz} / s_{nm},
\end{eqnarray}
$$ (eq:sg31)

and the specular facet upwind and crosswind slopes are

$$
\begin{eqnarray}
S_u  &=& s_{nx} / s_{nz},\\
S_c  &=& -s_{ny} / s_{nz}.
\end{eqnarray}
$$ (eq:sg32)

Letting the mean square slope in the upwind and crosswind directions
be $\sigma_u^2$ and $\sigma_c^2$, respectively, we now define
normalized facet slopes as

$$
\begin{eqnarray}
\eta &=& S_u/\sigma_u,\\
\xi  &=& S_c/\sigma_c.
\end{eqnarray}
$$ (eq:sg33)

Using the preceding definitions, the geometric optics cross sections take the form


$$\begin{eqnarray}
\sigma_{pq} = {\cal A} \cdot P(S_u,S_c) \cdot |{\cal \overline{K}|_{pq}}^2 = \left[\frac{\pi k^2 q^2}{q_z^4}\right]\cdot\left[\frac{1}{2\pi\sigma_u\sigma_c}\exp\left\{-\frac{\xi^2+\eta^2}{2}\right\}\right]\cdot|{\cal \overline{K}|_{pq}}^2,
\end{eqnarray}
$$ (eq155)

where  $A=(\pi k^2 q^2)/(q_z^4)$ and $P$ is the sea surface slope 2D probability distribution function which 
is taken to be Gaussian in the upwind and crosswind directions, and where  $σ_u^2$ and $σ_c^2$ are the upwind 
and crosswind  mean square slope which are function of the surface wind speed. The Kirchhoff kernel functions that appear in the cross section models are
of the form

$$
\begin{eqnarray}
{\cal \overline{K}}_{hh} &=& C\left[R_v ({\bf\hat{h}_s} \cdot {\bf\hat{n}_i})({\bf\hat{h}_i} \cdot {\bf\hat{n}_s}) + R_h ({\bf\hat{v}_s} \cdot {\bf\hat{n}_i})({\bf\hat{v}_i} \cdot {\bf\hat{n}_s})\right],\\
{\cal \overline{K}}_{vv} &=& C\left[R_v ({\bf\hat{v}_s} \cdot {\bf\hat{n}_i})({\bf\hat{v}_i} \cdot {\bf\hat{n}_s}) + R_h ({\bf\hat{h}_s} \cdot {\bf\hat{n}_i})({\bf\hat{h}_i} \cdot {\bf\hat{n}_s})\right],\\
{\cal \overline{K}}_{hv} &=& C\left[R_v ({\bf\hat{h}_s} \cdot {\bf\hat{n}_i})({\bf\hat{v}_i} \cdot {\bf\hat{n}_s}) - R_h ({\bf\hat{v}_s} \cdot {\bf\hat{n}_i})({\bf\hat{h}_i} \cdot {\bf\hat{n}_s})\right],\\
{\cal \overline{K}}_{vh} &=& C\left[R_v ({\bf\hat{v}_s} \cdot {\bf\hat{n}_i})({\bf\hat{h}_i} \cdot {\bf\hat{n}_s}) - R_h ({\bf\hat{h}_s} \cdot {\bf\hat{n}_i})({\bf\hat{v}_i} \cdot {\bf\hat{n}_s})\right].
\end{eqnarray}
$$ (eq:sg35)

Here, $R_v$ and $R_h$ are the Fresnel reflection coefficients as defined previously.


The isotropic slope PDF at any given wind speed is
assumed to take the form of a truncated series expansion in terms of Gaussian PDFs,

$$
\begin{equation}
P(S;U_{10},\theta_s) =
\sum\limits_{n=1}^{N_p} c_{n}(U_{10},\theta_s) P_{n}(S),
\label{eq_dft_backward}
\end{equation}
$$ (eq:sg36)

where

$$
\begin{eqnarray}
P_n(S) = \frac{1}{\pi\sigma_n^2}\exp\left\{-\frac{S_u^2+S_c^2}{\sigma_n^2}\right\} = \frac{1}{\pi\sigma_n^2}\exp\left\{-\frac{S^2}{\sigma_n^2}\right\}.
\end{eqnarray}
$$ (eq:sg37)

The coefficients of the expansion were determined by fitting the model
to the sky glint obtained from the SMOS reconstructed
brightness temperatures and the full ocean forward model without sky
glint included. This model for the cross sections can then be
expressed as

$$
\begin{eqnarray}
\sigma_{pq} = \left[\frac{\pi k^2 q^2}{q_z^4}\right] \cdot |{\cal \overline{K}}_{pq}|^2 \cdot P(S;U_{10},\theta_s)
= \left[\frac{\pi k^2 q^2}{q_z^4}\right] \cdot |{\cal \overline{K}}_{pq}|^2 \cdot \sum\limits_{n=1}^{N_p} c_{n}(U_{10},\theta_s) P_n(S).
\end{eqnarray}
$$ (eq:sg38)

and the GO model sky glint can be expressed as the sum

$$
\begin{equation}
{T}_{scp}(\alpha_s,\delta_s,\theta_s,\psi_{uh},U_{10}) =
\sum\limits_{n=1}^{N_p} c_{n}(U_{10},\theta_s)
\frac{k^2}{4\cos{\theta_s}}
\displaystyle\int_{\Omega_o}
\left[
|{\cal {K}}_{pp}|^2 + |{\cal {K}_{pq}}|^2
\right]
P_n(S(\Omega_o))
\left(\frac{q^2}{q_z^4}\right)
T^{g}_p(\Omega_o)
\,d\Omega_o.
\end{equation}
$$ (eq:sg39)

For notational convenience we now introduce the galactic glint basis
functions,

$$
\begin{equation}
  B_n(\alpha_s,\delta_s,\theta_s,\psi_{uh}) =
\frac{k^2}{4\cos{\theta_s}}
\displaystyle\int_{\Omega_o}
\left[
|{\cal {K}}_{pp}|^2 + |{\cal {K}_{pq}}|^2
\right]
P_n(S(\Omega_o))
\left(\frac{q^2}{q_z^4}\right)
T^{g}_p(\Omega_o)
\,d\Omega_o,
\end{equation}
$$ (eq:sg40)

or, in terms of the (assumed) Gaussian slope PDFs $P_n(\sigma_n)$ with total
mean square slopes $\sigma_n$,

$$
\begin{equation}
  B_n(\alpha_s,\delta_s,\theta_s,\psi_{uh}) =
\frac{k^2}{4\pi\sigma^2_n\cos{\theta_s}}
\displaystyle\int_{\Omega_o}
\left[
|{\cal {K}}_{pp}|^2 + |{\cal {K}}_{pq}|^2
\right]
\exp\left\{-\frac{S^2(\Omega_o)}{\sigma_n^2}\right\}
\left(\frac{q^2}{q_z^4}\right)
T^{g}_p(\Omega_o)
\,d\Omega_o.
\end{equation}
$$ (eq:sg41)

In terms of these basis functions, the model galactic glint can be
compactly expressed as a sum over these basis functions,

$$
\begin{equation}
{T}_{scp}(\alpha_s,\delta_s,\theta_s,\psi_{uh},U_{10}) =
\sum\limits_{n=1}^{N_p} c_{n}(U_{10},\theta_s)
B_n(\alpha_s,\delta_s,\theta_s,\psi_{uh}).
\end{equation}
$$ (eq:sg42)

As discussed above, the galactic glint functions are pre-computed for
a fixed set of 23 total mean square slope values.  The coefficients
were determined by fitting the model to the residual sky glint deduced
from the SMOS brightness temperatures and the ocean forward scene
brightness model (without galactic glint
included). Note that the normalized facet slopes are:

This scattering model is using effective sea surface slope variance parameters which are about 50% less than for optical data ({cite:t}`Cox1954`). 
These values are consistent with the model used for Aquarius and SMAP ({cite:t}`meissner2018salinity`,{cite:t}`meissner2019`), GNSS-Reflectometry studies at L-band ({cite:t}`Garrison2002`)  and well match the aircraft flight data 
acquired by the JPL PALS instrument ({cite:t}`Wilson2001`), or during the ESA/COSMOS, campaigns ({cite:t}`Reul2008a`).

#### Functionnal flow diagram

```{figure} FFD_galglint.png
--- 
name: FFD_galglint
---
Flowchart of the sky glint model used in this ATBD. Input data are (i) the location in the sky of the specular 
direction with respect to the radiometer look direction, with sky coordinate provided by the specular right ascension $\alpha_s$, specular declination $\delta_s$; (ii) the scattered incidence ($\theta_s$) and 
azimuth  ($\phi_s$) angles in the direction of observation from earth target $T(\phi,\psi,t)$ towards CIMR; 
(iii) $T_{sky}(\alpha_s,\delta_s)$: the brightness temperature of the sky at 1.4 GHz in the specular direction
with respect to the radiometer look direction; and, the SSS, SST, wind speed $U_{10}$, 
wind direction $\phi_w$ and inverse wave age $\Omega$. Output are the brightness tempearture of the scattered sky
 radiations towards the radiometer $T_{scp}$.
```

### Accounting for rotation of the polarization plane in the Stokes vector ###

In this section, we summarize the Stokes vector transformation that is applied to the forward model from the surface basis to the instrument antenna frame basis,
 accounting for both a change in polarization basis and the Faraday rotation associated with the passage of radiation through the ionosphere.

#### From surface polarization basis to Ludwig-3 antenna basis ####



```{figure} ludwig-3.png
--- 
name: ludwig-3
---
Diagram summarizing the two rotations required to transport a brightness temperature vector from the surface basis $(\bf{h},\bf{v})$ into the instrument Ludwig-3 basis $(\bf{\hat{L'}}_x,\bf{\hat{L'}}_y$.  Here boresight is into the page so we are looking down towards the target from the instrument. Positive Faraday rotation corresponds to the rotation of the electric field vector $\bf{E}$ into $\bf{E’}$ by the angle $\Omega$ as shown. The additional rotation associated with the change of basis is a further counterclockwise rotation of the electric field vector, or clockwise rotation of the basis $(\bf{h},\bf{v})$ by the angle $\alpha'$. 
```

The first rotation, counterclockwise by angle $\alpha'$ looking down towards the target from the instrument, is associated with the change of polarization
 basis from the surface basis to the instrument basis (so-called Ludwig-3 basis as defined in {cite:t}`Ludwig1973`), so that:

$$
\begin{pmatrix}
E_{x} \\ 
E_{y} \\
\end{pmatrix}=
\begin{pmatrix}
\cos\alpha' & -\sin\alpha'  \\ 
 \sin\alpha' & \cos\alpha' \\ 
\end{pmatrix}
\begin{pmatrix}
E_{h} \\ 
E_{v} \\
\end{pmatrix}
$$	(eq189)

and the corresponding transformation of the Stokes vector is given by:

$$
\begin{pmatrix}
T_{x} \\ 
T_{y}  \\
U_{xy} \\
V_{xy}  \\
\end{pmatrix}=
\begin{pmatrix}
\cos^2\alpha' & \sin^2\alpha' & -\cos\alpha'\sin\alpha' & 0 \\ 
\sin^2\alpha' & \cos^2\alpha' & \cos\alpha'\sin\alpha' & 0 \\
\sin2\alpha' & -\sin2\alpha' & \cos2\alpha' & 0 \\
0 & 0 & 0 & 1 \\
\end{pmatrix}
\begin{pmatrix}
T_{h} \\ 
T_{v}  \\
U \\
V  \\
\end{pmatrix}
$$ (eq190)




```{figure} ludwig-3_2.png
--- 
name: ludwig-32.png
---
Top : Diagram showing the geometry and polarization basis vectors in the surface target frame, denoted by $(\bf{h},\bf{v})$. 
The altitude of the emission vector, directed towards the satellite, is $\theta_e$, and the azimuth of this vector, $\phi_e$, is measured positive 
counterclockwise from due east.  Right : Diagram showing the geometry in the instrument, or antenna, frame. Ludwig-3 polarization basis vectors are denoted
 by basis $(\bf{\hat{L'}}_x,\bf{\hat{L'}}_y)$.  The polarisation basis rotation angle $\alpha'$ is the clockwise rotation of the surface  $(\bf{h},\bf{v})$ 
 into the instrument Ludwig-3 polarization basis $(\bf{\hat{L'}}_x,\bf{\hat{L'}}_y)$. Equivalently, this angle is the counterclockwise rotation of the electric 
 field vector looking down towards the target. The angle of the look direction (towards the ground) off of boresight is $\theta_s$, and the azimuth of the look 
 direction $\phi_s$, is measured positive clockwise from north. 
```

{numref}`ludwig-32.png` shows the surface and instrument (Ludwig-3) polarization basis vectors. 
The polarization basis rotation angle is found using the method introduced by {cite:t}`Zundo2010`. 
In this method, the surface polarization basis vectors have the following cartesian components:

$$
\begin{matrix}
\mathbf{\hat{h}}\cdot\mathbf{\hat{X_e}}&=&-\sin\phi_e \\
\mathbf{\hat{h}}\cdot\mathbf{\hat{Y_e}}&=&\cos\phi_e \\
\mathbf{\hat{h}}\cdot\mathbf{\hat{Z_e}}&=&0 \\
\mathbf{\hat{v}}\cdot\mathbf{\hat{X_e}}&=&-\sin\theta_e\cos\phi_e \\
\mathbf{\hat{v}}\cdot\mathbf{\hat{Y_e}}&=&-\sin\theta_e\sin\phi_e \\
\mathbf{\hat{v}}\cdot\mathbf{\hat{Z_e}}&=&\cos\theta_e \\
\end{matrix}
$$ (eq191)

For the instrument polarization basis, the following associations are made:

$$
\begin{matrix}
\mathbf{\hat{x}_{s}} & \rightarrow & \mathbf{\hat{y}} \\
\mathbf{\hat{y}_{s}} & \rightarrow & \mathbf{\hat{x}} \\
\mathbf{\hat{z}_{s}} & \rightarrow & -\mathbf{\hat{z}} \\
\end{matrix}
$$ (eq192)

Now in the conventional formulation for the Ludwig-3 polarization basis vectors, we denote the vector pointing from the antenna to the target by $\bf{\hat{t}}$ and we simply begin by defining the usual « zonal » and « meridional » unit vectors on the sphere and then rotate them about the target vector $\bf{\hat{t}}$ by the antenna azimuth $\phi$. Thus, we define :

$$
\hat{e}_{\phi}=\displaystyle\frac{\mathbf{\hat{z}}\times\mathbf{\hat{t}}}{||\mathbf{\hat{z}}\times\mathbf{\hat{t}}||}=\frac{(\mathbf{\hat{x}}\times\mathbf{\hat{y}})\times\mathbf{\hat{t}}}{||(\mathbf{\hat{x}}\times\mathbf{\hat{y}})\times\mathbf{\hat{t}}||}=\frac{\mathbf{\hat{y}}(\mathbf{\hat{t}}\cdot\mathbf{\hat{x}})-\mathbf{\hat{x}}(\mathbf{\hat{t}}\cdot\mathbf{\hat{y}})}{||\mathbf{\hat{y}}(\mathbf{\hat{t}}\cdot\mathbf{\hat{x}})-\mathbf{\hat{x}}(\mathbf{\hat{t}}\cdot\mathbf{\hat{y}})||}
$$ (eq193)

$$
\hat{e}_{\theta}=\displaystyle (\mathbf{\hat{z}}\times\mathbf{\hat{t}})\times \mathbf{\hat{t}}
$$ (eq194)

For convenience, we also define the corresponding unnormalized polarization vectors:

$$
e_{\phi}=\displaystyle \mathbf{\hat{y}}(\mathbf{\hat{t}}\cdot\mathbf{\hat{x}})-\mathbf{\hat{x}}(\mathbf{\hat{t}}\cdot\mathbf{\hat{y}})
$$ (eq195)

$$
e_{\theta}=\displaystyle (\mathbf{\hat{z}}\times\mathbf{\hat{t}})\times\mathbf{\hat{t}}=(\mathbf{\hat{z}}\cdot\mathbf{\hat{t}})\mathbf{\hat{t}}-\mathbf{\hat{z}}
$$ (eq196)

which both have the same length, given by $||\mathbf{\hat{y}}(\mathbf{\hat{t}}\cdot\mathbf{\hat{x}})-\mathbf{\hat{x}}(\mathbf{\hat{t}}\cdot\mathbf{\hat{y}})||$. For simplicity, we will use these latter two vectors, rather than the normalized vectors, in what follows. The Ludwig-3 unnormalized components are defined in terms of the preceding unnormliazed vectors by a rotation by the target azimuth in the antenna frame. This rotation is defined so that at boresight, the resulting vectors are now a function of azimuth $\phi$:

$$
\mathbf{L_x}'=e_{\theta}\cos\phi-e_{\phi}\sin\phi
$$ (eq197)

$$
\mathbf{L_y}'=e_{\theta}\sin\phi+e_{\phi}\cos\phi
$$ (eq198)

Now $\cos⁡\phi$ and $\sin⁡\phi$ can be expressed in terms of the target vector and the cartesian basis vector as follows:

$$
\cos⁡\phi=-\mathbf{\hat{y}}\cdot\displaystyle\left[\frac{\mathbf{\hat{t}}\times\mathbf{\hat{z}}}{||\mathbf{\hat{t}}\times\mathbf{\hat{z}}||}\right]
$$ (eq199)

$$
\sin⁡\phi=-\mathbf{\hat{x}}\cdot\displaystyle\left[\frac{\mathbf{\hat{t}}\times\mathbf{\hat{z}}}{||\mathbf{\hat{t}}\times\mathbf{\hat{z}}||}\right]
$$ (eq200)

The normalized Ludwig-3 basis vectors are:

$$
\mathbf{\hat{L}_x}'=\mathbf{L_x}'/||\mathbf{L_x}'||
$$ (eq201)

$$
\mathbf{\hat{L}_y}'=\mathbf{L_y}'/||\mathbf{L_y}'||
$$ (eq202)

Given a target/satellite position with angles $(\theta_e,\phi_e)$ and $(\theta_s,\phi_s)$, both 

- the surface polarization basis vectors $(\hat{h},\hat{v})$ 

and 

- Ludwig-3 basis $(\hat{L_x}',\hat{L_y}')$

can be determined with the previous equations. To find the clockwise basis rotation of the surface basis into the Ludwig-3 basis, we note that this corresponds to a counterclockwise
rotation of the electric field vector itself, and so :

$$
\begin{pmatrix}
E_{x} \\ 
E_{y} \\
\end{pmatrix}=
\begin{pmatrix}
\mathbf{\hat{L}_x}'\cdot\mathbf{\hat{h}} & \mathbf{\hat{L}_x}'\cdot\mathbf{\hat{v}}\\ 
\mathbf{\hat{L}_y}'\cdot\mathbf{\hat{h}} & \mathbf{\hat{L}_y}'\cdot\mathbf{\hat{v}}\\
\end{pmatrix}
\begin{pmatrix}
E_{h} \\ 
E_{v} \\
\end{pmatrix}=
\begin{pmatrix}
\cos\alpha' & -\sin\alpha'  \\ 
 \sin\alpha' & \cos\alpha' \\ 
\end{pmatrix}
\begin{pmatrix}
E_{h} \\ 
E_{v} \\
\end{pmatrix}
$$	(eq203) 

Therefore, we have :

$$
\mathbf{\hat{L}_x}'\cdot\mathbf{\hat{h}}=\cos\alpha'
$$ (eq204)

$$
\mathbf{\hat{L}_x}'\cdot\mathbf{\hat{v}}=-\sin\alpha'
$$ (eq205)

and so, the polarization rotation angle $\alpha'$ may be computed as :

$$
\alpha'=\mathrm{atan2}(-\mathbf{\hat{L}_x}'\cdot\mathbf{\hat{v}},\mathbf{\hat{L}_x}'\cdot\mathbf{\hat{h}})
$$ (eq206)


#### Faraday rotation angle ####


```{figure} faraday.png
--- 
name: faraday.png
---
Diagram showing how the sense of Faraday rotation depends upon the relative directions of the magnetic field and energy propagation. 
Also noted is the expected sense of rotation in each hemisphere.
```

The plane of polarization rotates as radiation passes through the ionosphere with the angle :

$$
\Omega=(K_f⁄f^2 )\cdot \mathrm{VTEC}(z,lat_{400},lon_{400})\cdot B_0\cdot \cos⁡\tilde{\theta}\cdot\mathrm{sec}\chi
$$ (eq207)

where $K_f=1.355\times 10^4\rm{TECU}^{-1}\rm{GHz}^2T^{-1}$, $f$ is the electromagnetic frequency, VTEC is the vertical total electron content reduced to the satellite altitude *z* using 
the formulation of {cite:t}`Floury2007`, $B_0$ is the magnetic field strength [Tesla] evaluated at the ionospheric pierce point (IPP), the point where the ray from the spacecraft to the 
surface crosses 400 km $(lat_{400},lon_{400})$; $\psi$ is the angle the ray makes with the vertical towards the target and $\tilde{\theta}$ is the angle between the magnetic field 
vector and the ray from spacecraft to the surface. As shown in {numref}`faraday.png`, this angle is generaly larger than 90° in the northern hemisphere (with negative Ω) and less 
than 90° in the southern hemisphere (with positive Ω).
The reduction of VTEC to satellite altitude is formulated as two equations, one (morning) for local time within 6 hours of 6 a.m., and the other (evening) for local times within
 6 hours of 6 p.m.

$$
\mathrm{VTEC}(z=800 km,lat_{400},lon_{400})=$$ (eq208)

$$\mathrm{VTEC}(z=\infty,lat_{400},lon_{400})\times[(A_m \cdot F_s+B_m )+C_m \cdot \cos⁡(D_m \cdot C_m \cdot lat_{400}\cdot (\pi/180))]
$$ (eq209)

where $F_s$  is the daily solar flux that can be obtained from daily RSGA files [sfu] and the coefficients $A_m$, $B_m$, $C_m$ and $D_m$ were determined by N. Floury from ESA 
 as provided in Table 5.
The $\mathrm{VTEC}(z=\infty,lat_{400},lon_{400})$ can be obtained from the 1-day forecast produced Centre for Orbit Determination in Europe (CODE), University of Berne,
 Switzerland. For reprocessed SSS products, the VTEC can be obtained from IGS consolidated VTEC.

| Coefficient | Morning value (between 00 and 12 LT) | Evening value (between 12 and 24 LT) |
| :-: | :-: |:-: |
|$A_m$ | $-1.43\times 10^{-4}[\mathrm{sfu}^{-1}]$ | $-9.67\times 10^{-5}[\mathrm{sfu}^{-1}]$ |
|$B_m$ | $8.66\times 10^{-1}[\mathrm{nd}]$ | $8.76\times 10^{-1}[\mathrm{nd}]$ |
|$C_m$ | $3.75\times 10^{-3}[\mathrm{nd}]$ | $8.98\times 10^{-3}[\mathrm{nd}]$ |
|$D_m$ | $3.7[\mathrm{deg}^{-1}]$ | $2.03[\mathrm{deg}^{-1}]$ |
	Table: Coefficients in Floury TEC Altitude Correction

The Magnetic field vector can be obtained from the 12th generation of the International Geomagnetic Reference Field (IGRF), evaluated at 400 km above the earth's surface along 
the line of sight using the software provided [here](https://www.ngdc.noaa.gov/IAGA/vmod/igrf12.f), as converted into a callable FORTRAN function available [here](https://gist.github.com/myjr52/62ca6c3e9c78ea0411)
The function outputs magnetic field strength in nanoTeslas (1e-9 Teslas) which is converted into Gauss (1e-4 Teslas). This model is valid to the year 2020 and should be updated when a new version of the model becomes available.
Further information on the derivation of the associated geomagnetic model may be found [here](https://www.ngdc.noaa.gov/IAGA/vmod/igrf.html).

The Faraday rotation corresponds to the Stokes vector transformation

$$
\begin{eqnarray}
\left(
\begin{matrix}
I'_{hv} \\
Q'_{hv} \\
U'_{hv} \\
V'_{hv} \\
\end{matrix}
\right)
=
\left(
\begin{matrix}
                 1 &               0 &              0  &              0 \\
                 0 &   \cos(2\Omega) &   \sin(2\Omega) &              0 \\
                 0 &  -\sin(2\Omega) &   \cos(2\Omega) &              0 \\
                 0 &               0 &              0  &              1 \\
\end{matrix}
\right)
\left(
\begin{matrix}
I_{hv} \\
Q_{hv} \\
U_{hv} \\
V_{hv}
\end{matrix}
\right).
\end{eqnarray}
$$ (eq:rot1)


The corresponding transformation of the modified Stokes vector is

$$
\begin{equation}
\left(
\begin{matrix}
T'_{h}\\
T'_{v}\\
U'    \\
V'    \\
\end{matrix}
\right)
=
\left(
\begin{matrix}
(I'_{hv}-Q'_{hv})/2 \\
(I'_{hv}+Q'_{hv})/2 \\
U'                 \\
V'                 \\
\end{matrix}
\right)
=
\left(
\begin{matrix}
           \cos^2\Omega &              \sin^2\Omega &     -\cos\Omega\sin\Omega &                    0 \\
           \sin^2\Omega &              \cos^2\Omega &      \cos\Omega\sin\Omega &                    0 \\
          \sin(2\Omega) &            -\sin(2\Omega) &             \cos(2\Omega) &                    0 \\
                      0 &                         0 &                        0  &                    1 \\
\end{matrix}
\right)
\left(
\begin{matrix}
T_{h}  \\
T_{v}  \\
U      \\
V      \\
\end{matrix}
\right).
\end{equation}
$$(eq:rot2)


#### Total rotation from surface basis to antenna basis ####

Of
course, Faraday rotation is just one of two rotations necessary to
transport a surface Stokes vector into the instrument basis. The
second rotation, counterclockwise by angle $\alpha'$ looking down
towards the target from the instrument, is associated with the change
of polarization basis from the surface basis to the instrument
basis. For the chosen CIMR geometry conventions, this polarization
basis rotation angle $\alpha'$ is defined with the same sign
convention as that for $\Omega$  so
that

$$\begin{eqnarray}
\left(
\begin{matrix}
E_h^{Ant}\\
E_v^{Ant}
\end{matrix}
\right)
=
\left(
\begin{matrix}
\cos \alpha' & -\sin \alpha'\\
\sin \alpha' &  \cos \alpha'\\
\end{matrix}
\right)
\left(
\begin{matrix}
E'_h\\
E'_v
\end{matrix}
\right)
\end{eqnarray}
$$(eq:rot6)

and

$$
\begin{eqnarray}
\left(
\begin{matrix}
T_{h}^{Ant}  \\
T_{v}^{Ant}  \\
U_{hv}^{Ant} \\
V_{hv}^{Ant} \\
\end{matrix}
\right)
=
\left(
\begin{matrix}
           \cos^2\alpha' &              \sin^2\alpha' &     -\cos\alpha'\sin\alpha' &                    0 \\
           \sin^2\alpha' &              \cos^2\alpha' &      \cos\alpha'\sin\alpha' &                    0 \\
          \sin(2\alpha') &            -\sin(2\alpha') &              \cos(2\alpha') &                    0 \\
                       0 &                          0 &                           0 &                    1 \\
\end{matrix}
\right)
\left(
\begin{matrix}
T_{h}  \\
T_{v}  \\
U_{hv} \\
V_{hv} \\
\end{matrix}
\right).
\end{eqnarray}
$$(eq:rot7)

Considering both angles together we then have

$$
\begin{eqnarray}
\left(
\begin{matrix}
E_h^{Ant}\\
E_y^{Ant}
\end{matrix}
\right)
=
\left(
\begin{matrix}
\cos (\alpha'+\Omega) & -\sin (\alpha'+\Omega)\\
\sin (\alpha'+\Omega) &  \cos (\alpha'+\Omega)\\
\end{matrix}
\right)
\left(
\begin{matrix}
E_h\\
E_v
\end{matrix}
\right)
\end{eqnarray}
$$ (eq:rot8)

and

$$
\begin{eqnarray}
\left(
\begin{matrix}
T_{h}^{ant}  \\
T_{h}^{ant}  \\
U_{hv}^{ant} \\
V_{hv}^{ant} \\
\end{matrix}
\right)
=
\left(
\begin{matrix}
           \cos^2(\alpha'+\Omega) &              \sin^2(\alpha'+\Omega) &     -\cos(\alpha'+\Omega)\sin(\alpha'+\Omega) &                    0 \\
           \sin^2(\alpha'+\Omega) &              \cos^2(\alpha'+\Omega) &      \cos(\alpha'+\Omega)\sin(\alpha'+\Omega) &                    0 \\
           \sin 2(\alpha'+\Omega) &             -\sin 2(\alpha'+\Omega) &                        \cos 2(\alpha'+\Omega) &                    0 \\
                                0 &                                   0 &                                             0 &                    1 \\
\end{matrix}
\right)
\left(
\begin{matrix}
T_{h}^{TOA}  \\
T_{v}^{TOA}  \\
U_{hv}^{TOA} \\
V_{hv}^{TOA} \\
\end{matrix}
\right).
\end{eqnarray}
$$(eq:rot9)

and we define the rotation matrix :

$$
\begin{eqnarray}
\mathbf{\Psi}(\phi)
=
\left(
\begin{matrix}
           \cos^2(\alpha'+\Omega) &              \sin^2(\alpha'+\Omega) &     -\cos(\alpha'+\Omega)\sin(\alpha'+\Omega) &                    0 \\
           \sin^2(\alpha'+\Omega) &              \cos^2(\alpha'+\Omega) &      \cos(\alpha'+\Omega)\sin(\alpha'+\Omega) &                    0 \\
           \sin 2(\alpha'+\Omega) &             -\sin 2(\alpha'+\Omega) &                        \cos 2(\alpha'+\Omega) &                    0 \\
                                0 &                                   0 &                                             0 &                    1 \\
\end{matrix}
\right)
\end{eqnarray}
$$(eq:rot10)

where $\phi=\alpha'+\Omega$.

### Sum of contributions at the Top of the atmosphere and antenna pattern integration

Considering all components of the scene brightness temperature at L-band, the complete model solution for $T_{tp}^{TOA}$, in the surface polarization basis, is:

$$
\left(\begin{matrix}
T_{th}^{TOA,RTM} \\ 
T_{tv}^{TOA,RTM} \\
U^{TOA,RTM} \\
V^{TOA,RTM} \\
\end{matrix}\right)=
\left(\begin{matrix}
T_{atm}^{up}+(τ_d τ_v )[T_{surf,h}^{tot}+R_{surf,h}^{tot}\cdot T_{atm}^{dw}+T_{sch}+T_{ssh}] \\
T_{atm}^{up}+(τ_d τ_v )[T_{surf,v}^{tot}+R_{surf,v}^{tot}\cdot T_{atm}^{dw}+T_{scv}+T_{ssv}] \\
(τ_d τ_v ) T_{erU} \\
(τ_d τ_v ) T_{erV} \\
\end{matrix}\right)
$$ (eq210)
where 

- $T_{atm}^{up}$ and $T_{atm}^{dw}$ are the unpolarized upwelling and downwelling brightness temperature of atmospheric 1-way emission which can be derived from [Equation 2](#equation-eq2) and  [Equation 148](#equation-eq149),
- $τ_d$ is the 1-way atmospheric transmittance associated with molecular oxygen absorption, determined from [Equation 140](#equation-eq141),
- $τ_v$ is the 1-way atmospheric transmittance associated with water vapor absorption, , determined from [Equation 141](#equation-eq142),
- $T_{surf,p}^{tot}$ and $R_{surf,p}^{tot}$, the p-pol brightness temperature, and reflectivity, of the total sea surface emission, including specular emission determined from [Equation 18](#equation-eq19),[Equation 19](#equation-eq20); rough sea surface
 emission from [Equation 23](#equation-eq24) and  foam emission from [Equation 31](#equation-eq32),
- $T_{erU}$	and $T_{erV}$ are the third and fourth Stokes brightness temperature of rough surface emission, respectively, determined in from [Equation 23](#equation-eq24) 
- $T_{scp}$	is the p-pol brightness temperature of scattered celestial sky radiation, determined by [Equation 181](#equation-eq-sg27)
- $T_{ssp}$, is the p-pol brightness temperature of scattered solar radiation (sunglint), which can be determined by [Equation 150](#equation-eq150)

Note that in order to be able comparing CIMR data and the RTM solutions, one finally need to integrate the $T_{tp}^{TOA,RTM}$ over the CIMR L-band antenna patterns:

$$
T_{tp}^{TOA,RTMi}=\displaystyle\frac{1}{4\pi}\int_{\it{Earth}}\mathbf{G(b)}\mathbf{\Psi}(\phi)T_{tp}^{TOA,RTM}\frac{\partial \Omega}{\partial A}dA
$$ (eq210b)

The integral is over the surface of the earth visible to the sensor, where the differential surface area is $dA$ and $d\Omega$ is the differential
solid angle. The ratio of the differential solid angle to the differential surface area is
 
$$
\frac{\partial \Omega}{\partial A}=f_{lat}\frac{\cos\theta_o}{r^2}
$$ (eq210c)

where $\theta_o$ is the incidence angle and $r$ is the range. For a spherical Earth, the leading term $f_{lat}$ would
be unity. However, the Earth is modeled as an oblate spheroid and as a consequence this term is a
function of latitude, deviating about ±1% from unity.
The matrix $G$ is a 4×4 matrix describing the antenna gain function. Each element in this matrix is a function of the look direction $\mathbf{b}$ 
which  is the unit vector pointing from the antenna to $dA$. The term $\mathbf{\Psi}(\phi)$ is the rotation matrix defined in previous section ([Equation (243)](#equation-eq-rot10)).

### CIMR Leve1b re-sampling approach ###


We recommend the Backus-Gilbert (BG) Optimum Interpolation for the CIMR Leve1b re-sampling approach, which is an established and widely used
method for sampling and gridding passive microwave satellite data ({cite:t}`Poe1990`). It finds a set of
weights $A_i$ in the neighborhood of a chosen synthetic target footprint and computes the antenna temperature $T_A$ of the target
$T_{A,rsp}$, as weighted sum of the individual observations $T_{A,i}$:

$$T_{A,rsp}=\sum_{i} A_i\cdot  T_{A,i}$$ (eq211)

The weights $A_i$ are determined by minimizing the least square deviation of the fit:


$$
Q=\int\int [G_T(x,y)-\sum_i A_i \cdot G_i(x,y)]^2 dx dy
$$ (eq212)

 between the chosen target response (gain) GT and the resampled gain. The index $i$ runs over all
samples in the neighborhood of the target cell that have a sufficiently large weight $A_i$ to be included in the average $T_{A,rsp}$. On can include all samples within a 180
 km radius of the target cell.
 
Carrying out this optimization requires the computation of the normalization integral 

$$
u_i=\int \int G_i(x,y) dxdy
$$ (eq213)

and the two overlap integrals

$$
v_i=\int \int G_T(x,y)\cdot G_i(x,y) dxdy
$$ (eq214)

$$
g_{ij}=\int \int G_T(x,y)\cdot G_i(x,y)+\beta\cdot\delta_{ij} dxdy
$$ (eq215)

The gain functions $G_i(x,y)$ of the individual CIMR observations are given by the pre-launch measured antenna patterns of the CIMR L-band chanel and
 are the individual CIMR gain patterns of the effective field of view (EFOV) 36 x 64 km L-band TA that are recorded in the L1B files.
 
 
 The result of the optimization can be summarized as follows (using vector/matrix notation, the
superscript T denotes the transposed vector):

$$
\mathbf{A}=\mathbf{g}^{-1}\cdot[\mathbf{v}+\displaystyle\frac{(1-\mathbf{u}^{T}\cdot\mathbf{g}^{-1}\cdot\mathbf{v})}{\mathbf{u}^{T}\cdot\mathbf{g}^{-1}\cdot\mathbf{u}}\mathbf{u}]
$$ (eq216)

The parameter $\beta$ is a small smoothing parameter. Its value is chosen to optimize the noise reduction factor $NRF=\sum_i A_i^2$ compared to the orginal gain 
pattern $g_i(x,y)$ in a tradeoff for the quality of fit $Q$. A smaller/larger value for $\beta$ results in a better/worse fit value $Q$ and in a
worse/better $NRF$. 

The values of the resampling weights  $A_i$ , the fit $Q$ and the $NRF$ depend all on the scanning geometry and the scan azimuth angle.

The target cells for the CIMR SSS products are centered on the center points of a fixed $0.25^{\circ}$ Earth grid whose vertices are located at $0^{\circ},0.25^{\circ},0.5^{\circ}$, ....longitude and at $0^{\circ},\pm 0.25^{\circ},\pm 0.5^{\circ}$, ..., latitude. 
The target gain patterns $g_i$ are the same as the original 36 x 64 km EFOV L-band footprints. For the smoothing factor $\beta$ a value of 0.5 is chosen. This results in an average NRF of about 0.4.

The BG OI that can be applied in the CIMR L1B processing of the salinity retrieval algorithm can actually be done in two steps. 
The first step in the resampling is to take a single scan and adjustment the position of the observations to corresponds to integer azimuth angles (i.e. $0^{\circ}$ to $359^{\circ}$).
The sampling in the along-scan direction well exceeds Nyquist sampling, and therefore the fit accuracy of the resampled data shall be very high. 
The second step is the resampling onto the fixed $0.25^{\circ}$.

### Sea Surface Salinity inversion algorithm

The input data from CIMR are the "eight-flavors" CIMR Leve1b L-band channel resampled four stokes parameters for the aft- and fore view of the instrument at the Top of the Atmosphere, namely,
 $T_{th}^{TOA}$, $T_{tv}^{TOA}$, $U^{TOA}$, and $V^{TOA,aft}$.  Other input data from CIMR are the L2 sea surface temperature  and wind speed vector, also properly resampled on the CIMR Leve1b L-band channel grid. 
 In case, these two variables are not available, the algorithm will also rely on ECMWF model weather forecasts. It will also provise an ensemble of auxilliary data needed for evaluating the atmospheric corrections.  Solar fluxes, Sky emission map 
 and TEC data will be provided by external sources. 
  
Due to the way in which surface salinity, temperature, and roughness (e.g. determine from wind speed vector at first order) affect sea surface emissivity, it is difficult to fully separate the effects of surface temperature, 
roughness, and salinity with only the L-band  radiometer measurements. In the proposed processing, we allow the sea surface temperature and wind speed vector to vary within a region about the ancillary wind speed, direction and temperature 
(which shall be derived from the multi-frequency chanels of CIMR, or, from a numerical weather forecast such as ECMWF) via additional penalty terms in the objective function while leaving the salinity unconstrained. 
To retrieve SSS, we will use a maximum-likelihood method with the following objective sum of square function:

$$
\chi^2(SSS,U_{10},\phi_{w},T_s)=\displaystyle\sum_i \left[\frac{T_{ti}^{TOA,CIMR}-T_{ti}^{TOA,RTM}}{NEDT_i}\right]^2+\left[\frac{U_{10}-U_{10}^{p}}{\delta_{U_{10}}}\right]^2+\left[\frac{\phi_{w}-\phi_{w}^{p}}{\delta_{\phi_{w}}}\right]^2+\left[\frac{T_{s}-T_{s}^{p}}{\delta_{T_{s}}}\right]^2
$$ (eq217)

where $T_{ti}^{TOA,CIMR}$ is one of the four flavors of CIMR L1B $T_B$ for either aft-, or, fore- views of CIMR and  $T_{ti}^{TOA,RTM}$ is the forward model value of $T_B$,
 which is dominantly a function of surface wind speed, SST, relative wind azimuth,  and incidence angle.  The weighting factors for the CIMR data are set according to the expected measurement
and modeling uncertainties. We let $NEDT_i$ be the Noise-Equivalent-Delta-T (NEDT) of the L-band radiometer chane *i*. 

The variables $U_{10}^{p}$, $\phi_{w}^{p}$, and $T_{s}^{p}$ are the *a priori* values for the auxilliray surface wind speed, wind direction and temperature, respectively,
 with uncertainties $\delta_{U_{10}}$, $\delta_{\phi_{w}}$, and, $\delta_{T_{s}}$. 

We apply the conjugate gradient technique using a modified Levenberg-Marquardt algorithm [14] to find the local minima
The wind speed vector, SST, and SSS solution to this constrained minimization problem is the final L2B retrievals.

### Algorithm Assumptions and Simplifications



### Functional description of each Algorithm step


##### Mathematical description Roughness Induced Emissivity Model and Lookup Table


###### Mathematical description

In the two-scale model we use in this ATBD, the rough sea surface emission model  can
be written as

$$
\begin{equation}
\left(
\begin{matrix}
\Delta T_{h}\\
\Delta T_{v}\\
\Delta {U}\\
\Delta {V}\\
\end{matrix}
\right)
=
\left(
\begin{matrix}
T_h^{(0)} + T_h^{(1)} \cos (\phi_r-\phi_w)+ T_h^{(2)} \cos 2(\phi_r-\phi_w)\\
T_v^{(0)} + T_v^{(1)} \cos (\phi_r-\phi_w)+T_v^{(2)} \cos 2(\phi_r-\phi_w)\\
U^{(2)} \sin 2(\phi_r-\phi_w)\\
V^{(2)} \sin 2(\phi_r-\phi_w)\\
\end{matrix}
\right),
\end{equation}
$$ (eq218)

or equivalently as


$$
\begin{eqnarray}
\left(
\begin{matrix}
\Delta T_{h}\\
\Delta T_{v}\\
\Delta {U}\\
\Delta {V}\\
\end{matrix}
\right)
&=&
\left(
\begin{matrix}
T_h^{(0)}\\
T_v^{(0)}\\
0\\
0\\
\end{matrix}
\right)
+
\left(
\begin{matrix}
T_h^{(2)}\\
T_v^{(2)}\\
0\\
0\\
\end{matrix}
\right)
\cos 2(\phi_r-\phi_w)
+
\left(
\begin{matrix}
0\\
0\\
U^{(2)}\\
V^{(2)}\\
\end{matrix}
\right)
\sin 2(\phi_r-\phi_w)\\
&&
+
\left(
\begin{matrix}
T_h^{(1)}\\
T_v^{(1)}\\
0\\
0\\
\end{matrix}
\right)
\cos (\phi_r-\phi_w)
+
\left(
\begin{matrix}
0\\
0\\
U^{(1)}\\
V^{(1)}\\
\end{matrix}
\right)
\sin (\phi_r-\phi_w).
\end{eqnarray}
$$ (eq219)

Now using the identities

$$
\begin{eqnarray}
\cos 2(\phi_r-\phi_w) &=& \cos 2\phi_r\cos 2\phi_w + \sin 2\phi_r\sin 2\phi_w,\\
\sin 2(\phi_r-\phi_w) &=& \sin 2\phi_r\cos 2\phi_w - \cos 2\phi_r\sin 2\phi_w,\\
\end{eqnarray}
$$ (eq220)

we can rewrite the emission equation second harmonics as

$$
\begin{equation}
\left(
\begin{matrix}
\Delta T_{h}^{(2)}\\
\Delta T_{v}^{(2)}\\
\Delta {U}^{(2)}\\
\Delta {V}^{(2)}\\
\end{matrix}
\right)
=
\left(
\begin{matrix}
T_h^{(2)}\\
T_v^{(2)}\\
0\\
0\\
\end{matrix}
\right)
\left[
\cos 2\phi_r\cos 2\phi_w + \sin 2\phi_r\sin 2\phi_w
\right]
+
\left(
\begin{matrix}
0\\
0\\
U^{(2)}\\
V^{(2)}\\
\end{matrix}
\right)
\left[
\sin 2\phi_r\cos 2\phi_w - \cos 2\phi_r\sin 2\phi_w
\right].
\end{equation}
$$ (eq221)

This may be rearranged as

$$
\begin{eqnarray}
\left(
\begin{matrix}
\Delta T_{h}^{(2)}\\
\Delta T_{v}^{(2)}\\
\Delta {U}^{(2)}\\
\Delta {V}^{(2)}\\
\end{matrix}
\right)
&=&
\left[
\left(
\begin{matrix}
T_h^{(2)}\\
T_v^{(2)}\\
0\\
0\\
\end{matrix}
\right)
\cos 2\phi_r
+
\left(
\begin{matrix}
0\\
0\\
U^{(2)}\\
V^{(2)}\\
\end{matrix}
\right)
\sin 2\phi_r
\right]
\cos 2\phi_w
\\
&&+
\left[
\left(
\begin{matrix}
T_h^{(2)}\\
T_v^{(2)}\\
0\\
0\\
\end{matrix}
\right)
\sin 2\phi_r
-
\left(
\begin{matrix}
0\\
0\\
U^{(2)}\\
V^{(2)}\\
\end{matrix}
\right)
\cos 2\phi_r
\right]
\sin 2\phi_w.
\end{eqnarray}
$$ (eq222)

Now subtracting the boresight emission azimuth $\phi_{rb}$ from both
$\phi_r$ and $\phi_w$, we can cast the preceding equation into a form
in which the coefficients multiplying the wind direction factors only
depend upon emission azimuth relative to boresight emission azimuth:

$$
\begin{eqnarray}
\left(
\begin{matrix}
\Delta T_{h}^{(2)}\\
\Delta T_{v}^{(2)}\\
\Delta {U}^{(2)}\\
\Delta {V}^{(2)}\\
\end{matrix}
\right)
&=&
\left[
\left(
\begin{matrix}
T_h^{(2)}\\
T_v^{(2)}\\
0\\
0\\
\end{matrix}
\right)
\cos 2(\phi_r-\phi_{rb})
+
\left(
\begin{matrix}
0\\
0\\
U^{(2)}\\
V^{(2)}\\
\end{matrix}
\right)
\sin 2(\phi_r-\phi_{rb})
\right]
\cos 2(\phi_w-\phi_{rb})
\\
&&+
\left[
\left(
\begin{matrix}
T_h^{(2)}\\
T_v^{(2)}\\
0\\
0\\
\end{matrix}
\right)
\sin 2(\phi_r-\phi_{rb})
-
\left(
\begin{matrix}
0\\
0\\
U^{(2)}\\
V^{(2)}\\
\end{matrix}
\right)
\cos 2(\phi_r-\phi_{rb})
\right]
\sin 2(\phi_w-\phi_{rb}).
\end{eqnarray}
$$ (eq223)

And the first harmonic terms may be written as

$$
\begin{equation}
\left(
\begin{matrix}
\Delta T_{h}^{(1)}\\
\Delta T_{v}^{(1)}\\
\Delta {U}^{(1)}\\
\Delta {V}^{(1)}\\
\end{matrix}
\right)
=
\left(
\begin{matrix}
T_h^{(1)}\\
T_v^{(1)}\\
0\\
0\\
\end{matrix}
\right)
\left[
\cos \phi_r\cos \phi_w + \sin \phi_r\sin \phi_w
\right]
+
\left(
\begin{matrix}
0\\
0\\
U^{(1)}\\
V^{(1)}\\
\end{matrix}
\right)
\left[
\sin \phi_r\cos \phi_w - \cos \phi_r\sin \phi_w
\right],
\end{equation}
$$ (eq224)

or as

$$
\begin{eqnarray}
\left(
\begin{matrix}
\Delta T_{h}^{(1)}\\
\Delta T_{v}^{(1)}\\
\Delta {U}^{(1)}\\
\Delta {V}^{(1)}\\
\end{matrix}
\right)
&=&
\left[
\left(
\begin{matrix}
T_h^{(1)}\\
T_v^{(1)}\\
0\\
0\\
\end{matrix}
\right)
\cos (\phi_r-\phi_{rb})
+
\left(
\begin{matrix}
0\\
0\\
U^{(1)}\\
V^{(1)}\\
\end{matrix}
\right)
\sin (\phi_r-\phi_{rb})
\right]
\cos (\phi_w-\phi_{rb})
\\
&&+
\left[
\left(
\begin{matrix}
T_h^{(1)}\\
T_v^{(1)}\\
0\\
0\\
\end{matrix}
\right)
\sin (\phi_r-\phi_{rb})
-
\left(
\begin{matrix}
0\\
0\\
U^{(1)}\\
V^{(1)}\\
\end{matrix}
\right)
\cos (\phi_r-\phi_{rb})
\right]
\sin (\phi_w-\phi_{rb}).
\end{eqnarray}
$$ (eq225)


##### Roughness Induced Emissivity Model and Lookup Table

When the radiating surface is rough we must consider the impact of
this roughness on the emissivity. In the TSM formulation used in
this ATBD, the total brightness temperature vector including roughness
effects can be written as

$$
\begin{equation}
\left(
\begin{matrix}
T_{h}\\
T_{v}\\
{U}\\
{V}\\
\end{matrix}
\right)
=
{\cal K}
\left(
\begin{matrix}
\left<E_h E_h^*\right> \\
\left<E_v E_v^*\right> \\
2\Re{\left<E_v E_h^*\right>} \\
2\Im{\left<E_v E_h^*\right>} \\
\end{matrix}
\right)
=
T_s
\left(
\begin{matrix}
1-|R_{hh}^{(0)}|^2 \\
1-|R_{vv}^{(0)}|^2 \\
0 \\
0 \\
\end{matrix}
\right)
+
\left(
\begin{matrix}
\Delta T_{h}\\
\Delta T_{v}\\
\Delta {U}\\
\Delta {V}\\
\end{matrix}
\right),
\label{eq_tbrough}
\end{equation}
$$ (eq226)

 Here the linearly polarized electric field components
normal to the propagation direction, $E_h$ and $E_v$, are defined with
respect to polarization basis vectors at the target given by
${\bf\hat{h}}$ and ${\bf\hat{v}}$. These basis vectors, in turn, are
defined such that the surface emission propagation direction unit
vector ${\bf\hat{k}}={\bf\hat{v}}\times{\bf\hat{h}}$. Under this *forward scattering alignment* (FSA) polarization basis convention,
the relative azimuth angle $\phi_s$ that appears in the two-scale rough
surface emission contribution is defined as $\phi_s=\phi_r - \phi_w$,
which is the difference between the radiometer azimuth $\phi_r$
(measured counterclockwise from due east and from the perspective of
an observer looking towards the radiometer from the target) and the
downwind direction (towards which the wind is blowing, positive
counterclockwise from due east).  The angle $\theta_s$ is the
radiometer incidence angle.   In the literature, azimuthal harmonics
are typically presented as a function of $\phi_w - \tilde\phi_r$ where
$\tilde\phi_r=\phi_r + 180^\circ$ is the radiometer {\it look}
direction, although frequently the convention is not clearly stated.

Typically, the roughness induced brightness temperature is decomposed
into azimuthal harmonics, and for a sea surface with reflection
symmetry only even harmonics can be different from zero, in which case
the rough surface contribution becomes

$$
\begin{equation}
\left(
\begin{matrix}
\Delta T_{h}\\
\Delta T_{v}\\
\Delta {U}\\
\Delta {V}\\
\end{matrix}
\right)=
\left(
\begin{matrix}
T_h^{(0)} +T_h^{(1)} \cos \phi_s+ T_h^{(2)} \cos 2\phi_s\\
T_v^{(0)} +T_v^{(1)} \cos \phi_s T_v^{(2)} \cos 2\phi_s\\
U^{(2)} \sin 2\phi_s\\
V^{(2)} \sin 2\phi_s\\
\end{matrix}
\right).
\label{eq_tbrough3}
\end{equation}
$$ (eq227)


The first two Stokes parameters of the roughness correction are even
functions of the relative azimuth angle $\phi_s=\phi_r-\phi_w$ in the
FSA convention, while the third and fourth Stokes parameters are odd
functions of this angle and have no zeroth
harmonics. Table~\ref{tab_elut} lists the correspondence between
variable names and quantities described above.

| MATLAB Variable Name | Physical Quantity | Independent Variables |
| :-: | :-: | :-: |
| beta                               |             $k'_{\rho}/k_o$ [nd]                                | beta                       |
| sss                                |             SSS ($S$) [PSU]                                     | sss                        |
| sst                                |             SST ($T_s$) [K]                                     | sst                        |
| theta\_i                           |             radiometer incidence angle ($\theta_i$) [$^\circ$]  | theta\_i                   |
| ust                                |             friction velocity ($u^*$) [m/s]                     | ust                        |
| omega                              |             inverse wave age ($\Omega$) [nd]                    | omega                      |
| char                               |             Charnock parameter ($\alpha_c$) [nd]                | char                       |
| dT\_h\_0                           |             $T_h^{(0)}$ [K]                                     | sss,sst,theta\_i,ust,omega |
| dT\_v\_0                           |             $T_v^{(0)}$ [K]                                     | sss,sst,theta\_i,ust,omega |
| dT\_h\_1                           |             $T_h^{(1)}$ [K]                                     | sss,sst,theta\_i,ust,omega |
| dT\_v\_1                           |             $T_v^{(1)}$ [K]                                     | sss,sst,theta\_i,ust,omega |
| dT\_h\_2                           |             $T_h^{(2)}$ [K]                                     | sss,sst,theta\_i,ust,omega |
| dT\_v\_2                           |             $T_v^{(2)}$ [K]                                     | sss,sst,theta\_i,ust,omega |
| dT\_U\_2                           |             $U^{(2)}$ [K]                                       | sss,sst,theta\_i,ust,omega |
| dT\_V\_2                           |             $V^{(2)}$ [K]                                       | sss,sst,theta\_i,ust,omega |

Mapping Between MATLAB Variable Names and Physical Quantities in the TSM Emissivity Lookup Table.



##### Mathematical description Scattering Galactic Sky Lookup Table 


###### Formulation

The scattered galactic noise signal at polarization $p$, denoted by
$\tilde{T}^{gs}_{p}$, can be expressed in terms of five-dimensional
functions that provide zeroth and second harmonics as a function of
downwind direction, so that we have

$$
\begin{eqnarray}
\tilde{T}^{gs}_{p}(\alpha_s,\delta_s,\theta_s,\psi_{uh},u_{10},\varphi_w) &=& \tilde{A}^{(0)}_p(\alpha_s,\delta_s,\theta_s,\psi_{uh},u_{10})\\
&+& \tilde{A}^{(2)}_p(\alpha_s,\delta_s,\theta_s,\psi_{uh},u_{10}) \cos(2\varphi'_w)\\
&+& \tilde{B}^{(2)}_p(\alpha_s,\delta_s,\theta_s,\psi_{uh},u_{10}) \sin(2\varphi'_w),
\end{eqnarray}
$$ (eq228)

where $\alpha_s$, $\delta_s$, $\theta_s$, $u_{10}$ are, respectively,
the specular right ascension, specular declination, radiometer
incidence angle, and the 10-m wind
speed. $\varphi'_w=\varphi_w-\phi_s$ is the difference between the
downwind direction (towards which the wind is blowing, counterclockwise
from due east) and the radiometer azimuth (looking towards the
radiometer from the origin).

The *upper hemisphere orientation angle* $\psi_{uh}$ is the angle
one must rotate a vector defined in the upper hemisphere alt-azimuth
frame counterclockwise about the line-of-sight in the specular
direction to obtain the vector components in the celestial sphere
alt-azimuth frame.  Equivalently, it is the angle one must rotate the
alt-azimuth basis vectors clockwise to obtain the basis vectors for
the celestial alt-azimuth frame.

To define this angle, we begin by defining *horizontal* and
*vertical* basis vectors along the line of sight from the target to
the galactic noise source in the specular direction.  These basis
vectors are defined in both the topocentric frame and the celestial
frame. The upper hemisphere basis vectors in the topocentric frame
(whose origin is at the target) are defined as

$$
\begin{eqnarray}
{\bf\hat{h}^u} &=& -\sin\phi_s {\bf\hat{x}^u} + \cos\phi_s {\bf\hat{y}^u},\\
{\bf\hat{v}^u} &=& -\cos\phi_s\cos\theta_s {\bf\hat{x}^u} -\sin\phi_s\cos\theta_s{\bf\hat{y}^u} + \sin\theta_s {\bf\hat{z}^u},\\
\end{eqnarray}
$$ (eq229)

where ${\bf\hat{x}^u}$, ${\bf\hat{y}^u}$, and ${\bf\hat{z}^u}$ are
basis vectors for the target topocentric frame that determines the
upper hemisphere.  Also, $\phi_s$ is the radiometer azimuth (looking
towards the radiometer from the origin) and $\theta_s$ is the
radiometer incidence angle.  Analogous basis vectors can be defined in
the celestial frame as

$$
\begin{eqnarray}
{\bf\hat{h}^c} &=& -\sin\alpha_s {\bf\hat{x}^c} + \cos\alpha_s {\bf\hat{y}^c},\\
{\bf\hat{v}^c} &=& -\cos\alpha_s\sin\delta_s {\bf\hat{x}^c} -\sin\alpha_s\sin\delta_s{\bf\hat{y}^c} + \cos\delta_s {\bf\hat{z}^c},\\
\end{eqnarray}
$$ (eq230)

where, as above, $\alpha_s$ and $\delta_s$ are, respectively, the
specular right ascension and specular declination.  Also,
${\bf\hat{x}^c}$, ${\bf\hat{y}^c}$, and ${\bf\hat{z}^c}$ are basis
vectors for the celestial frame.

Since both sets of mutually orthogonal basis vectors are orthogonal to
the line of sight, they must be related to each other via a rotation
(and possible reflection). Assuming (by convention) only a rotation
relates the two, we can compute this rotation angle by transforming
both sets of vectors to the same frame and then equating component
projection inner products with corresponding rotation matrix entries.

The approach we take is to transform the upper hemisphere alt-azimuth
basis vectors into the celestial frame by appling transform matrices
derived from EE CFI (version 3.4, \cite{deimos:mcd}).  The transformation matrix from the
target topocentric frame (with basis vectors ${\bf\hat{x}^u}$,
${\bf\hat{y}^u}$, and ${\bf\hat{z}^u}$) to the celestial frame (with
basis vectors ${\bf\hat{x}^c}$, ${\bf\hat{y}^c}$, and
${\bf\hat{z}^c}$) is ${\bf T_{ac}}$:

$$
\begin{equation}
{\bf T_{ac}}(\vartheta_g,\varphi_g,t) = {\bf T_{ec}}(H) {\bf T_{ae}}(\vartheta_g,\varphi_g),
\end{equation}
$$  (eq231)

where

$$
\begin{equation}
{\bf T_{ec}} = {\bf R_z}(-H) = {\bf R_z}(-G - \mu).
\end{equation}
$$  (eq232)

Here, $\vartheta_g$ is geodetic latitude, $\varphi_g$ is the geodetic
longitude, ${\bf R_z}$ is the rotation matrix defined as

$$
\begin{equation}
\begin{array}{c}
{\bf R_z}(\phi) = \left( \begin{array}{ccc}
 \cos\phi & \sin\phi & 0 \\
-\sin\phi & \cos\phi & 0 \\
          0 &          0 & 1 \end{array} \right).
\end{array}
\end{equation}
$$  (eq233)

and ${\bf T_{ec}}$ is the transformation matrix the transforms vector
components from the earth fixed frame to the TOD celestial frame. $G$
is a third-order polynomial in time and $\mu$ is a nutation angle, and
both are calculated within EE CFI. ${\bf T_{ec}}$ may be computed
conveniently using EE CFI.

The transformation matrix ${\bf T_{ae}}(\vartheta_g,\varphi_g)$
transforms a vector from the topocentric frame, or earth alt-azimuth
frame $\cal A$, to the earth fixed frame $\cal E$. It may be derived
by determining a series or rotations to align the basis vectors
(${\bf\hat{x}^u}$, ${\bf\hat{y}^u}$, ${\bf\hat{z}^u}$) with the
cartesian basis vectors in $\cal E$. The resulting composite
transformation from $\cal A$ to $\cal E$ can be written as

$$
\begin{equation}
{\bf T_{ae}}(\vartheta_g,\varphi_g) = {\bf R_z}\left(-\varphi_g - \frac{\pi}{2}\right) {\bf R_x}\left(\vartheta_g - \frac{\pi}{2}\right),
\end{equation}
$$  (eq234)

and its inverse, mapping vectors from $\cal E$ to $\cal A$, is simply

$$
\begin{equation}
{\bf T_{ea}}(\vartheta_g,\varphi_g) = {\bf R_x}\left(-\vartheta_g + \frac{\pi}{2}\right) {\bf R_z}\left(\varphi_g + \frac{\pi}{2}\right),
\end{equation}
$$  (eq235)

where we have introduced another rotation matrix,

$$
\begin{equation}
\begin{array}{c}
{\bf R_x}(\phi) = \left( \begin{array}{ccc}
1 &           0 &          0 \\
0 &  \cos\phi & \sin\phi \\
0 & -\sin\phi & \cos\phi \end{array} \right).
\end{array}
\end{equation}
$$  (eq236)

Applying the composite transformation matrix $T_{ac}$ to the upper
hemisphere alt-azimuth basis vectors, we obtain

$$
\begin{eqnarray}
{\bf \tilde{h}^{u}} &=& {\bf T_{ac}}{\bf\hat{h}^{u}},\\
{\bf \tilde{v}^{u}} &=& {\bf T_{ac}}{\bf\hat{v}^{u}}.
\end{eqnarray}
$$  (eq237)

where ${\bf \tilde{h}^{u}}$ and ${\bf \tilde{v}^{u}}$ are alt-azimuth
basis vectors for the upper hemisphere frame transformed into the
celestial frame.

We now denote the components of a vector normal to the line-of-sight
in the upper hemisphere alt-azimuth frame by $(V^{hu},V^{vu})$, and we
define corresponding components in the celestial alt-azimuth frame,
denoted by $(V^{hc},V^{vc})$. The two sets of components are related
by the transformation

$$
\begin{eqnarray}
\left(
\begin{matrix}
V^{hc}\\
V^{vc}
\end{matrix}
\right)
=
\left(
\begin{matrix}
{\bf\hat{h}^c} \cdot {\bf\hat{h}^{u}} & {\bf\hat{h}^c} \cdot {\bf\hat{v}^{u}}\\
{\bf\hat{v}^c} \cdot {\bf\hat{h}^{u}} & {\bf\hat{v}^c} \cdot {\bf\hat{v}^{u}}\\
\end{matrix}
\right)
\left(
\begin{matrix}
V^{hu}\\
V^{vu}
\end{matrix}
\right)
\end{eqnarray}
$$  (eq238)

or equivalently

$$
\begin{eqnarray}
\left(
\begin{matrix}
V^{hc}\\
V^{vc}
\end{matrix}
\right)
=
\left(
\begin{matrix}
{\bf\hat{h}^c} \cdot {\bf\tilde{h}^{u}} & {\bf\hat{h}^c} \cdot {\bf\tilde{v}^{u}}\\
{\bf\hat{v}^c} \cdot {\bf\tilde{h}^{u}} & {\bf\hat{v}^c} \cdot {\bf\tilde{v}^{u}}\\
\end{matrix}
\right)
\left(
\begin{matrix}
V^{hu}\\
V^{vu}
\end{matrix}
\right).
\end{eqnarray}
$$  (eq239)


Since, as noted earlier, the preceding transformation must be a rotation
matrix, we can write this transformation as

$$
\begin{eqnarray}
\left(
\begin{matrix}
V^{hc}\\
V^{vc}
\end{matrix}
\right)
=
\left(
\begin{matrix}
\cos\psi_{uh} & -\sin\psi_{uh}\\
\sin\psi_{uh} &  \cos\psi_{uh}\\
\end{matrix}
\right)
\left(
\begin{matrix}
V^{hu}\\
V^{vu}
\end{matrix}
\right)
\end{eqnarray}
$$ (eq240)

where $\psi_{uh}$ is the angle one must rotate a vector defined in the
upper hemisphere alt-azimuth frame counterclockwise about the
line-of-sight in the specular direction to obtain the vector
components in the celestial sphere alt-azimuth frame.  Equivalently,
it is the angle one must rotate the alt-azimuth basis vectors
clockwise to obtain the basis vectors for the celestial alt-azimuth
frame. This angle is analogous to the Claassen angle in radiometry.
Comparing the previous two equations, we see that

$$
\begin{eqnarray}
\left(
\begin{matrix}
{\bf\hat{h}^c} \cdot {\bf\tilde{h}^{u}} & {\bf\hat{h}^c} \cdot {\bf\tilde{v}^{u}}\\
{\bf\hat{v}^c} \cdot {\bf\tilde{h}^{u}} & {\bf\hat{v}^c} \cdot {\bf\tilde{v}^{u}}\\
\end{matrix}
\right)
=
\left(
\begin{matrix}
\cos\psi_{uh} & -\sin\psi_{uh}\\
\sin\psi_{uh} &  \cos\psi_{uh}\\
\end{matrix}
\right).
\end{eqnarray}
$$  (eq241)

Equating the top row entries in these two matrices, we see that

$$
\begin{eqnarray}
\cos\psi_{uh} &=&  {\bf\hat{h}^c} \cdot {\bf\tilde{h}^{u}},\nonumber\\
\sin\psi_{uh} &=& -{\bf\hat{h}^c} \cdot {\bf\tilde{v}^{u}},\nonumber\\
\end{eqnarray}
$$  (eq242)

or

$$
\begin{eqnarray}
\frac{\sin\psi_{uh}}{\cos\psi_{uh}} = \frac{-{\bf\hat{h}^c} \cdot {\bf\tilde{v}^{u}}}{{\bf\hat{h}^c} \cdot {\bf\tilde{h}^{u}}},
\end{eqnarray}
$$  (eq243)

and so

$$
\begin{eqnarray}
\psi_{uh} = \tan^{-1}\left(\frac{-{\bf\hat{h}^c} \cdot {\bf \tilde{v}^{u}}}{{\bf\hat{h}^c} \cdot {\bf \tilde{h}^{u}}}\right).
\end{eqnarray}
$$  (eq244a)

It is understood that the $\tan^{-1}$ function returns the full
$360^\circ$ range of angles (so it is equivalent to the atan2()
function in MATLAB and C):

$$
\begin{eqnarray}
\psi_{uh}~\mbox{[deg]} = \frac{180}{\pi} \cdot \mbox{atan2}\left(-{\bf\hat{h}^c} \cdot {\bf \tilde{v}^{u}},{\bf\hat{h}^c} \cdot {\bf \tilde{h}^{u}}\right).
\end{eqnarray}
$$  (eq244)

If we consider the complex variable $x+iy=\left(x^2+y^2\right)^{1/2}e^{i\theta}$, with $x$ and $y$ real
variables, then clearly we have

$$
\begin{eqnarray}
\theta+2n\pi = \mbox{atan2}(y,x)
\end{eqnarray}
$$  (eq245)

and also

$$
\begin{eqnarray}
i(\theta+2n\pi) + \mbox{ln}\left(\sqrt{x^2+y^2}\right) = \mbox{Log}~(x+iy).
\end{eqnarray}
$$  (eq246)

where $n$ is any integer and $\mbox{Log(z)}$ is the principal branch
of the natural logarithm of the complex variable $z$. By convention,
we (and MATLAB) define the principal branch $q=\mbox{Log}(z)$ so that
$-\pi<\Im\left\{q\right\}<\pi$ for any $z$ not equal to a negative
real number and $q=-\pi$ for $z$ equal to a negative real number. So
defined, the function has a branch cut along the negative real axis
(this is merely a convention).  Equating imaginary parts in the
preceding equation, we then have

$$
\begin{eqnarray}
\theta = \Im\left\{\mbox{Log}(x+iy)\right\}.
\end{eqnarray}
$$  (eq247)

and so may write the four-quadrant arctangent for $\psi_{uh}$ as

$$
\begin{eqnarray}
\psi_{uh}~\mbox{[deg]} = \frac{180}{\pi} \cdot \Im\left\{\mbox{Log} \left({\bf\hat{h}^c} \cdot {\bf \tilde{h}^{u}} - i\left({\bf\hat{h}^c} \cdot {\bf \tilde{v}^{u}}\right)\right)\right\}.
\end{eqnarray}
$$  (eq248)

If $\psi_{uh}<0$ then $\psi_{uh} = \psi_{uh}+360$, and this wrapping
is conveniently expressed using the MATLAB mod() function, so that
$\psi_{uh}=\mbox{mod}(\psi_{uh},360)$ The angle actually used in the lookup
table interpolation is then conveniently expressed as

$$
\begin{eqnarray}
\psi^{lut}_{uh}~\mbox{[deg]} = \mbox{mod}\left(\frac{180}{\pi} \cdot \mbox{atan2}\left(-{\bf\hat{h}^c} \cdot {\bf \tilde{v}^{u}},{\bf\hat{h}^c} \cdot {\bf \tilde{h}^{u}}\right),360\right).
\end{eqnarray}
$$  (eq249)

or as

$$
\begin{eqnarray}
\boxed
{
\psi^{lut}_{uh}~\mbox{[deg]} = \mbox{mod}\left(\frac{180}{\pi} \cdot \Im\left\{\mbox{Log} \left({\bf\hat{h}^c} \cdot {\bf \tilde{h}^{u}} - i\left({\bf\hat{h}^c} \cdot {\bf \tilde{v}^{u}}\right)\right)\right\},360\right).
}
\end{eqnarray}
$$  (eq250)

The five-dimensional harmonic coefficient functions can be expressed
as

$$
\begin{eqnarray}
\tilde{A}^{(0)}_p(\alpha_s,\delta_s,\theta_s,\psi_{uh},u_{10}) &=& {\cal I}\left(\tilde{a}''^{(0)}_{p}({\bf\tilde{G}_{nm}}) \cdot T({\bf\tilde{G}_{nm}}),{\bf\tilde{G}_{nm}}\right),\\
\tilde{A}^{(2)}_p(\alpha_s,\delta_s,\theta_s,\psi_{uh},u_{10}) &=& {\cal I}\left(\tilde{a}''^{(2)}_{p}({\bf\tilde{G}_{nm}}) \cdot T({\bf\tilde{G}_{nm}}),{\bf\tilde{G}_{nm}}\right),\\
\tilde{B}^{(2)}_p(\alpha_s,\delta_s,\theta_s,\psi_{uh},u_{10}) &=& {\cal I}\left(\tilde{b}''^{(2)}_{p}({\bf\tilde{G}_{nm}}) \cdot T({\bf\tilde{G}_{nm}}),{\bf\tilde{G}_{nm}}\right),
\end{eqnarray}
$$  (eq251)


where ${\cal I}$ is a discrete integration operator that integrates
products of discrete galactic noise $T({\bf\tilde{G}_{nm}})$ with
(appropriately rotated) discrete scattering cross sections
$\tilde{a}''^{(0)}_{p}({\bf\tilde{G}_{nm}})$,
$\tilde{a}''^{(2)}_{p}({\bf\tilde{G}_{nm}})$, and
$\tilde{b}''^{(2)}_{p}({\bf\tilde{G}_{nm}})$. The $p$ subscript
emphasizes the fact that there is one set of three five dimensional
functions for each polarization $p$.  To construct a lookup table, we
must discretize all five dimensions. By analyzing the dependence on
each dimension, and weighing the accuracy constraint with constraints
imposed by computational resources, we have determined that a
reasonable discretization for wind speed and incidence angle is the
following:

$$
\begin{eqnarray}
\left\{u_{10}\right\} &=& \left\{3,5,7,10,15,25\right\},\\
\left\{\theta_s\right\} &=& \left\{0,10,20,25,30,35,40,45,50,55,60,70,80\right\}.
\end{eqnarray}
$$  (eq252)

The grid for $\psi_{uh}$ is regular and defined (in degrees, mathematical
convention) by the set

$$
\begin{eqnarray}
\left\{-\psi_{uh}\right\} = \left\{22.5n\right\},
\end{eqnarray}
$$  (eq253)

where n is a integer ranging from 0 through 16.  The grid for the
specular right ascension $\alpha_s$ is regular and defined (also in
degrees) by the set

$$
\begin{eqnarray}
\left\{\alpha_s\right\} = \left\{3.75n\right\},
\end{eqnarray}
$$  (eq254)

where n is a integer ranging from 0 through 96.  Finally, the grid for
the specular declination $\delta_s$ is regular and defined (in
degrees) by the set

$$
\begin{eqnarray}
\left\{\delta_s\right\} = \left\{3.75n\right\},
\end{eqnarray}
$$  (eq255)

where n is a integer ranging from 0 through 96.  It should be noted
that the lookup table is defined in B1950 celestial coordintes, not
the J2000 coordinate system. The lookup table is stored in a MATLAB
Version 7 file. Table~\ref{tab_glut} lists the correspondence between
variable names and quantities described above.


| MATLAB Variable Name               |      Physical Quantity                                                      | Independent Variables   | 
| :- | -: | :-: |
|dec\_b1950                          |      B1950 declination $\delta_s$ [$^\circ$]                                | $\delta_s$              |
|ra\_b1950                           |      B1950 right ascension $\alpha_s$ [$^\circ$]                            | $\alpha_s$              |
|ws                                  |      10-m wind speed [m~s$^{-1}$]                                           | $u_{10}$                |
|eia                                 |      radiometer incidence angle [$^\circ$]                                  | $\theta_s$              |
|psi                                 |      $\psi^{lut}_{uh}=-\psi_{uh}$ [$^\circ$]                                | $\psi^{lut}_{uh}$             |
|th\_symm                            |      $\tilde{A}^{(0)}_h$: symmetric H-pol component [K]                     | $(\delta_s,\alpha_s,u_{10},\theta_s,\psi^{lut}_{uh})$ |
|tv\_symm                            |      $\tilde{A}^{(0)}_v$: symmetric V-pol component [K]                     | $(\delta_s,\alpha_s,u_{10},\theta_s,\psi^{lut}_{uh})$ |
|th\_hc                              |      $\tilde{A}^{(2)}_h$: $\cos(2\varphi'_w)$ harmonic amplitude H-pol [K]  | $(\delta_s,\alpha_s,u_{10},\theta_s,\psi^{lut}_{uh})$ |
|tv\_hc                              |      $\tilde{A}^{(2)}_v$: $\cos(2\varphi'_w)$ harmonic amplitude V-pol [K]  | $(\delta_s,\alpha_s,u_{10},\theta_s,\psi^{lut}_{uh})$ |
|th\_hs                              |      $\tilde{B}^{(2)}_h$: $\sin(2\varphi'_w)$ harmonic amplitude H-pol [K]  | $(\delta_s,\alpha_s,u_{10},\theta_s,\psi^{lut}_{uh})$ |
|tv\_hs                              |      $\tilde{B}^{(2)}_v$: $\sin(2\varphi'_w)$ harmonic amplitude V-pol [K]  | $(\delta_s,\alpha_s,u_{10},\theta_s,\psi^{lut}_{uh})$ |



SubSubsection Text
##### Input data

SubSubsection Text

##### Output data

SubSubsection Text

##### Auxiliary data

SubSubsection Text

##### Ancillary data

SubSubsection Text

##### Validation process

SubSubsection Text






