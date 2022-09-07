# Baseline Algorithm Definition

Definition

### Retrieval Method

Subsection Text


### Forward Model

SSS retrieval over a specular sea surface 

The dependence of $T_{B}$ on SSS is contained in the emissivity, e: $T_{B} = T \times e$, where $\it{T}$ is the sea surface temperature. The emissivity is a quantity that depends on physical and chemical properties of the water (e.g. salinity and temperature), observational conditions (incidence angle, electromagnetic frequency, polarization, as well as sea surface roughness. For a perfectly flat ocean surface with salinity, $\it{S}$, temperature, $\it{T}$, and observed at incidence angle $\theta$, the emissivity at polarization, $\it{p}$ (horizontal or vertical), and electromagnetic frequency, $\it{f}$, (note that we quote the center of a microwave frequency bandwidth associated with a given radiometer) is given by Peake (1959):

$e_{p} (\theta,f,S,T)=1-|R_{p} (\theta,f,S,T)|^2$

where $R_p$  is the Fresnel reflection coefficient given by :

$R_v (θ,f,S,T)=\displaystyle\frac{\sqrt{ε_{sw}-\sin^2⁡{\theta}}-ε_{sw} cos\theta}{\sqrt{ε_{sw}-\sin^2{\theta}}+ε_{sw} \cos{\theta}}$ for vertical polarization, and,

$R_h (θ,f,S,T)=\displaystyle\frac{\sqrt{ε_{sw}-\sin^2⁡\theta}-cos\theta}{\sqrt{ε_{sw}-sin^2⁡θ}+cos\theta}$  for horizontal polarization.

In the expression above, $ε_{sw}(f, S, T)$ is the dielectric constant of sea water.  In its simplest form, SSS remote sensing, therefore, consists of measuring the L-band $T_{B}$ together with SST. The intersection of the two values on a graph such as shown in Figure 3.b can be used to retrieve SSS.  It is also important to note that the penetration depth of electromagnetic wave into the sea water (defined as the e‐folding attenuation of the energy flux transported by electromagnetic radiation into water, see Ulaby et al., 1986) is less than 1 cm at 1.4 GHz for typical open ocean salinity and temperature. Given that the majority of in situ measurements of SSS are obtained at depth ranging from 0.5 to 20 meters (e.g., Argo floats, TSG, surface drifters, etc..), understanding the vertical distributions of salinity in the upper ocean is critical for many applications. These applications include the assimilation of satellite SSS in numerical ocean models which requires dedicated treatment of shallow stratifications (Large and Caron, 2015; Fine et al., 2015) and the  validation of satellite SSS with in situ data (Boutin et al. 2016).
There are also other issues that complicate the remote sensing of salinity from space. Several geophysical parameters other than seawater salinity and temperature contribute significantly to L-band $T_B$ measured by satellite sensors (e.g., see Yueh et al., 2001; Font et al., 2004). These contributions need to be accurately known and used in corrections of measured antenna TB to properly retrieve SSS. As illustrated in Figure 8,  they include:  the direct and earth-reflected solar and sky emission (Le Vine et al. 2005; Reul et al., 2007, 2008; Tenerelli et al., 2008; Dinnat and Le Vine, 2008), the Faraday rotation in the ionosphere (Yueh et al., 2000; Le Vine and Abraham, 2002; Vergely et al., 2014),  the impact of the atmosphere (Liebe et al., 1992; Skou et al., 2005; Wentz and Meissner, 2016), and the effect of sea surface roughness on L-band emissivity (Meissner et al., 2014, 2018; Yin et al. 2016; Yueh et al., 2010, 2014).   For all L-band radiometers, SSS retrievals algorithms are therefore based on:   
	
- an empirical sea-water dielectric constant model at 1.4 GHz,
-  a surface roughness correction model,
-  a Radiative Transfer Model for Atmospheric corrections,
-  a scattering model to correct for sea surface scattered Solar and celestial radiation, and, 
-  a model to correct for Faraday rotation and geometric rotation associated with the finite beam width of the antenna.

We review these models and corrections in the following subsections.

 




### CIMR Level-1b re-sampling approach

Subsection Text


### Algorithm Assumptions and Simplifications

Subsection Text

### Level-2 end to end algorithm functional flow diagram

Subsection Text

### Functional description of each Algorithm step

Subsection Text

##### Mathematical description

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


