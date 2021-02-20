# Quick overview

---
## Data

### Working with

Suggest using [xarray](http://xarray.pydata.org/en/stable/) to open and work with data.

Code block shows an example of how to open and view 

```python
import numpy as np
import xarray as xr

forecast_date = "YYYYMMDDHH"
domain        = 'd02'        ## or 'd03'
name 	      = 'daily'      ## or 'hourly'

file_dir = str(/path/to/dir/) + f"/fwf-{name}-{domain}-{forecast_date}.zarr"
ds       = xr.open_zarr(file_dir)

## Example: look at variable P (Duff Moisture Code)
print(ds.P)
```
```
<xarray.DataArray 'P' (time: 2, south_north: 417, west_east: 627)>
dask.array<xarray-P, shape=(2, 417, 627), dtype=float32, chunksize=(1, 209, 314), chunktype=numpy.ndarray>
Coordinates:
    Time     (time) datetime64[ns] dask.array<chunksize=(2,), meta=np.ndarray>
    XLAT     (south_north, west_east) float32 dask.array<chunksize=(209, 314), meta=np.ndarray>
    XLONG    (south_north, west_east) float32 dask.array<chunksize=(209, 314), meta=np.ndarray>
    XTIME    (time) float32 dask.array<chunksize=(2,), meta=np.ndarray>
Dimensions without coordinates: time, south_north, west_east
Attributes:
    FieldType:    104
    MemoryOrder:  XY 
    description:  DUFF MOISTURE CODE
    projection:   PolarStereographic(stand_lon=-110.0, moad_cen_lat=53.25, tr...
    stagger: 
```

### Datasets and Variables
domain:
- d02 (12 km) 
- d03 (4 km)


| Hourly Dataset <br> `fwf-hourly-<domain>-YYYYMMDDHH.zarr`  | Daily Dataset <br> `fwf-daily-<domain>--YYYYMMDDHH.zarr`  | 
 --------------------------- | ------------------------- |
|**Time**: Hourly UTC  |**Time**: Noon Local for that Day |
|**XLAT**: Degrees Latitude  |**XLAT**: Degrees Latitude|
|**XLON**: Degrees Longitude  |**XLON**: Degrees Longitude|
|**F**: Fine Fuel Moisture Code  |**P**: Duff Moisture Code  |
|**m_o**: Fine Fuel Moisture Content  |**D**: Drought Moisture Code  |
|**R**: Initial Spread Index   |**U**: Build Up Index   |
|**S**: Fire Weather Index| Bu**T**: 2 meter Temperature C |
|**DSR**: Daily Severity Rating  | **TD**: 2 meter Dew Point Temperature C |
|**T**: 2 meter Temperature C  | **H**: 2 meter Relative Humdity %  |
|**TD**: 2 meter Dew Point Temperature C  | **W**: 10 meter Wind Speed km/h |
|**H**: 2 meter Relative Humdity % | **WD**: 10 meter Wind Direction deg  |
|**W**: 10 meter Wind Speed km/h  | **r_o**: Total Accumulated Precipitation mm  |
|**WD**: 10 meter Wind Direction deg  | **r_o_tomorrow**: Carry Over Precipitation mm  |
|**U10**:  U Component of Wind at 10 meter m/s  |**SNOWC**: Flag Inidicating Snow <br> Cover (1 for Snow Cover) Snow Depth m   |
|**V10**: V Component of Wind at 10 meter m/s  |   |
|**r_o**: Total Accumulated Precipitation mm  |   |
|**r_o_hourly**: Hourly Accumulated Precipitation mm  |  |
|**SNW**: Total Accumulated Snow cm  |   |
|**SNOWH**: Physical Snow Depth m  |   |
|**SNOWC**: Flag Inidicating Snow <br> Cover (1 for Snow Cover) Snow Depth m  |   |


<br/>
<br/>


---
## Model Domains

The FWF model resolves the FWI System in the D02 (12 km) and D03 (4 km) at 55 hour forecast horizon.
![original image](https://cdn.mathpix.com/snip/images/S8MbCC7tE3d9-BN7CP1V6Atf7LAFMEvMm6vjNYoItc8.original.fullsize.png)
