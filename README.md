# Leaflet-Minecraft-Region-Tiles-Map

http://83.83.222.154/amsterdam/

http://83.83.222.154/nl/

Changed the index.html

Showing how to put (see def.conf in apache dir for directory settings) two different tile maps in different dirs on different harddrive into one single map, on different positions of course). The Amsterdam poster has different tile numbers than the nl poster.
the http://83.83.222.154/nl/ positions on the Minecraft Holland map. When you zoom out to level 1, or access http://83.83.222.154/nl/amsterdam.html you can see it.


Leaflet HTML/Javascript Minecraft Region Mapper BTE121 For Browser Plus C/C++ Zoom Level 2x2x512 Tilemaker

For use with: https://github.com/HakkaTjakka/REGION_FINDER

***
![clipboard_small](https://github.com/HakkaTjakka/Leaflet-Minecraft-Region-Tiles-Map/blob/main/NL/Untitled.png)
***
[![Demo CountPages alpha](https://github.com/HakkaTjakka/Leaflet-Minecraft-Region-Tiles-Map/blob/main/NL/Untitled.jpg)](https://www.youtube.com/embed/m01M_vSGTMw)
(Click to play vid)
***
  Use: region_finder.exe tiles <tiledir>
  Where <tiledir> is a directory with r.*.*.png tiles (512pxx512px) of Minecraft region files.
  Tiles can be made with the MinecraftWorldEditor program (pacman.exe from the pacman_cuberite dir)
    Using dir /saves/test/region/done0 for floor 0 (0/255), /saves/test/region/done-1 for floor -1 (-256/-1)
    /saves/test/region/done1 for floor 1 (256/511) etc.
    Then call from pacman_cuberite (start the qsetpath_seh_64_v7 - kopie.bat for dos shell with path set to pacman_cuberite/bin dir for calling pacman.exe)
    pacman.exe plot (Enter)
  Then all the tiles create from all the region files (r.x.z.mca) top view will be in pacman_cuberite/../cut (level up -> down to cut)
  If ok you end up with all tiles from your world. (Remember placing them in /saves/test/region/done0 etc...)

  Then:
  Goto the region_finder directory and call: DO_ALL.BAT
  Then first all the tiles from pacman_cuberite/../cut/r.*.*.png will be copied into seperate dirs into 18 (dir 18).
  At the same time it merges 2x2 (4) tiles together for ZOOM LEVEL 17.
  From there the program region_finder.exe is called with region_finder tiles 17
  To create the ZOOM LEVEL directory 16 out of 17, by also combining 4 512x512 textures together and place them into seperate dirs.
  The leaflet.js script the calculates the right directory and tile name, for display them in a browser that runs javascript (tm).
  You need a webserver to test it. Like https://www.apachefriends.org/
  The script is set to: http://my.ip.address/nl/
  You can add a alias dir in: /xampp/apache/conf/alias/def.conf like:

<Directory "g:/web/map">
    Options Indexes FollowSymLinks Includes ExecCGI
    AllowOverride All
    Require all granted
</Directory>
Alias /map/ "g:/web/map/"

<Directory "g:/web/nl">
    Options Indexes FollowSymLinks Includes ExecCGI
    AllowOverride All
    Require all granted
</Directory>
Alias /nl/ "g:/web/nl/"
	
  Where in my case the directories 1 to 18 reside in /web/map and the script/web dir in /web/nl. Or you have to put it in the default dir /xampp/htdocs
  In that case (also for your/the ip address/domain url) you need to adapt the script.
  When accessing from outside your router (need to be port forwared on some ports to use the webserver for access from outside on your ip address)
  you also need to adapt the ip addresses (2 pieces) or url, AND the SHA256 key.
  To get the keys easily:
    Just use a key. When webserver runs, router port forwarded, access the http://ip-address-or_domain-name/nl/ to load the index.html
    Press F12 to get the CONSOLE window, and look what chrome expects for a RIGHT SHA256 key, THEN FILL THAT IN THE index.html Javascript stuff.
    Like:
    	<script src="./leaflet2.js"
      		integrity="sha256-qV281PHHRLgHc2Yh8xiUUYArOdLIjSu4DJKvOjuUNqk="
      		crossorigin="">
	</script>
    For using internal ip (like 192.168.178.28) or external ip the SHA keys seems differen.
    Also an older version of leaflet is in the repo, there the shader area can be adapted. The newer leaflet.js has 2x 1/2 pixel smooth so lines between
    the tiles pop up. Fix it in the newer one. The older one zooms from 18 to 16 to 14 etc. So both have some bugs to keep you busy like always.

When all ok it should work. If not: MSG ME.

Its the start, its all under construction. 
Zoomable orthographic mega renders in 3d perspective of super large minecraft 1.12.2 worlds will follow. Like some google earth 3d posters i will upload
soon, just like with the tiles from this proggie.
	
	http://83.83.222.154/nl/ (Demo when server is running).
