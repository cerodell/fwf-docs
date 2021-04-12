Structure
============

Flow Chart
------------
.. image:: _static/images/fwf-model-flowchart.png    
   :width: 600
   :align: center


File Directories
------------------
master
******

``/bluesky/fireweather/fwf/master`` is the operational directory, its where the code to run the model resides.
    - all_fwf_run.sh 
    - run.py 
    - geojson_maker.py 
    - ds2json.py 
    - geo2topo_mv.sh
    - index_generator.py
    - build_intercomparison_data.py
    - intercomp_json.py

utils
******
``/bluesky/fireweather/fwf/utils``  is the model code  directory, all the scripts in ``master`` call on class methods and other functions in this directory.
    - fwf.py
    - read_wrfout.py
    - geoutils.py

data
******
``/bluesky/fireweather/fwf/data``   is the current forecast data directory, the directory has subfolders for each data type. 
    - the key subfolder is ``/xr``  the current fwf forecast zarr files live there.
    - the current fwf zarr forecast data is broken up into two groups ``hourly`` and ``daily`` the table below shows whats in each group.


+---------------------------------------------------+-------------------------------------------------+
| **Hourly Dataset** ``fwf-hourly-YYYYMMDDHH.zarr`` | **Daily Dataset** ``fwf-hourly-YYYYMMDDHH.zarr``| 
+===================================================+=================================================+
| Fine Fuel Moisture Code **FFMC**                  | Duff Moisture Code **DMC**                      |
+---------------------------------------------------+-------------------------------------------------+
| Initial Spread INdex **ISI**                      | Drought Moisture Code **DC**                    |
+---------------------------------------------------+-------------------------------------------------+
| Fire Weather Index **FWI**                        | Build Up Index **BUI**                          |
+---------------------------------------------------+-------------------------------------------------+
| - *WRF*: Temp, RH,                                | - *WRF*: Average Temp, RH,                      |
| - Wind Speed/Direction                            | - Wind Speed/Direction                          |
| - Hourly Rain Fall Totals                         | - 24 hour Rain Fall between (1100-1300) local   |
+---------------------------------------------------+-------------------------------------------------+


archive
********
``/bluesky/archive/fireweather/data/`` is the archive directory, it where copies of completed forecasts are stored as `.tgz`
    - ``fwf-hourly-YYYYMMDDHH.tgz`` for the hourly forecasts
    - ``fwf-daily-YYYYMMDDHH.tgz`` for the daily forecasts

wrf
********
the model currently uses 4-km WRF 00Z but is adaptable to other domains. 
    - ``/nfs/kitsault/archives/forecasts/WAN00CP-04/YYMMDD00/`` is the WRF directory where the model pulls in `.nc` files
    - If you change to a new model domain you'll first need to run `timezone.py` to generate a tzone_ds.zarr file (Note it takes awhile to generate ~3 hours but you only need to run it once!)


Required packages
------------------
Conda is used to manage python, its recommend to use conda to make the model work. 
    - ``fwf-env.yml`` lives in the parent folder of the fwf repo
    - to make a conda environment with all the required packages run the following code block

.. code-block:: python

    conda env create -f fwf-env.yml




