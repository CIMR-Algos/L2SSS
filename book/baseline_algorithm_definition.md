# Baseline Algorithm Definition

Definition

### Retrieval Method

Subsection Text


### Forward Model

Perfectly Flat sea surface emission

The brightness temperature emitted by the sea surface can be expressed as the sum of two terms; the brightness temperature in the case of completely flat sea and the additional brightness temperature (Tb) due to the surface roughness, as follows:

[4.1.1]

The first term is Tb due to the emission of a flat sea surface, which is well described by the Fresnel equations and is polarization dependent (p). The second term is the increment of brightness temperature due to sea roughness, which can be described through several parameters (Prough) related to processes that modify this roughness. θ is the angle under which Tb is measured, SST is the sea surface temperature and SSS the sea surface salinity. ,

The brightness temperature is defined as:

 				           	       [4.1.2]


where e(θ) is the surface emissivity at L-Band which carries the major information regarding SSS. Assuming thermodynamical equilibrium, the Kirchhoff laws applies and emissivity is considered equal to absorption, and equal to 1 – reflectivity.

It can be written, as follows:

 			       [4.1.3]

where  is the reflectivity, which is dependent on the incident radiation nadir angle , on the complex dielectric constant of sea water  (’+j’’), the azimuth angle , the roughness and the polarisation.

In the case of a smooth surface sea, the reflectivity can be calculated straighforward using the Fresnel reflection laws, and providing an accurate dielectric constant model.

The Fresnel reflection coefficients R, for each polarisation, are defined as function of the sea water dielectric constant and the incidence angle, as follows:

  			       [4.1.4]

Therefore Th and Tv for a flat surface are computed as:


 			          [4.1.5]


The complex dielectric constant of the sea water is dependent on temperature and on the concentration of salt. It can be calculated at any frequency, within the microwave band, from Debye (1929) expression:

 				         [4.1.6]
in which i is the imaginary number,  is the electrical permittivity at very high frequencies, s is the static dielectric constant,  is the relaxation time,  is the ionic conductivity, and 0 is the permittivity of free space. s,  and  are functions of the temperature and salinity of sea-water, and have been evaluated by Klein and Swift (1977), Ellison et al. (1998) and Blanch and Aguasca (2004).

After some comparisons and analysis (Camps et al., 2004; Wilson et al., 2004), the Klein and Swift dielectric constant model has been agreed to be the model that better expresses this parameter and is the one presented here.

The term Tbrough is described by 3 different models in sections 4.2, 4.3 and 4.4.

4.1.1.2. Mathematical description of algorithm

Some of the parameters in eq. 4.1.4 can be expressed through polynomial functions of salinity (SSS in the case of SMOS) in psu (practical salinity units, UNESCO, 1978) and water temperature in ºC

       	 		[4.1.7]

           		 		[4.1.8]

       		  		[4.1.9]

The values of these parameters are provided in SMOS TGRD document. The present version considers the model provided by Klein and Swift (1977), but this will be modified if a more accurate model is available before SMOS launch


The dielectric constant  is computed following equation [4.1.6] and a complex value results.
 
4.1.1.3. Error budget estimates (sensitivity analysis)

  has an error of 20% but this is negligible at L-band.

Cox quotes that the ionic conductivity of the sea water, , has an error of  0.03% for salinities between 30 and 40 psu, which is also negligible.

The static permittivity, s, has a maximum per cent error of 0.49 with respect the measurements, and an average per cent error of 0.11.

The relaxation time, , has been derived from measurement with an accuracy of 2.12*10-13 and this is the assumed error for that parameter.

Ho’s estimated error for ’ is 0.2%.

Taking  =75 + j42, which is the approximate value of the dielectric constant of the sea water at 1.43 GHz when SSS=20 psu and T=20ºC, it then follows that the error associated with this particular choice is:

  1.15(’+’’)10-3

Using the above mentioned values,  ’  0.15 and ’’  0.13. Hence, the error in the brightness temperature with T=293K is

Tb = e*T  0.09 K

4.1.2. Practical considerations
4.1.2.1. Calibration and validation

S. Blanch, from the Polytechnic University of Catalonia (UPC), is preparing a new laboratory experiment to compute the parameters that describe the dielectric constant following the Debye expression. Results of this experiment will be used to validate the Klein and Swift model. If any change or tuning of the parameters is necessary this should be applied to the model, and the corresponding coefficients file modified accordingly.
4.1.2.2. Quality control and diagnostics

We expect that the SMOS Cal/Val activities will provide, after commissioning phase, further information on the quality of the model, and if necessary new values to improve it

4.1.2.3. Exception handling
There is no particular exception handling in the mathematical algorithm except if the following auxiliary data are not provided by the processor or exceed the ranges: SST, SSS,   incidence angles at SMOS pixel. In that case the computation could not be done.

4.1.3. Assumption and limitations

The measurements on which the Klein & Swift model has been based, were obtained from NaCl solutions and some from real sea water samples. Few measurements were done on the salinity range from 30-40 psu, which are the most common values in the world’s ocean. However the authors confirm that the model should be valid for sea waters that have a salinity range between 3 and 35 psu. However, until a new model is established and validated, the SMOS and Aquarius communities agreed on using Klein and Swift.



 




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


