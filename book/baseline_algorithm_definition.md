# Baseline Algorithm Definition

Definition

### Retrieval Method

Subsection Text


### Forward Model

SSS retrieval over a specular sea surface 

from IPython.display import Image
Image(url= "images/no_regrets_cat.jpg", width=400, height=400)

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

 Models of the Dielectric Constant of seawater at 1.4 GHZ

The model function $ε_{sw}(f, S, T)$  is a key component of the radiative transfer forward model used for sea surface salinity retrieval from L-band radiometer data. The relative permittivity (also called dielectric constant) of the seawater, is a complex function dependent on temperature and salinity. The accuracy of SSS retrievals from L-band $T_B$ strongly depends on how well the dielectric constant is known as a function of these two geophysical parameters (Lang et al., 2016). It can be estimated at any frequency within the microwave band from the Debye (1929) expression:
 
 $ε_{sw}(f, S, T)=\displaystyle ε_{\infty}+\frac{(ε_{s}(S,T)-ε_{\infty})}{1+i \omega \tau(f,S,T)}-i\frac{\sigma(f,S,T)}{\omega ε_0}$
 
 
in which $\it{i}$ is the imaginary unit, $ε_{\infty}$ is the electrical permittivity at very high frequencies, $ε_{s}$ is the static dielectric constant, $\tau$ is the relaxation time, $\sigma$ is the ionic conductivity, and $ε_0$ is the permittivity of free space, where $ε_{s}$, $\tau$ and $\sigma$ are functions of $\it{T}$ and $\it{S}$. At the time the first salinity mission was developed, these functions had been evaluated historically by Stogryn (1971, 1995), Klein and Swift (1977), and Ellison et al. (1998). Klein and Swift (denoted KS hereafter) modified the Stogryn (1971) model by using a different expression for the static dielectric constant $ε_{s}(S,T)$, based on Ho and Hall (1973) and Ho et al. (1974) measurements at 2.6 and 1.4 GHz, respectively. The KS and Stogryn $ε_{sw}$ models are valid for frequencies ranging from L- to X-bands (Meissner and Wentz, 2004; 2012;Meissner et al., 2014). Following pre-launch comparisons and analyses (Camps et al., 2004; Wilson et al., 2004, Blanch and Aguasca, 2004), the KS model was selected in the Level 2 Ocean Salinity (OS) processor (SMOS-Ocean Expert Support Laboratories, 2016).  
 	An alternative model function developed by Meissner and Wentz (2004, MW hereafter) fits the dielectric constant data to a double Debye relaxation polynomial that performs best at higher frequencies. The seawater dielectric data were obtained by inverting TB measurements from the Special Sensor Microwave Imager (SSM/I) at frequencies higher than 19 GHz; measurements from Ho et al. (1974) were used to derive the model at the lower frequencies.  The MW model function was recently updated by providing small adjustments to the Debye parameters based on including results for the C-band and X-band channels of WindSat and AMSR (Meissner and Wentz, 2012, 2014).  The MW model is used in the Aquarius and SMAP SSS retrieval algorithms (Meissner et al. 2018). 
Dinnat et al. (2014) analyzed the difference in SSS retrieved by SMOS and Aquarius radiometers and found that both instruments observe similar large scale patterns, but also reported significant regional discrepancies (mostly between +/- 1 pss). SMOS SSS was found generally fresher than Aquarius SSS (within 0.2-0.5 pss depending on latitude and SST), except at the very high southern latitudes near the ice edge and in a few local (mostly coastal) areas. It was found that the differences exhibit large-scale patterns similar to SST variations. To investigate its source, Dinnat et al. (2014) reprocessed the Aquarius SSS, including the calibration, using the KS ε_sw  model that is used in SMOS processing. This reprocessing decreases the difference between Aquarius and SMOS SSS by a few tenths of a pss for SST between 6°C and 18°C while warmer waters show little change in the difference. Water colder than 3°C shows mixed results, probably due to a complex mix of error sources, such as the presence of sea ice and rough seas.  The comparison of the reprocessed Aquarius SSS with in situ data from Argo shows an improvement of a few tenths of a pss for temperatures between 6°C and 18°C. In warmer waters, both the nominal and reprocessed Aquarius data, as well as SMOS data, have a fresh SSS bias. For very cold waters (less than 3°C), the reprocessed Aquarius data using the KS model show significant degradation of the SSS in comparison with the Argo, in turn suggesting that the KS model might be in error in the lowest sea surface temperature regime.
Direct laboratory measurements of the ε_sw at 1.413 GHz and SSS=30, 33, 35, and 38 (Lang et al., 2016) were used to develop a new model (Zhou et al., 2017) by fitting the measurements with a third-order polynomial. This new L-band ε_sw model has been compared with KS and MW.  The authors claimed that this new model function (see Figure 9) gives more accurate SSS at high (25°C to 30°C) and low (0.5°C to 7°C) SSTs than other existing model functions. Laboratory measurements at low SSS lead to a small increase in the accuracy of the model function.  New measurements at 34 and 36 pss have been recently made and will be incorporated in the model function. This work is still in progress.
Despite its importance for SSS remote sensing, uncertainties remain in the 1.4 GHz seawater dielectric constant model. The new laboratory measurements combined with satellite data, will certainly help to minimize those remaining uncertainties.





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


