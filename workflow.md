Since we are stuck in VI... we need to learn a few things

( :q if you want the easy way out )

But we like the hard way

`C-z`     <-- suspend your current process

`fg`      <-- bring suspended process into forground

`C-a`     <-- beginning of line

`C-e`     <-- end of line

`ps`      <-- list your processes

`ps -ef`  <-- list them all

`grep <term> <files>`      <-- find something in file

`grep -i <term> <files>`   <-- case insensitive

`grep -in <term> <files>`  <-- add line numbers

`grep -r <term> <dir>`     <-- recursive in a directory

`find . -name "*.html" -exec grep -in <term> {} \;`  <-- more powerful recursive grep

`cmd | grep <term>`        <-- use pipe to chain commands (cool!)

`ps | grep vi`         <-- find our VI process

get too many... need to exclude the word "grep"

`ps | grep -v "grep" | grep vi`  <-- Now we have our process!

`ps | grep -v "grep" | grep vi | awk '{ print $1 }'`   <-- Get just the ID
 
``` `` ``` <-- Execute inline

Kill the process... show no mercy :-)

``` kill -9 `ps | grep -v "grep" | grep vi | awk '{ print $1 }'` ```

Open in real editor

`emacs workflow.txt`   <-- Winning!!!

All joking aside... clifu is legit...

======================================
======================================

A few more OS related tips::

GET OUT OF CURRENT PROCESS

`C-c`     <-- Stop current process

TAB TAB TAB

`<tab>` to complete files/dirs/cmds

`double<tab>` to see completions

PIPE|REDIRECT|APPEND

`|`   <-- pipe stdout to next command

`>`   <-- redirect stdout to new file (overwrite if exists)

`>>`  <-- redirect stdout to new file (append if exists)

`ls > my_directory_listing.txt`

HISTORY

`C-r`  <-- reverse history search... my go to command!

HEAD/TAIL

`head -n 50 myfile`   <-- Print the first 50 lines of a file

`tail -n 50 myfile`   <-- Print the last 50 lines of a file

`tail -f myfile`      <-- Monitor the end of the file

`tail -f /var/log/system.log`  <-- Monitor system log 

(run sudo command in another window)

`C-c` when done

COUNT THINGS

`wc -l`  <-- number of lines

`ls *.shp | wc -l`  <-- number of shapefiles

DIFFS

diff works on dirs as well as files

`diff old new`     <-- Compare old and new file

`diff dir1 dir2`   <-- Show dir and file diffs

FOR LOOP

`for i in {1..5}; do COMMAND-HERE; done`

`for f in us*; do echo $f; done`  <-- print all the filenames 
                                    starting with 'us'

basename is cool...

`for f in *.json; do echo ogr2ogr $f $(basename $f .json).shp; done`

SSH AGENT

`ssh-agent bash`          <-- start an ssh-agent shell

`ssh-add ~/.ssh/id_rsa`   <-- Add your private key

`ssh -A user@server.com`  <-- Log into server and have 
                            your priv key available!

SCREEN - Keep your session alive if connection drops

`screen`  <-- Does not get much simpler than that

WEB (Downloading things)

`wget http://download.osgeo.org/geotiff/samples/usgs/f41078e1.tif`

or 

`curl http://download.osgeo.org/geotiff/samples/usgs/f41078e1.tif > my.tif`

LOCAL SERVER

`python -m SimpleHTTPServer`


=========================================
=========================================
=========================================


--------------  GEO ---------------------

Lets look at some gdal/ogr command line fu

VSI is an interesting feature... great for pipe |

`ogr2ogr -f GeoJSON /vsistdout/ us_germany.shp`

tough to read... lets clean it up with python -m json.tool

`ogr2ogr -f GeoJSON /vsistdout/ us_germany.shp | python -m json.tool`

How about filter some stuff

`ogr2ogr -f GeoJSON /vsistdout/ us_germany.shp -where "name = 'Germany'" | python -m json.tool`

Or with more sql to filter attributes

`ogr2ogr -f GeoJSON /vsistdout/ us_germany.shp -sql "select economy from us_germany where name = 'Germany'" | python -m json.tool`

You can use it to download from the web as well with vsicurl

`ogr2ogr -f GeoJSON /vsistdout/ /vsizip/vsicurl/http://www.naturalearthdata.com/http//www.naturalearthdata.com/download/110m/cultural/ne_110m_admin_0_countries.zip/ne_110m_admin_0_countries.shp -sql "select economy from ne_110m_admin_0_countries where name='Germany'" | python -m json.tool`

How about a building a VRT (virtual file)

`gdalbuildvrt test.vrt *.tif`   <-- use gdalbuildvrt to make virtual raster

`QGIS test.vrt`

Or using VRT's to specify external tile sources

`QGIS mapbox.xml`


