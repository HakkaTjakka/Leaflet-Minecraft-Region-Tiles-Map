<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<link rel="stylesheet" href="./leaflet2.css">

</link>

<script src="./leaflet2.js"></script>
	
</script>
<style>
body {
height: 100%;
overflow:hidden;
padding:0px;
margin: 10px solid red;
background-color: #35363a;

}
html {
height: 96%;
}
.stroke, .no-stroke {
color: white;
font-size: 2.5em;
}
#map {
width:100%;
height:100%;
}
.stroke {
text-shadow:0 0 2px red
}
.infotile {
display: flex;
}
.infotile span {
font-weight: bold;
margin: auto;
font-size: 3.5em;
color: #ffff6677;
text-shadow: -2px -2px 1px #00000088;
line-height: 80%;
}
.infotile span_top_left {
font-weight: bold;
position: absolute;
font-size: 1.0em;
top: 0;
left: 0px;
line-height: 90%;
}
.infotile span_bottom_right {
font-weight: bold;   position: absolute;   bottom: 0;   right: 0;
}
img, a {
display: inline-block;
}
</style>
</head>
<body>
<div id="map" class="leaflet-container leaflet-touch leaflet-retina leaflet-fade-anim leaflet-grab leaflet-touch-drag leaflet-touch-zoom" tabindex="0" style="position: center; outline: none;">
</div>
<script type="text/javascript">
function bte121_region_coords_to_real_latlng(x,z) {
top_left =[ 3098732.937711787  , -5572933.950075601  ];
top_right=[ 3383947.8832087284 , -5484947.531326232  ];
bot_left =[ 3030557.8188913134 , -5363927.8285290385 ];
bot_right=[ 3321268.558218175  , -5273012.065926061  ];
return 'test ok!';
}
var map = L.map('map', {
crs: L.CRS.Simple,
attributionControl: true,
center: [10.210692, 6.053001],
zoom: 18,
minZoom: 1,
tileSize: 512,
zoomOffset: 0,
maxZoom: 18,
zoomControl: true,
});

L.TileLayer.Kitten = L.TileLayer.extend({
getTileUrl: function(coords) {
var div = 2**(18 - coords.z);
var mc_x; if ( coords.x >= 0 ) {mc_x = Math.floor( coords.x/10 ); } else { mc_x = -Math.floor( -((coords.x+1)/10-1) ); }
var mc_y; if ( coords.y >= 0 ) {mc_y = Math.floor( coords.y/10 ); } else { mc_y = -Math.floor( -((coords.y+1)/10-1) ); }

return "http://83.83.222.154/map/" + coords.z + "/" + mc_x + "/" + mc_y + "/r." + coords.x + "." + coords.y + ".png";
},
getAttribution: function() {
return "<a href='http://83.83.222.154/data/'>Pacman Graphics</a>"
}
});
L.tileLayer.kitten = function() {
return new L.TileLayer.Kitten();
}
map.addLayer( L.tileLayer.kitten() );
map.on("contextmenu", function (event) {
console.log("user right-clicked on map coordinates: " + event.latlng.toString());
L.marker(event.latlng).addTo(map);
var tile = map.latLngToLayerPoint(event.latlng);
var mo = map.getPixelOrigin();
var zoom=map.getZoom();

var div = 2**(18 - zoom+1);
var mc_x; mc_x=((tile.x+mo.x)*div);
var mc_y; mc_y=((tile.y+mo.y)*div);

tile.x = (mc_x);
tile.y = (mc_y);
var region_x = Math.trunc((tile.x)/512); if (tile.x < 0) region_x--;
var region_z = Math.trunc((tile.y)/512); if (tile.y < 0) region_z--;

alert(
'x=' + tile.x + '  z=' + tile.y + '\n' +
'r.' + region_x + '.' + region_z + '.mca\n' +
'lat=' + event.latlng.lat + ' lon=' + event.latlng.lng + '\n' +
'info()=' + bte121_region_coords_to_real_latlng(tile.x,tile.z) +
'zoom=' + zoom
);
});
L.GridLayer.DebugCoords = L.GridLayer.extend({
createTile: function (coords) {
var tile = document.createElement('div');
tile.className = "infotile";

var div = 2**(18 - coords.z);
var mc_x; mc_x=coords.x*div;
var mc_y; mc_y=coords.y*div;


var tileBounds = this._tileCoordsToBounds(coords);
tile.innerHTML = 
'<span_top_left>' +
mc_x*512 + '<br>' +
mc_y*512 + '<br>' +
'</span_top_left>'; 

if (coords.z==18) tile.innerHTML = tile.innerHTML +
'<span>' +
mc_x + '<br>' +
mc_y + '<br>' +
'</span>';
tile.innerHTML = tile.innerHTML +
'<span_bottom_right>' +
'zoom=' + coords.z +
'</span_bottom_right>'
;
tile.setAttribute("style", "font: sans-serif;" + 
"fontWeight: bold;" +
"color: #ffffff99;" + 
"font-size: 1.5em;" + 
"text-shadow: 2px 2px 2px #000000FF, -1px -1px 2px #000000ff;" +
"margin: auto;"
);

tile.style.textAlign = "center";

tile.style.outline = '1px solid #35363a';
return tile;
}
});
L.gridLayer.debugCoords = function(opts) {
return new L.GridLayer.DebugCoords(opts);
};
map.addLayer( L.gridLayer.debugCoords() );
</script>
</body>
</html>