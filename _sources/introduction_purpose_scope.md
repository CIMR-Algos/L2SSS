# Introduction, purpose and scope

The purpose of this Algorithm Theoretical Baseline Document (ATBD) is to establish the procedure that will be used in the CIMR mission to generate 
the Sea Surface Salinity (SSS) data from brightness temperatures (Tb) recorded by the CIMR L-band radiometer.  The historical satellite SSS "Background" and justification of selected algorithm are first provided. 
As described in the "Level-2 product definition" section, the output product (CIMR SSS Level 2 SSS product) will consist of files containing half-orbit data (from pole to pole) 
on a geographical grid (e.g. EASE2-like for Polar Regions, Equal Area Cylindrical projection) defined by the level 1c grid, itself defined by
 the CIMR Leve1b re-sampling approach.


As it is not possible to transform the Tb into SSS through a univocal mathematical expression. L-band emission of the sea surface is computed using a 
series of mathematical models that have as independent variables the different geophysical parameters (including SSS) that determine this emission. 
Some of these parameters are obtained from sources external to CIMR or from CIMR data directly (SST, wind,..), and for SSS a guessed value is considered. The computed Tb, 
for all observation configurations that correspond to the specific satellite passage, are compared to the measured ones, 
and then the independent variables are modified in an iterative process until reaching the maximum similarity between both Tb values. 
The SSS that corresponds to this situation is the value retrieved from CIMR.

In the "Baseline Algorithm Definition" section,  a detailed description of all the parts of the algorithm, the different modules or sub-models that are used to compute the different contributions to sea surface 
Tb as well as the procedures to compare it with the measured Tb and the iterative convergence method. 

In the Algorithm Input and Output Data Definition, we define all needed input and output data.

The Intergovernmental Oceanographic Commission adopted the new International Thermodynamic Equation of Seawater - 2010 that describes salinity through the Absolute Salinity definition in g/kg instead of Practical Salinity (PS Scale - 78) to take into account the spatial variability of 
seawater composition and to use IS units. However, measuring systems, both in situ and remotely sensed, will continue being based on conductivity, so all instruments and data bases will deliver practical salinity as before. For historical reasons, all the SSS data processing algorithms 
presented here use practical salinity regardless of labels in different modules (no units/psu/pss/pss-78). 
CIMR is a data provider and users may apply the new TEOS-2010 formulae to convert conductivity-based practical salinity to the more correct Absolute Salinity values when appropriate.
