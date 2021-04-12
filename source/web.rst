Website 
========

User Guide
------------
.. image:: _static/images/fwf-popups.png
   :width: 600 
   :align: center

Meteogram forecasts of:
    - (A) FWI values
    - (B) Associated meteorology
    - (C) Station-located two-week comparison of observed vs. forecasted FWI values
    - (D) Associated meteorology. Solid lines represent forecasts and dashed lines represent observed values. 
    

- A and B can be activated by a double-click on a Desktop computer and single-click on mobile. 

- C and D can be activated by selecting the “weather station” layer in the main drop-down menu and clicking on a weather station of interest.


Visualization Steps
---------------------
Below are the steps to visualizing the data on a leaflet map. The data starts in python as a zarr file and is converted to a workable leafletjs formate.

#. Python's matplotlib is used to create ``countourf`` of each forecast product.
    * reference: https://matplotlib.org/3.1.1/api/_as_gen/matplotlib.pyplot.contourf.html
    * ``fwf/json/colormaps.json`` contains the color schemes, the defined contour levels, and the max/min for each ``contourf`` plot.
#. From here ``geojsoncontour`` is used to convert the ``countourf`` plot to a ``geojson`` file. 
    * The utility that does this is in within ``fwf/utils/geoutils.mycontourf_to_geojson`` 
    * reference: https://github.com/bartromgens/geojsoncontour
    * Here is a snippet of the code

    .. code-block:: python

        Cnorm = matplotlib.colors.Normalize(vmin= vmin, vmax =vmax+1)
        contourf = plt.contourf(lngs, lats, fillarray, levels = levels, \
                                linestyles = 'None', norm = Cnorm, colors = colors, extend = 'both')
        plt.close()

        geojsoncontour.contourf_to_geojson(
            contourf=contourf,
            min_angle_deg=None,
            ndigits=2,
            stroke_width=None,
            fill_opacity=None,
            geojson_properties=None,
            unit='', 
            geojson_filepath = f'/fwf/data/geojson/{folderdate}/{geojson_filepath}.geojson')

#. Now that the data is in a ``geojson`` format it could be added to a leaflet map using a variety of different leaflet extensions. However, the file size is a bit large, at this stage ~ 8 Mb. To help reduce the file size ``geojsons`` are converted to ``topojsons`` using ``geo2topo``
    * If you quantize the ``geojosn`` to a ``topojson`` you save a lot on file size
    * I found if you use a quantization count (``q``) of 1e4, will reduce the ``geojson`` file by nearly an order of magnitude and doesn't take away from the quality of the visualization on leaflet
    * reference: https://github.com/topojson/topojson-server
    * Execute geo2topo quantization on comand line 

    .. code-block:: bash

        geo2topo -q 1e4 path_to_infile/file_YYYYMMDDHH.geojson > path_to_outfile/file_YYYYMMDDHH.json

#. Now that the data is in a ``topojsons`` its added to leaflet using ``Leaflet.VectorGrid.Slicer``
    * API: https://leaflet.github.io/Leaflet.VectorGrid/vectorgrid-api-docs.html
    * GitHub: https://github.com/Leaflet/Leaflet.VectorGrid
    * An example js code block snippet

    .. code-block:: javascript

        fetch(url, {cache: "default"}).then(function(response){
            return response.json();
        }).then(function(json){
            newLayer.addLayer(L.vectorGrid.slicer( json, {
                minZoom: 2,
                maxZoom: 18,
                rendererFactory: L.canvas.tile,
                vectorTileLayerStyles:{
                    'FFMC': geo_json_styler18
                        }
                    }
                ).setZIndex(500)
            )
        })};

