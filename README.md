# SPEIbase [![DOI](https://zenodo.org/badge/97464490.svg)](https://zenodo.org/badge/latestdoi/97464490)

R code used in generating the SPEI global database.


## Details

The SPEI global database, http://spei.csic.es/database.html, offers long-time,
robust information about drought conditions at the global scale, with a 0.5 
degrees spatial resolution and a monthly time resolution.
It has a multi-scale character, providing SPEI time-scales between 1 and 48 
months. 
Currently it covers the period between January 1901 and December 2015.

The data are accesible from the SPEI web site, http://spei.csic.es, but we
have shared the R code on this repository to allow anyone to reproduce the
dataset.

If you use this code or parts of it, it'd be nice if you cite the repo as:
* **Beguería S. (2017) SPEIbase: R code used in generating the SPEI global database, doi:10.5281/zenodo.834462**.

If you use the SPEI dataset in your resarch, please cite the following papers:
* **Vicente-Serrano S.M., Santiago Beguería, Juan I. López-Moreno, (2010) A Multi-scalar drought index sensitive to global warming: The Standardized Precipitation Evapotranspiration Index - SPEI. Journal of Climate 23: 1696-1718.**
* **Beguería, S., Vicente-Serrano, S.M. y Angulo, M., (2010): A multi-scalar global drought data set: the SPEIbase: A new gridded product for the analysis of drought variability and impacts. Bulletin of the American Meteorological Society. 91, 1351-1354**
* **Beguería, S., Vicente-Serrano, S.M., Fergus Reig, Borja Latorre. Standardized Precipitation Evapotranspiration Index (SPEI) revisited (2014): parameter fitting, evapotranspiration models, kernel weighting, tools, datasets and drought monitoring. International Journal of Climatology, 34: 3001-3023**


## Using the code

The script `R/computeSPEI.R` computes the global SPEI dataset at different
time scales. One netCDF file covering the whole globe and time period is
generated for each time scale, e.g. `spei01.nc` for a time scale of 1 month,
etc. Output files are stored on `/outputNcdf`.
These are the global files that can be downloaded from http://spei.csic.es/database.html.

Before running the script it is necesary to replace the fake files in the
`/inputData` directory with the real ones containing the data.
These can be downloaded from the website of the Climatic Research Unit (CRU),
University of East Anglia.

The script `R/outputTxt` generates, from the .nc files computed previously,
additional files containing the SPEI time series at the scales between 1 and
48 months, for every single cell in the dataset.
These files are stored in plain text, comma-separated, files, and stored
under `/outputTxt`.
The files names (e.g. `spei_-0.25_5.25.csv.gz`) contain the central coordinates
of the grid cell.
These are the files that can be downloaded as 'single location' data at
http://spei.csic.es/database.html.



## Using the data

The output files are in netCDF v4 format, which allows for data compression.
The files can be read in R using the ncdf4 package.

However, some widely used GIS packages do not provide suuport yet for netCDF v4.
It is possible to convert the files to the netCDF v3 format used in many
GIS softwares, by using the nccopy program by unidata.
For instance, to convert a netCDF-4 format file foo4c.nc to file foo3.nc in
netCDF-3 you can use:

```
nccopy -k classic foo4c.nc foo3.nc.
```

Another way would be to install the netCDF operators (NCO) toolset from unidata,
and then use the ncks utility:

```
ncks -3 foo4c.nc foo3.nc.
```

In fact, ncks allows for much greater functionality.
For example, if one wants to extract the first 100 times from an SPEIbase file:

```
ncks -d time,0,100 spei_12.nc output_file.nc
```

would generate a (smaller) netCDF file with only those timesteps.
In a similar fashion, it is possible to use ncks to select a specific
geographical region, and other useful options.


## Version history

* SPEIbase v2.5: 1) Based on the CRU TS 3.24.01 dataset, extending the temporal
range of the SPEIbase up to December 2015. 2) Corrected an important bug on
versions 2.2 to 2.4 of the dataset that prevented correctly reading the ETo data
in mm/month.
* SPEIbase v2.4: 1) Based on the CRU TS 3.23 dataset, extending the temporal 
range of the SPEIbase up to December 2014.
* SPEIbase v2.3: 1) Based on the CRU TS 3.22 dataset, extending the temporal 
range of the SPEIbase up to December 2013.
* SPEIbase v2.2: 1) The CRU TS 3.2 dataset has been used, extending the data 
range of the SPEIbase up to December 2011. 2) Potential evapotranspiration data 
from the CRU TS 3.2 dataset has been used instead of computing our own.
* SPEIbase v2.1: 1) The revised dataset CRU TS 3.10.01 for precipitation is 
used. 2) Data on surface pressure and wind from 20th Century Reanalysis v.2 
until December 2009 has been used for computing Penman PET. 3) An error while 
reading some data sources that yielded no data at longitudes 179.25 and 179.75 
has been corrected.
* SPEIbase v2.0: 1) Data has been extended to the period 1901-2010. 2) The 
FAO-56 Penman-Monteith's method has been used for computing PET instead of 
Thornthwaite in SPEIbase v1.0. 3) Unbiased probability weighted moments (ub-pwm) 
method has been used for fitting the log-Logistic distribution, instead of the 
sub-optimal plotting-position pwm method used in version 1.0. 4) The whole 
World is put in one single netCDF file.
* SPEIbase v1.0: The global gridded SPEI dataset is available at time scales 
between 1 and 48 months, with spatial resolution of 0.5º lat/lon and temporal 
coverage between January 1901 and December 2006.


## Got problems?

Feel free to [write an issue](https://github.com/sbegueria/SPEIbase/issues)
if you have any questions or problems.


## Copyright and license

This program is free software: you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free Software
Foundation, either version 3 of the License, or (at your option) any later
version.
