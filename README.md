LANDSAT-Download
================

*It seems USGS has changed the structure of its data, and so far, I have not been able to find the direct links to the products? Help welcome !*

*you might also be interested by the S2-download tool to download Sentinel-2 data
https://github.com/olivierhagolle/Sentinel-download/blob/master/README.md*

The routine provided below enables to automatically download LANDSAT data, using the current (April 2014) version of EarthExplorer system. This new version (2014-08-19) does not need to provide the exact overpass date anymore, I have reused here an idea of my colleague Michel Le Page at CESBIO (Thanks Michel !), though implemented differently.

It works for LANDSAT 8 and LANDSAT 5&7, but needs that the data be already online. It was true for LANDSAT 8 until September 2014, but after that date, to avoid increasing the on-line data volume indefinitely, USGS started to clean out older data to replace them by the new ones. It is also the case for the older LANDSAT satellites. Depending on the products you need, it may be necessary to first order for the production of L1T products, on the EarthExplorer site http://earthexplorer.usgs.gov. And of course, you will need to have an account and password on the EarthExplorer website, and you will have to store it on the usgs.txt file. If you have an access through a proxy, you might try the -p option. It works through CNES proxy at least but was only tested there.

In 2016, USGS introduced a CRSF token for the authentification. [@mkmitchell](https://github.com/mkmitchell) found a solution, which was later enhanced by [@dswanepoel](https://github.com/dswanepoel).

#### Examples :
This routine may be used in three ways :

- iterative search, by providing the WRS-2 coordinates of the LANDSAT scene, for instance, (198,030) for Toulouse (-s option), , the start date (-d option), and the end date (-f option). If the end date is not provided, it is replaced by today's date by default. Example:

`       download_landsat_scene.py -o scene -b LC8 -d 20130127  -s 198030 -u usgs.txt -p proxy.txt --output /mnt/data/LANDSAT8/N0/`
- catalog search, by providing the WRS-2 coordinates of the LANDSAT scene, for instance, (198,030) for Toulouse (-s option), , the start date (-d option), and the end date (-f option). If the end date is not provided, it is replaced by today's date by default. Example:

`       download_landsat_scene.py -o catalog -b LC8 -d 20130127  -s 198030 -u usgs.txt -p proxy.txt --output /mnt/data/LANDSAT8/N0/`

- by providing a list of products to download, as in the example below:

`       python download_landsat_scene.py -o liste -l list2_landsat8.txt -u usgs.txt --output /mnt/data/LANDSAT8/N0/`

with a file list2_landsat8.txt as provided below (the LANDSAT references must exist in the EarthExplorer catalog) :

`       Tunisie LC81910352013160LGN00`

`       Tunisie LC81910362013160LGN00`

The usgs.txt must contain your username and password on the same line separated by a blank.

If you do not use the --output option, the files will be downloaded to /tmp/Landsat (provided it exists)

Set a cloud limit to get only images with cloud cover below that limit. In the catalogue search mode, the program will get the best image below that limit. For example, if you set a limit of 20% and it finds 3 images it will download the one will less cloud cover.

To see all the options : 
`       download_landsat_scene.py -h`

The nice progress bar was provided by Jake Brinkmann ( Thanks Jake !)

Vascobnunes made large improvements, in terms of performance (connection to EarthExplorer was included in the download loop !) and added options to automatically unzip data, and configuration for LANDSAT 5 and 7. (Thanks Vascobnunes !)

## Troubleshooting
There is a difficulty with this code which is the necessity to know in advance the station where the product is received, the directory where it is stored, and the version of the product. If this is not known, there is a need to try the various possibilities, which takes some time. It is also not excluded that USGS might change these values from time to time. They ghanged it for instance when introducing Landsat Coolelctions

Here is what we found so far :

| Satellite name | directory    | Stations                                       | Versions |

| LT5            |  12266  | GLC,ASA,KIR,MOR,KHC,PAC,KIS,CHM,LGS,MGR,COA,MPS,JSA |   0-1    |

| LE7            |  12267  |'EDC','SGS','AGS','ASN'                         |   0-1    |

| LT8            |  12864  | 'LGN'                                          |   0-5    |


If the configuration we provide by default does not work, you may provide the directory and station on the command line, using options --dir and --station . If you use thess options, it might speed a little the download as the tool will not need to search for all possible stations.


To find the station, you can try to download a preoduct from the user interface (https://earthexplorer.usgs.gov), annd check with the web console which dir and station the product belongs to. Here is for instance a screen copy provided by 

<img  title="How to get product directory and station" src="https://github.com/olivierhagolle/LANDSAT-Download/blob/master/find_dir_and_station.jpg" alt="" width="130"  />
