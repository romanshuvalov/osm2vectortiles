# osm2vectortiles [![Build Status](https://travis-ci.org/osm2vectortiles/osm2vectortiles.svg)](https://travis-ci.org/osm2vectortiles/osm2vectortiles)

Create [Mapbox Streets](https://www.mapbox.com/developers/vector-tiles/mapbox-streets-v5) compatible vector tiles for custom
styling with [Mapbox Studio Classic](https://www.mapbox.com/mapbox-studio-classic/) and provide easy deployment methods.

## Get started

### Create a custom OSM base map

TODO: Create documentation for cartographer

### Deploy a custom OSM base map

TODO: Create documentation for sysadmin

## Components

The project consists of several components.

### Import

Import toolchain based on [Imposm 3](https://github.com/omniscale/imposm3) to
import [OpenStreetMap data](http://wiki.openstreetmap.org/wiki/Downloading_data)
into a [PostGIS](http://postgis.net/) database.
The mapping is optimized for fast generation of vectortiles.

![Import Step](https://github.com/manuelroth/osm2vectortiles-thesis/raw/master/source/figures/import_step.png)

### Export

Export toolchain consisting of [our custom tm2source project](https://github.com/geometalab/open-streets.tm2source)
based on the import mapping and tools to scale exporting of the vectortiles horizontally.

![Export Step](https://github.com/manuelroth/osm2vectortiles-thesis/raw/master/source/figures/export_step.png)

### Serve

A very easy tileserver where you can simply drop in your `tm2` style project and our produced `mbtiles` file
and it will serve a custom styled OSM map.

![Tileserver Step](https://github.com/manuelroth/osm2vectortiles-thesis/raw/master/source/figures/tileserver_step.png)

## Development

We use Docker extensively for development and deployment.
The easiest way to get started is using [Docker Compose](https://www.docker.com/docker-compose).

Start up your PostGIS container with the data container attached.

```
docker-compose up -d postgis
```

Download a PBF and put it into the local `import` directory.

```
wget https://s3.amazonaws.com/metro-extracts.mapzen.com/zurich_switzerland.osm.pbf
```

Now you need to import the PBF files into PostGIS.

```
docker-compose up import-osm
```

Now you need to import several external data sources.

Import water polygons from [OpenStreetMapData.com](http://openstreetmapdata.com/data/water-polygons).


```
docker-compose up import-water
```

Import Natural Earth data for lower zoom levels.

```
docker-compose up import-natural-earth
```

Import custom country, sea and state labels.

```
docker-compose up import-labels
```

Now import custom SQL functions.

```
docker-compose up import-sql
```

Update the scaleranks of OSM places.

```
docker-compose up update-scaleranks
```

Export the data as MBTiles file to the `export` directory.

```
docker-compose up export-local
```

Serve the tiles as raster tiles from `export` directory.

```
docker-compose up serve
```

## Docker Images

The workflow consists of several prebuilt Docker images.

| Image                             | Size                                                                                                               |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| osm2vectortiles/serve             | [![ImageLayers Size](https://img.shields.io/imagelayers/image-size/osm2vectortiles/serve/latest.svg)]()            |
| osm2vectortiles/export            | [![ImageLayers Size](https://img.shields.io/imagelayers/image-size/osm2vectortiles/export/latest.svg)]()           |
| osm2vectortiles/import-external   | [![ImageLayers Size](https://img.shields.io/imagelayers/image-size/osm2vectortiles/import-external/latest.svg)]()  |
| osm2vectortiles/import-sql        | [![ImageLayers Size](https://img.shields.io/imagelayers/image-size/osm2vectortiles/import-sql/latest.svg)]()       |
| osm2vectortiles/import-osm        | [![ImageLayers Size](https://img.shields.io/imagelayers/image-size/osm2vectortiles/import-osm/latest.svg)]()       |
| osm2vectortiles/update-scaleranks | [![ImageLayers Size](https://img.shields.io/imagelayers/image-size/osm2vectortiles/update-scaleranks/latest.svg)]() |
| osm2vectortiles/postgis           | [![ImageLayers Size](https://img.shields.io/imagelayers/image-size/osm2vectortiles/postgis/latest.svg)]()          |
| osm2vectortiles/pgbouncer         | [![ImageLayers Size](https://img.shields.io/imagelayers/image-size/osm2vectortiles/pgbouncer/latest.svg)]()        |