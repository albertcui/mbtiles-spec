# MBTiles 1.0

## Abstract

MBTiles is a specification for storing tiled map data in  [SQLite](http://sqlite.org/) databases for immediate usage and for transfer. MBTiles files, known as **tilesets**, must implement the specification below to ensure compatibility with devices.

## Database Specifications

Tilesets are expected to be valid SQLite databases of [version 3.0.0](http://sqlite.org/formatchng.html) or higher. Only core SQLite features are permitted; tilesets **cannot require extensions**.

## Database

Note: the schemas outlined are meant to be followed as interfaces. SQLite views that produce compatible results are equally valid. For convenience, this specification refers to tables and virtual tables (views) as tables.

### Metadata

#### Schema

The database is required to contain a table or view named `metadata`.

This table must yield exactly two columns named `name` and `value`. A typical create statement for the `metadata` table:

    CREATE TABLE metadata (name text, value text);

#### Content

The metadata table is used as a key/value store for settings. Four keys are required:

* `name`: The plain-english name of the tileset.
* `type`: `overlay` or `baselayer`
* `version`: The version of the tileset, as a plain number.
* `description`: A description of the layer as plain text.

### Tiles

#### Schema

The database is required to contain a table named `tiles`.

The table must yield four columns named `zoom_level`, `tile_column`, `tile_row`, and `tile_data`. A typical create statement for the `tiles` table:

    CREATE TABLE tiles (zoom_level integer, tile_column integer, tile_row integer, tile_data blob);

#### Content

The tiles table contains tiles and the values used to locate them. The `zoom_level`, `tile_column`, and `tile_row` columns follow the [Tile Map Service Specification](http://wiki.osgeo.org/wiki/Tile_Map_Service_Specification) in their construction, but in a restricted form:

* **The [global-mercator](http://wiki.osgeo.org/wiki/Tile_Map_Service_Specification#global-mercator) (aka Spherical Mercator) profile is assumed**

The `blob` column contains raw image data in binary.

Formats supported:

* `png`
* `jpg`
