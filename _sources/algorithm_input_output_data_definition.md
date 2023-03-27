# Algorithm Input and Output Data Definition (IODD)






### Input data

```{table} Input Data
:name: InpData
|Quantity                    | Description                      |  Sources      |        
| :-: | :-: | :-: |
|L1b TB at L-band |  L1B Brightness Temperatures at 1.4 GHz (V, H, 3$^{rd}$ Stokes)          | CIMR L1 ground segment   |
| Level-2b SST product | Sea Surface Temperature (optional) | CIMR L2 ground segment   |
| Level-2b SWS product | Sea Surface Wind speed  and direction (optional) | CIMR L2 ground segment   |
| Level-2b SIC product | Sea Ice concentration (optional) | CIMR L2 ground segment   |
```



### Output data
```{table} Output Data
:name: OutData
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

### Auxiliary data

Auxiliary Data Required for Forward Model

```{table} Auxilliary Data
:name: AuxData
|Quantity                    | Required For                      |  Sources              |        
| :-: | :-: | :-: |
|sea surface salinity        | spec. emission                    | WOA, CMEMS, ISAS      |
|sea surface temperature     | spec. emission                    | CIMR, ECMWF           |
|surface pressure            | atmos. model                      | ECMWF,                |
|2-m temperature             | atmos. model                      | ECMWF,                |
|columnar vapor              | atmos. model                      | ECMWF,                |
|10-m NE wind speed          | glint, rough sfc. emission        | CIMR,ECMWF, Metop-SG|
|10-m wind direction         | rough sfc. emission               | CIMR,ECMWF, Metop-SG  |
|mispointing angles          | geometry initialization           | CIMR L1 ground segment   |
|best fit plane angles       | geometry initialization           | CIMR L1 ground segment    |
|time correlations           | EO-CFI initialization             | CIMR L1 ground segment|
|solar flux                  | TEC alt. correction, sun glint    |  RSGA files   |
|ORBSCT file                 | EO-CFI                            |  RSGA files   |
|wind GMF                    | wind speed inversion              | internal file        |
|OTT bias correction         | Stokes vector bias correction     | CIMR SSS processor  |
|LSC correction              | land sea contamination correction | internal file        |
| distance to nearsest coasts | LSC flagging                     | CIMR L1 ground segment |
```


### Ancillary data

Subsection text

