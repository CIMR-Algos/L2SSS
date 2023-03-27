# Level-2 product definition

The Level-2b SSS product is obtained through application of the SSS retrieval algorithm, given the Level-1b TBs. 
The product is provided in the L-band grid of the CIMR instrument ($xdim_{grid},ydim_{grid}$). There is also one SSS retrieval per CIMR 
look direction (aft and fore views).
 The product is provided in netCDF format and contains  the variables listed in {numref}`L2PD`.

```{table} Level-2 product definition
:name: L2PD

 |     name |  description |  Units | dimensions |
 | ---------| ---------------------| ---------------------|---------------------| 
 | time     | seconds of observation since YYYY-MM-DD 00:00:00 UTC. | seconds | $[look,xdim_{grid},ydim_{grid}]$ |
 | lon | longitude. range: $[0^{\circ}, 360^{\circ}]$ | degrees East | $[look,xdim_{grid},ydim_{grid}]$ |
 | lat | latitude. range: $[90^{\circ}S, 90^{\circ}N]$ | degrees North | $[look,xdim_{grid},ydim_{grid}]$ |
 |sea_surface_salinity | sea surface salinity | pss | $[look,xdim_{grid},ydim_{grid}]$ |
 |sea_surface_salinity uncertainty | sea surface salinity uncertainty | pss | $[look,xdim_{grid},ydim_{grid}]$ |
 |sea_surface_sainiy quality level | Flag indicating the quality level of the SSS retrieval | n/a | $[look,xdim_{grid},ydim_{grid}]$ |
 |sea_surface_temperature | sea surface tempearture | Kelvin | $[look,xdim_{grid},ydim_{grid}]$ |
 | wind_speed | sea surface wind speed | m/s | $[look,xdim_{grid},ydim_{grid}]$ |
 | wind_direction | sea surface wind direction | meteorological convention. $0^{\circ}$: wind coming out of N, +90$^{\circ}$ : wind coming out of E, etc. range: $[0^{\circ},+360^{\circ}]$. | $[look,xdim_{grid},ydim_{grid}]$ |
 ```
 