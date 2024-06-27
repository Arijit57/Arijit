<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
      
    <!--leaflet css-->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin="" />
    

<style>
    html,body,#map{
        width: 100%;
        height: 100vh;
        margin: 0;
        padding: 0;
    }

</style>
</head>


<body>    
    <div id= "map"></div>
</body>
</html>


<!--leaflet js-->
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

<!--leaflet draw plugin-->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet.draw/0.4.2/leaflet.draw.css"/>
 <script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet.draw/0.4.2/leaflet.draw.js"></script>


<script>


    //map initialization
    var map = L.map('map').setView([20.5937, 78.9629], 5);


//osm layer
var osm = L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
})
 osm.addTo(map);

//water color
var Watercolor = L.tileLayer('https://tiles.stadiamaps.com/tiles/stamen_watercolor/{z}/{x}/{y}.{ext}', {
	minZoom: 1,
	maxZoom: 16,
	attribution: '&copy; <a href="https://www.stadiamaps.com/" target="_blank">Stadia Maps</a> &copy; <a href="https://www.stamen.com/" target="_blank">Stamen Design</a> &copy; <a href="https://openmaptiles.org/" target="_blank">OpenMapTiles</a> &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
	ext: 'jpg'
});
//watercolor.addTo(map);

///Dark color
var dark = L.tileLayer('https://tiles.stadiamaps.com/tiles/alidade_smooth_dark/{z}/{x}/{y}{r}.{ext}', {
	minZoom: 0,
	maxZoom: 20,
	attribution: '&copy; <a href="https://www.stadiamaps.com/" target="_blank">Stadia Maps</a> &copy; <a href="https://openmaptiles.org/" target="_blank">OpenMapTiles</a> &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
	ext: 'png'
});
//darkcolor.addTo(map);

//googlestreets
googleStreets = L.tileLayer('http://{s}.google.com/vt/lyrs=m&x={x}&y={y}&z={z}',{
    maxZoom: 20,
    subdomains:['mt0','mt1','mt2','mt3']
});
////googleStreets.addTo(map);

//satelite 
googleSat = L.tileLayer('http://{s}.google.com/vt/lyrs=s&x={x}&y={y}&z={z}',{
    maxZoom: 20,
    subdomains:['mt0','mt1','mt2','mt3']
});
///googleSat.addTo(map)

//marker
var myIcon = L.icon({
        iconUrl: 'red_marker.png',
        iconSize: [40, 40],
    });
var singleMarker = L.marker([20.5937, 78.9629], { icon: myIcon , draggable: true});
var popup = singleMarker.bindPopup('This is the India' + singleMarker .getLatLng()).openPopup()
popup.addTo(map);

var secondMarker = L.marker([25.5937, 78.9629], { icon: myIcon , draggable: true});

console.log(singleMarker.toGeoJSON())

/*==============================================
                    LAYER CONTROL
    ================================================*/
    var baseMaps = {
        "OSM": osm,
        "Water color map": Watercolor,
        'Dark': dark,
        'Google Street': googleStreets,
        "Google Satellite": googleSat,
    };
    var overlayMaps = {
        "First Marker": singleMarker,
        "second Marker": secondMarker
    };
    // map.removeLayer(singleMarker)
    L.control.layers(baseMaps, overlayMaps, {collapsed:false }).addTo(map);
    
    /*==============================================
                       leaflet draw   
    ================================================*/
       
  var drawnFeatures = new L.FeatureGroup();
      map.addLayer(drawnFeatures);

    var drawControl = new L.Control.Draw({
         edit:{
            featureGroup: drawnFeatures,
            remove: false
         },
    });
     map.addControl(drawControl);

     
     map.on("draw:created", function(e){
        var type = e.layerType;   
        var layer = e.layer;
     drawnFeatures.addLayer(layer);
     });

     map.on("draw:edited", function(e){
        var type = e.layers;   
        var layer = e.layer;
     drawnFeatures.addLayer(layer);

    
     layers.eachLayer(function(layer){
         console.log(layer)
     })
    })
    

     
    
</script>
