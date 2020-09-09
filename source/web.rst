Website 
========

User Guide
------------
.. image:: _static/images/fwf-web-user-guide.png
   :height: 200 px
   :width: 400 px
   :scale: 40%
   :align: center

To visualize the data on leaflet several steps are made to simplify and reduce the file as much as possible. 

Visualization Steps
---------------------
#. zarr file data is first masked to remove all lakes, oceans, and snow cover.
    * see ``/bluesky/fireweather/fwf/utils/geoutils.py``
#. after mask is applied fire weather indices/codes are made into contourf
    * customize by changing ``/bluesky/fireweather/fwf/json/colormaps.json``
#. from contourf they are converted to geojson files using geojsoncontour
    * geojsoncontour reference: https://pypi.org/project/geojsoncontour/
    * note all indices and moisture codes are rounded to the third decimal 
    * steps 1-3 are done in python all in ``/bluesky/fireweather/fwf/firewx_website/python/geojson_maker.py``

#. next geojsons are converted to topojsons using ``geo2topo``
    * ``geo2topo -q 1e4 path_to_infile/file_YYYYMMDDHH.geojson > path_to_outfile/file_YYYYMMDDHH.json``
    * reference: https://github.com/topojson/topojson-server
    * a ``q`` (ie quantization count) of `1e4` reduces the geojson file by about half and doest take away for the quality of the visualization on leaflet
    * there are many ways to simplify topojson and reduce the file size further
    * reference: https://github.com/topojson/topojson-simplify 

#. topojsons are stored as json files: ``/bluesky/archive/fireweather/forecast/YYYYMMDDHH`` 
    * stored as .json extension so server can gzip and send file to client