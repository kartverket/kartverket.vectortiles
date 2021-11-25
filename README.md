# Introduction
Vectortiles from Kartverket are designed to offer the speed and performance of todays cache (tile) services but with flexible styling, provided through a data service and client side rendering.

The first pilot service (landtopo) delivers data covering the whole of Norway and includes our most detailed 1:3000 data at higher zoom levels and generalised data at lower zoom levels for better performance.

The vectortiles service can be used with most of the mainstream web mapping libraries, including mapbox, maplibre, openlayers, leaflet as well GIS clients such as QGIS. The functionality through these clients and libraries vary, but all have in common that they provide easy access to both predefined and user defined styles as well as the fast performance expected in todays web applications. This means that the user is in full control of which data to show, which styles to use and is able to customise the map to a specific need.

In addition to the styling flexibility provided, increased user interaction is made possible through access to feature level information. This data can either be provided to the user as information, or used as input to further processes.



# What are vectortiles good for?
Flexible styling
Reactive web applications
Specialised use cases
online clients requiring fast background mapping
Mobile applications
what shouldn't you use vectortiles for?
Downloading source data (the data in a vectortile is encoded and has been customised specifically for visualisation)
GIS analysis (the data model is often too simple and the geometries not accurate enough)




# Tilejson manifest doc and info


The tilejson specification was designed as an open way to provide map metadata, and defines the structure for a json manifest document.



The url to the tilejson manifest document for the landtopo sevice is:

https://cache.kartverket.no/test/vectortiles/tilejson/landtopo.json

The url can be used to specify service information in various clients. More info information can be found here: https://github.com/mapbox/tilejson-spec



### The document includes information about:

Tile endpoints (url to the service with parameters)

Layers included in the service

Fields for each layer

Zoom levels

Bounding box of the data coverage



# Examples:
Mapbox
Leaflet
OL
Qgis?


# Style:

The style of the service is defined within a .json file, and contains:

Urls to one or more data sources (can be vectortiles, raw vector data, wms service osv), https://docs.mapbox.com/mapbox-gl-js/style-spec/sources/

Url to a sprite (a single png and accompanying json file containing all symbol images used by the service) https://docs.mapbox.com/mapbox-gl-js/style-spec/sprite/

Url to available fonts https://docs.mapbox.com/mapbox-gl-js/style-spec/glyphs/

One or more style blocks



### Links to default styles:

https://cache.kartverket.no/test/styles/landtopo.json



We have provided 2 default styles so that it is possible to use and look at the service with no extra work needed. These styles have been designed to mirror the cartography of our topo4 and norges_grunnkart raster tile services, so are quite complex and long.

However, it is relatively easy for a user to create a custom style using either a text editor or an online style editor such as [maputnik](https://maputnik.github.io/editor) or [fresco](https://fresco.go-spatial.org/). in addition to changing the style of the data, its also possible to remove layers or change the drawing order of the layers. By doing this, the user can create a style that is customised to a specific need.


Thumbnails of styles
Code examples
Link to maputnik?

# Fonts:
One standard font (otherwise known as 'glyphs') are also available from Kartverket:

**opensansregular**

and can be used through this url:

https://cache.kartverket.no/test/fonts/{fontstack}/{range}.pbf"

(The application will replace the fontstack and range variables automatically as long as one of the above font names are defined as a text-font within a layer. For example: "text-font": ["OpenSansRegular"])

Several other fonts are available from the [OpenMapTiles](https://github.com/openmaptiles/fonts). Just follow the instructions to include them in your own styles.



# Vector Tile Contents
We've tried to simplify the data model as much as possible to make it easier to build user defined styles. In effect, the service contains 21 layers, but each layer contains multiple feature types that can be filtered and styled as needed.

Most of the layers contain a mix of data sources from our Kartdata and FKB data products. Because the models for these 2 datasets are very different, not all the attributes will contain values. Some features will have full attribute values for the first 14 zoom levels, but limited values from level 15 to 19, and vice versa.

Every layer has an 'objtype' attribute which contains information about what type of feature the object is, for example the objtype attribute in the adm_boundaries layer includes the values; 'national boundary', 'county', 'municipality' and 'territorial boundary'.



## Layer overview
**adm_grenser**
  - objtype

**tekst**
  - objtype
  - textstring
  - subtype
  
**vegnavn**
  - objtype
  - vegstatus
  - vegnummer
  - gatenavn
  - vegkategori
  - medium
  
**bygninger**
  - objtype
  - subtype
  
**bygningspunkter**
  - objtype
  - subtype
  - betjeningsgrad
  - tilgjengelighet
  
**hoydelag**
  - objtype
  - makshoyde
  
**hoydekurver**
  - objtype
  - medium
  - hoyde
  
**pois**
  - objtype
  - medium
  - hoyde
  
**vannlinje**
  - objtype
  - vannbredde
  
**arealdekke**
  - objtype
  
**naturinfo**
  - objtype
  
**vegflater**
  - objtype
  - medium
  
**veglinjer**
  - objtype
  - vegkategori
  - motorvegtype
  - medium
  
**vegavgrensning**
  - objtype
  - medium
  
**stier**
  - objtype
  - rutemerking
  - belysning
  
**jernbane**
  - objtype
  - subtype
  - medium
  
**ferger**
  - objtype
  
**samferdsel punkt**
  - objtype
  - subtype
  
**anleggslinjer**
  - objtype
  
**anleggspunkter**
  - objtype
  
**anleggsomrader**
  - objtype



## Attributes
All layers contain the attribute objtype (object type), which describes the specific object type for each individual feature. Certain layers, like adm_grenser and arealdekke, only contain this one attribute.

**Objtype:** Several layers also contain one or more of the following attributes

**Subtype:** a subdivision of objtype, allows for more detailed styling
**Textstring:** Specific for text layers, contains the text displayed on the map
**Medium:** Describes feature qualities such as location of an with respect to the earth surface (tunnel vs bridge, inside a building etc. for roads or paths, on the ground or on a glacier for hoydekurver or trigonometriske punkt)

In addition there other layer-specific attributes, such as betjeningsgrad, tilgjhengelighet, hoyde, makshoyde, vannbredde vegkategori, motorvegtype,, rutemerking, belysning or retningsverdi.

Layers such as veglinjer or bygningspunkt might contain one or more of these.



## Detailed Data model



> ## adm_grenser
This layer contains country, municipality and district data. The geometry type is polygon so its possible to either shown areas or outlines.

### Attributes:
**objtype**
  - *Values:*
     - **Riksgrense:** Delimitation of the country of Norway over against other countries
     - **Territorialgrense:** Maritime territorial borders
     - **AvtaltAvrensningslinje:** Maritime delimitation of the country of Norway
     - **Kommunegrense:** Administrative districting of the municipalities
     - **Grunnlinje:** Baseline
     - **Fylkesgrense:** Administrative districting of the counties
##  


> ## tekst
Contains area names, heights and addresses. This is a point type layer, however the features are displayed as labels rather than icons

### Attributes:

**objtype**
  - *Values:*
     - **adresse:** building and house addresses , numbers and letters
     - **StedsnavnTekst:** Names of all administrative units apart from municipalities.
     - **Kommune:** names of municipalities
     - **Vannhoyde:** height of a body of water
     - **høydetall:** height of terrain/peaks
         - 1000,2000,3000,4000,5000,6000,7000: All other stedsnavn (place names), such as cities, rural areas, national parks, mountains, lakes or rivers. The numbers 1000 - 7000 refer to different "categories" of named areas/objects. See code lists for more information
         - Examples: 1000 = Landforms, 3000 = fresh water bodies, 5000 = buildings and populated places
      - **Textstring:** Textstrings, labels and numbers displayed on the map
      - **Subtype:** Subdivision of Stedsnavn, more specifically objtypes 1000 - 7000. None of the other objtypes have subtypes. See code lists for more information.
          - Examples: 1102 = mountainous area, 1104 = forested area, 5101 = city, 5103 = town
##  

> ## vegnavn
Separate text layer containing road names and numbers. This too is a point layer but had to be separated from the tekst layer due to the inclusion of several attributes which are only required for vegnavn

### Attributes:

**objtype**
  - *Values:'
     - **VegSenterlinje:** line in the centre between two road shoulders
     - **Kjørebane:** line in the centre of a carriageway
**vegstatus:** refers to the status of the road. Existing, temporary etc.
  -Values:
   - P: Approved planned road
   - V: Existing road
**vegnummer:** road number
**gatenavn:** street name. For example: Main Street
**vegkategori:** Indicates road type. Examples: private road, European route etc.
  - Values:
    - **E:** Europaveg/ European Route
    - **F:** Fylkesveg/ County Road
    - **K:** Kommunal veg/ District Road
    - **P:** Privat veg/Private Road
    - **R:** Riksveg/ National Road
    - **S:** Skogsbilveg/ Forest Road

**medium:** the location of the object relative to the earth's surface
  - *Values:*
     - **U:** Under the terrain, tunnel
     - **T:** On the terrain
     - **L:** In the air, bridge
##  

> ## bygninger
The bygninger layer is a polygon layers which includes all buildings as areas. These are only shown on lower zoom levels (12 and under). On higher zoom levels buildings are represented as points

### Attributes:

**objtype**
  - *Values:*
     - **bygning**

bygningspunkter holds quite an extensive number of objtype values, the most common one being 'Bygning'. The others are rarely used. See codelists for more information.

**subtype:** contains 114 different subtypes which indicates byilding type, ordered by numbers, see code lists for more information. The bygnigner subtypes correspond to the bygningspunkter subtypes

examples: 511 = hotel, 671 = church, 970 = hospital with ER
##  

> ## bygningspunkter
Buildings represented with points and icons on higher zoom levels (11 and over). on lower zoom levels the buildings are represented as polygons.

### Attributes:

**objtype**
  - *Values:*
     - **bygning**

bygningspunkter holds quite an extensive number of objtype values, the most common one being 'Bygning'. The others are rarely used. See codelists for more information.

**subtype:** contains 114 different subtypes which indicates byilding type, ordered by numbers, see code lists for more information. The bygnigner subtypes correspond to the bygningspunkter subtypes

examples: 511 = hotel, 671 = church, 970 = hospital with ER

**betjeningsgrad:** description of which service functions are available. Only applicable to public cabins (subtypes 956 and 56)
  - *Values:*
     - **S:** Selvbetjent/ Self-service cabins
     - **U:** Ubetjent/ No-service cabins
     - **B:** Betjent/ Staffed lodges
     - **R** Rastebu/Small huts with wood stoves
     - **D:** Dagstur/ Cabins suitable for day trips. Open seasonally
     - **G:** Gapahuk/ Lean-to, or very primitive basic cabins

**tilgjengelighet:** Describes whether a building is locked or open. Only applicable to public cabins
  - *Values:*
     - **Låst:** Locked
     - **Ulåst:** Unlocked
##  

> ## hoydelag
A polygon layer which represents the overall landscape elevation, visualized with a colour gradient. Represented in meters above sea level. Generated from a DTM

### Attributes:

**objtype**
  - *Values:*
     - **Høydelag**

**makshoyde:** Max height in meters, given in intervals of 500m (N250,N500) or 600m (N2000), between 500-3000m
##  

> ## hoydekurver
Landscape elevation represented in meters above sea level in a line layer. Used for drawing contour lines. Generated from a DTM

### Attributes:

**objtype**
  - *Values:*
     - **Høydekurve**
 
**medium:** Describes ground surface type
  - *Values:*
     - **I:** isbre/glacier
     - **T:** Terreng/terrain

**hoyde:** height in meters of each contour line. The vertical distance between them varies with zoom levels, from 10m intervals (zoom level 14 and below) to 600m (zoom level 8). From -20m - 2400m.
##  

> ## pois
Points of interest, these are usually visualized with an icon or a circle. Contains landscape-related points of interest such as trigonometric points, mountain peaks etc.

### Attributes:

**objtype:** The majority of these objtypes are only applicable to specific zoom level intervals, see code lists for more information. TrigonometriskPunkt and Terrengpunkt are available on all zoom levels
  - *Values:*
     - **TrigonometriskPunkt:** Trig point. Permanently marked point, marked with a bolt or other mark in which the plane coordinates and/or height are determined in a Trigonometrical network in a geodetic system
     - **Terrengpunkt:** Point in the terrain with a measured height value used to indicate the height on pronounced surfaces in the terrain
     - **Toppunkt:** highest point in the terrain with a measured height value used to indicate the height on pronounced surfaces in the terrain
     - **Forsenkningspunkt:** Point in the terrain with a fixed height value which describes a depression in the terrain
     - **Golfbane:** Golf course
     - **Steinbrudd:** Quarry
     - **Industriomrade:** Industrial area/site
     - **Lufthavn:** Airport
     - **Tettbebyggelse:** Populated area
     - **InnmåltTre:** Tree
     - **Stein:** Boulder

**medium:** Describes ground surface type for topographic and terrain points. Only available for TrigonometriskPunkt and Terrengpunkt
  - *Values:*
     - **I:** isbre/glacier
     - **T:** Terreng/terrain

**hoyde:** Specific height of each point, given in meters. Only available for TrigonometriskPunkt and Terrengpunkt. A separate layer holds all water area polygons
##  

> ## vannlinje

Vannlinje contains all water lines, mostly rivers. Water polygons can be found in the Arealdekke layer (land cover). At Zoom level 14 and down rivers are shown as polygons (Arealdekke)

### Attributes: 

**objtype**
  - *Values:'
     - **ElvBekk:** Rivers and streams
     - **FlomløpKant:** Demarkation line for large river courses where water flow is occational due to regulation or other causes. only available on zoom level 13

Vannlinje holds quite an extensive number of objtype values which are only available from zoom level 14 and under. see code lists for more information
   
**vannbredde:** Rough classification of watercourses according to average width over longer expanses. Only applicable for features with objtype 'ElvBekk' and for zoom levels 8-13
  - *Values:*
     - **2:** width: < 3m
     - **3:** width: 3 - 15m
     - **4:** width 15 - 40m
     - **5:** width 40 - 100m 
     - **6:** width > 100m 
##  

> ## arealdekke
Polygon layer which contains information on land cover, including both natural and industrial cover, as well as water bodies such as oceans and lakes 

### Attributes:

**objtype:** There are 30 different values for objtype. Information on all of these can be found in code lists. Below follows an unofficial subdivision of all 30 values by  theme/topic:
  - *Values:*
     - **Undeveloped land cover:** Skog, SnøIsbre, Myr, Steinbrudd, Steinslipp, Åpen fastmark, ÅpentOmrade
     - **Sports facilities:** Alpinbakke, Golfbane, SportIdrettPlass
     - **Residential areas:** Hyttefelt, ByggmessigBebyggelse, Bebygd, Tettbebyggelse
     - **Other built-up areas:** Gravplass, Park, Lufthavn, Rullebane, Industriområde
     - **Agricultural land:** DyrketMark, Fulldyrka jord, Overflatedyrka Jord, Innmarksbeite
     - **Waters:** InnsjøkanRegulert, FerskvannTørrfall, Havflate, ElvBekk, Innsjø, Innsjøregulert , KanalGrøft

##  

> ## naturinfo
Line layer outlining all protected sites and national parks, as well as "land cover line data"

### Attributes: 

**objtype**
  - *Values:*
     - **Naturverngrense:** Protected site and Natinal park oundaries 
     - **Hekk:** Hedge 
     - **Allé:** Alley/nature strip 

##  

> ## vegflater
Road infrastructure presented as a polygon layer. This layer is only available on zoom levels 14 and higher. On higher zoom levels the road infrastructure is represented using the road infrastructure line layer

### Attributes: 

**objtype**
  - *Values:*
     - **Parkeringsområde:** Parking area
     - **GangSykkelveg:**  pedestrian and cycle path
     - **Veg:** Roadway
     - **Traktorveg:** Fire road/forest road
     - **Trafikkøy:** Traffic island 

**medium:** the location of the object relative to the earth's surface.
  - *Values:*
     - **B:** Inside bulding or construction
     - **U:** Under the terrain, tunnel
     - **I:** On glacier
     - **D:** (partially) under water 
     - **T:** On the terrain, on ground level
     - **L:**  In the air, bridge

##  


## veglinjer
Road infrastructure presented as a line layer. This layer is only available on zoom levels 13 and lower. On higher zoom levels the road infrastructure is represented using the road infrastructure polygon layer

### Attributes: 

**objtype**
  - *Values:*
     - **Vegsenterlinje:** line in the centre between two road shoulders
     - **Traktorveg:** Fire road/forest road
     - **Fortauskant:** Pavement 

**vegkategori:** Indicates road type. Examples: private road, European route etc. Only applicable for features with objtype 'Vegsenterlinje'
  - *Values:*
     - **E:** Europaveg/ European Route
     - **F:** Fylkesveg/ County Road
     - **K:** Kommunal veg/ District Road 
     - **P:** Privat veg/Private Road 
     - **R:** Riksveg/ National Road
     - **S:** Skogsbilveg/ Forest Road 

**motorvegtype:** Sub-classification of roadway type. Only applicable for features with objtype 'Vegsenterlinje'
  - *Values:*
     - **Motortrafikkveg:** Two-lane expressway
     - **Ikke motorveg:** Normal road
     - **Motorveg:** highway/freeway

**medium:** the location of the object relative to the earth's surface.
  - *Values:*
     - **B:** Inside bulding or construction
     - **U:** Under the terrain, tunnel
     - **T:** On the terrain, on ground level
     - **L:**  In the air, bridge

##  

> ## vegavgrensning
Line layer representing outlines of roads and paths, including roadways, pavements and cycle paths. This layer is only available on zoom levels 14 and higher

**objtype:** There are 16 different objtypes within the vegavgresning layer. the 7 most abundant of these are listed below 
  - *Values:*
     - **Vegdekkekant:** general road surface outline
     - **Kjørebanekant:** carriageway outline
     - **Fortauskant:** Pavement outline
     - **Gangvegkant:** pedestrian path outline
     - **GangSykkelvegkant:** pedestrian and cycle path outline
     - **Trafikkøykant:** Traffic island outline
     - **AnnetVegarealAvgrensning:** outher road or path outline

**medium:** the location of the object relative to the earth's surface.
  - *Values:*
     - **B:** Inside bulding or construction
     - **U:** Under the terrain, tunnel
     - **I:** On glacier
     - **D:** (partially) under water 
     - **T:** On the terrain, on ground level
     - **L:**  In the air, bridge
     - **O:** On water 
     - **V:** Always in/under water
     - **S:** On the ocean bottom
     - **X:** Unknown

##  

> ## stier
A line layer containing all roads, paths and trails in the terrain or on uncultivated land 

**objtype**
  - *Values:*
     - **Sti:** distinct path in the terrain which has become pronounced through many years' use or prepared for general travel on foot.
     - **Barmarksløype:** traditional driving tracks/tractor roads on uncultivated land where motor traffic for the general public is permitted on snowless ground in accordance with the regulations laid down by the County governor

**rutemerking:** States whether the trail is marked or not . Only applicable for features with objtype 'Sti'
  - *Values:*
     - **JA:** trail is marked
     - **NEI:** Trail is not marked

**belysning:** States whether trail route is lit up or not. Only applicable for features with objtype 'Sti' and on zoom levels 14 and higher 
  - *Values:*
     - **1:** Trail route is lit up
     - **2:** Trail route is partially lit up
     - **3:** Trail route is not lit up

##  

> ## jernbane
Line layer which represents a single or several parallel tracks as part of a railway line. 

**objtype**
  - *Values:*
     - **Bane:** Railway. Zoom level 7-13 
     - **Spormidt:** line in the centre between two rail strings. zoom levels 14 and higher

**subtype:** Describes railway type. Only applicable for features with objtype 'bane' on zoom levels 10 and higher
  - *Values:*
     - **N:** Nedlagt: Disused railway
     - **P:** Planlagt. Planned railway
     - **J:** Jernbane. Railway
     - **T:** Tunellbane. Underground railway
     - **M:** Museumsbane. Heritage railway

**medium:** The location of the object relative to the earth's surface. 
  - *Values:*
     - **B:** Inside bulding or construction
     - **U:** Under the terrain, tunnel
     - **T:** On the terrain, on ground level
     - **L:** In the air, bridge

##  

> ## ferger
Line layer representing all routes serviced by ferries

**objtype**
  - *Values:*
**bilferjestrekning:** ferry route serviced by car ferries as part of the road network 

passasjerferjestrekning: ferry route for transportation of people and goods. Wherever bilferjestrekning and passasjerferjestrekning coincide the objtype bilferjestrekning will be used 

##  

> ## samferdsel punkt
All point data related to infrastructure, including stations, airports and parking spaces. Several of these features are also available as polygon features through anleggsomrader or arealdekke on higher zoom levels

**objtype**
  - *Values:*
     - **Lufthavn:** Airport/ landing platforms and runways for planes and helicopters 
     - **Stasjon:** Railway station 
     - **Vegsperring:** Roadblocks. Only available on zoom levels 13 and higher 
     - **Parkeringsomrade:** Parking areas and structures. Only available on zoom levels 13 and higher 

**subtype:** Only applicable for features with objtype 'Lufthavn'. Indicate the aircraft type suitable to land at the specific site
  - *Values:*
     - **L:** Landing strip/runway for planes 
     - **H:** Landing platform for helicopter

##  

> ## anleggslinjer
Installation and construction line type data. Includes construction lines, outlines and boundaries, as well as electrical wires and lines conducting electrical power over large distances

**objtype:** There is a quite extensive list of different values for anleggslinjer objtype. All values can be found in code lists. Some examples include:
  - *Values:*
     - **Skitrekk:** Ski lift
     - **Taubane:** Cable car
     - **LuftledningLH:** Overhead power line
     - **Bygningslinje:** Building outline
     - **Vindkraftverk:** wind power station outline

##  


> ## anleggspunkter
Installation and construction point type data. Includes construction points, suck as towers, wind turbines etc.

**objtype:** There is a quite extensive list of different values for anleggslinjer objtype. All values can be found in code lists. Some examples include: 
  - *Values:*
     - **tårn:** tower
     - **Bensinpumpe:** Gas/petrol pump
     - **Capingplass:** camp site
     - **Vindkraftverk:** wind turbine
     - **Flaggstang:** Flag pole

##  

> ## anleggsomrader
Installation and construction polygon type data. Only available on zoom levels 13 and higher

**objtype:** There is a quite extensive list of different values for anleggsomrader objtype, several of which correspond to objtypes in anleggslinjer and anleggspunkter. All values can be found in code lists. Some examples include :
  - *Values:*
     - **Tank:** Tank
     - **Tårn:** tower
     - **Svømmebasseng:** Swimming pool
     - **Bru:** bridge 
     - **BautaStatue:** Statues and monuments 


Table view of layers - ala ordnance survey
Fields explanation


# Source data

The data in the service comes from 3 products:

Kartdata (general topographic data N5000 - N50)
FKB (1:5000 topographic data)
Matrikkel (cadastral data)

These data sources can be further explored within the [Norwegian national geoportal](https://www.geonorge.no/en) 


In depth description of data layers
In depth description per zoom level

# FAQ's

Are there any code examples available?
> we have some code examples at the top of this page, and within the seperate 'examples' folder. 

Will these services work in desktop GIS clients?
> Some, but not all. A qgis example can be found at the top of the page, but we do not have a comprehensive list over which clients currently support vectortiles services.

Is technical support available?
> Technical questions can be asked with the [issues](https://github.com/kartverket/kartverket.vectortiles/issues) section of this github project, or directly to the email address: 

What projections are the services available in?
> Currently the services are available in only google mercator (EPSG:3857) and UTM33N (EPSG:25833)

Can I use the vectortiles service as a download service?
> No. These services have been designed and customised to be view services only, we kindly ask that they are used as such.

Which other vectortiles services are Kartverket planning?
> We'll soon be providing services for marine topographic data, ports and hiking and bridleway trails. But with the number fo open datasets available from Kartverket, we're sure there'll be others to follow, stay tuned!


 
