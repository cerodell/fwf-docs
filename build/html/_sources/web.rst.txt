Website 
========

User Guide
------------
.. image:: _static/images/fwf-web-user-guide.png
   :height: 1000 px
   :width: 2500 px
   :scale: 40%
   :align: center

To visualize the data on leaflet several steps are made to simplify and reduce the file as much as possible. 

Visualization Steps
---------------------
Steps to visualizing the data on a leaflet map.

#1 Pythons Matplotlib is used to `countourf` each forecast product.
    * `fwf/json/colormaps.json` contains the color schemes, levels, and max/min for each `contourf` plot.
#2 From a `countourf` `geojsoncontour` is used to convert the `countourf` plot to a `geojson` file. 
    * The utility that does this is in within `fwf/utils/geoutils.mycontourf_to_geojson` 
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
            geojson_filepath = f'/bluesky/fireweather/fwf/data/geojson/{folderdate}/{geojson_filepath}.geojson')


#3 Now that the data is in a `geojson` format it could be added to a leaf map using a variety of different leaflet extensions. However, the file size is a bit large at this stage ~8 Mb. To help reduce the file size `geojsons` are converted to `topojsons` using `geo2topo`
    * If you quantize the `geojosn` to a `topojson` you save a lot of file size
    * I found if you use quantization count (`q`) of 1e4 reduces the `geojson` file by nearly an order of magnitude and doest take away from the quality of the visualization on leaflet
    * comand line example: `geo2topo -q 1e4 path_to_infile/file_YYYYMMDDHH.geojson > path_to_outfile/file_YYYYMMDDHH.json`
    * reference: https://github.com/topojson/topojson-server
#4 Now that the data in a `topojsons` its added to leaflet using `Leaflet.VectorGrid.Slicer`
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

