## Capston Project 

<li>We are required to submit the following :</li>


<li> 1) A description of the problem and a discussion of the background. (15 marks)</li> 
<li>2) A description of the data and how it will be used to solve the problem. (15 marks) </li>
<li>3) A link to your Notebook on your Github repository, showing your code. (15 marks)</li>
<li>4) A full report consisting of all of the following components (15 marks):</li>
<li>3. Your choice of a presentation or blogpost. (10 marks).

## Introduction 

As known, London is considered one of the most important, expensive and visited city in the globe. It has diverse cultures with various races and demographics. In short, London is a mix of everything and we can write uncountable pages describing its culture, history and economy. Thus, in this project, I will address the issue of moving to the city of London as an expat or as an international students or any other reasons that make you move to London. I will consider some criteria in listing the best boroughs to live or to work or to study based on safety measurment, average renting rate. Besides, I will address the population rate in each boroughs for further measuremnt and analysis. Once again, my concentration is the safety and the renting rate.Furthermore, I did not point out about the transportation services or the entertainment venues since, I do belive that London is a well linked city in terms of transportation services and with that it will be easy to reach entertaining venues through varoius transportaion services that the city provides to its people and visitors.  

#### Business Problem:

 The problem is to precisely find the best place that possess the two criteria since it is only figure of the last two years and did not address in depth the kind of crimes or the category of accomodation based on specific details. the study is limited to period of time and it is exposed to differnt changes that casues a deacresing the crimes rates or even the rentig rates

#### Targeted Audience

For everyone who wants to move and live in a megacity in general, and for tourists and visitors who are looking for special criteria based on their preferences. More specifically, for those heading to London in the near Future.



#### Data needed:

<li>The needed data For this case are as the following:</li>
<li>1)	List of Boroughs and neighborhoods of city of London with their geodata (latitude and longitude) and thier population.</li>
<li>2)	List of crimes in London’s boroughs with their addresses. </li>  
<li>3) List of boroughs with home renting prices (least cost)</li> 


#### How to use the data

<li> The data will be used as follows:</li>
<li> 1) Use Foursquare and geopy to map the most 10 populous boroughs.</li>
<li> 2)	Use Foursquare and geopy data to map the location of broughs with least crimes in London.</li> 
<li> 3)	Use Foursquare and geopy data to map top 10 affordable venues for all London neighborhoods.</li> 
<li> 4) list the best boroughs to live according to the previous places.</li>



```python
# import libraries
import numpy as np # library to handle data in a vectorized manner
import time
import pandas as pd # library for data analsysis
pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', None)
!pip install bs4
!pip install lxml
!pip install xlrd
!pip install html5lib
import lxml 
import xlrd
from bs4 import BeautifulSoup 
import json # library to handle JSON files
import requests # library to handle requests
from pandas.io.json import json_normalize # tranform JSON file into a pandas dataframe

from geopy.geocoders import Nominatim # convert an address into latitude and longitude values
!pip install folium 
# uncomment this line if you haven't completed the Foursquare API lab
import folium # map rendering library
print ('folium installed')
print('Libraries imported.')
```

    Requirement already satisfied: bs4 in c:\users\dalal\appdata\local\programs\python\python38\lib\site-packages (0.0.1)

    WARNING: You are using pip version 19.2.3, however version 20.1.1 is available.
    You should consider upgrading via the 'python -m pip install --upgrade pip' command.
    

    
    Requirement already satisfied: beautifulsoup4 in c:\users\dalal\appdata\local\programs\python\python38\lib\site-packages (from bs4) (4.9.1)
    Requirement already satisfied: soupsieve>1.2 in c:\users\dalal\appdata\local\programs\python\python38\lib\site-packages (from beautifulsoup4->bs4) (2.0.1)
    Requirement already satisfied: lxml in c:\users\dalal\appdata\local\programs\python\python38\lib\site-packages (4.5.1)
    

    WARNING: You are using pip version 19.2.3, however version 20.1.1 is available.
    You should consider upgrading via the 'python -m pip install --upgrade pip' command.
    

    Requirement already satisfied: xlrd in c:\users\dalal\appdata\local\programs\python\python38\lib\site-packages (1.2.0)

    WARNING: You are using pip version 19.2.3, however version 20.1.1 is available.
    You should consider upgrading via the 'python -m pip install --upgrade pip' command.
    

    
    

    WARNING: You are using pip version 19.2.3, however version 20.1.1 is available.
    You should consider upgrading via the 'python -m pip install --upgrade pip' command.
    

    Requirement already satisfied: html5lib in c:\users\dalal\appdata\local\programs\python\python38\lib\site-packages (1.0.1)
    Requirement already satisfied: six>=1.9 in c:\users\dalal\appdata\roaming\python\python38\site-packages (from html5lib) (1.14.0)
    Requirement already satisfied: webencodings in c:\users\dalal\appdata\local\programs\python\python38\lib\site-packages (from html5lib) (0.5.1)
    Requirement already satisfied: folium in c:\users\dalal\appdata\local\programs\python\python38\lib\site-packages (0.11.0)
    Requirement already satisfied: branca>=0.3.0 in c:\users\dalal\appdata\local\programs\python\python38\lib\site-packages (from folium) (0.4.1)
    Requirement already satisfied: numpy in c:\users\dalal\appdata\roaming\python\python38\site-packages (from folium) (1.18.2)
    Requirement already satisfied: requests in c:\users\dalal\appdata\roaming\python\python38\site-packages (from folium) (2.23.0)
    Requirement already satisfied: jinja2>=2.9 in c:\users\dalal\appdata\local\programs\python\python38\lib\site-packages (from folium) (2.11.2)
    Requirement already satisfied: certifi>=2017.4.17 in c:\users\dalal\appdata\roaming\python\python38\site-packages (from requests->folium) (2020.4.5.1)
    Requirement already satisfied: chardet<4,>=3.0.2 in c:\users\dalal\appdata\roaming\python\python38\site-packages (from requests->folium) (3.0.4)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in c:\users\dalal\appdata\roaming\python\python38\site-packages (from requests->folium) (1.25.8)
    Requirement already satisfied: idna<3,>=2.5 in c:\users\dalal\appdata\roaming\python\python38\site-packages (from requests->folium) (2.9)
    Requirement already satisfied: MarkupSafe>=0.23 in c:\users\dalal\appdata\local\programs\python\python38\lib\site-packages (from jinja2>=2.9->folium) (1.1.1)
    

    WARNING: You are using pip version 19.2.3, however version 20.1.1 is available.
    You should consider upgrading via the 'python -m pip install --upgrade pip' command.
    

    folium installed
    Libraries imported.
    

#### 1) List of Boroughs and neighborhoods of city of London with their geodata (latitude and longitude).</li>
import a list of London's Boroughs with thier respective long and lat.


```python
# I will soup a wiki page about list of boroughs in London 
source = requests.get('https://en.wikipedia.org/wiki/List_of_London_boroughs').text
soup = BeautifulSoup(source, 'lxml')
soup.encode("utf-8-sig")
```




    b'\xef\xbb\xbf<!DOCTYPE html>\n<html class="client-nojs" dir="ltr" lang="en">\n<head>\n<meta charset="utf-8-sig"/>\n<title>List of London boroughs - Wikipedia</title>\n<script>document.documentElement.className="client-js";RLCONF={"wgBreakFrames":!1,"wgSeparatorTransformTable":["",""],"wgDigitTransformTable":["",""],"wgDefaultDateFormat":"dmy","wgMonthNames":["","January","February","March","April","May","June","July","August","September","October","November","December"],"wgRequestId":"2197f497-1f95-44dc-af63-10628c3312b2","wgCSPNonce":!1,"wgCanonicalNamespace":"","wgCanonicalSpecialPageName":!1,"wgNamespaceNumber":0,"wgPageName":"List_of_London_boroughs","wgTitle":"List of London boroughs","wgCurRevisionId":958873870,"wgRevisionId":958873870,"wgArticleId":28092685,"wgIsArticle":!0,"wgIsRedirect":!1,"wgAction":"view","wgUserName":null,"wgUserGroups":["*"],"wgCategories":["Use dmy dates from August 2015","Use British English from August 2015","Lists of coordinates","Geographic coordinate lists","Articles with Geo","London boroughs","Lists of places in London"],"wgPageContentLanguage":"en","wgPageContentModel":"wikitext","wgRelevantPageName":\n"List_of_London_boroughs","wgRelevantArticleId":28092685,"wgIsProbablyEditable":!0,"wgRelevantPageIsProbablyEditable":!0,"wgRestrictionEdit":[],"wgRestrictionMove":[],"wgMediaViewerOnClick":!0,"wgMediaViewerEnabledByDefault":!0,"wgPopupsReferencePreviews":!1,"wgPopupsConflictsWithNavPopupGadget":!1,"wgVisualEditor":{"pageLanguageCode":"en","pageLanguageDir":"ltr","pageVariantFallbacks":"en"},"wgMFDisplayWikibaseDescriptions":{"search":!0,"nearby":!0,"watchlist":!0,"tagline":!1},"wgWMESchemaEditAttemptStepOversample":!1,"wgULSCurrentAutonym":"English","wgNoticeProject":"wikipedia","wgWikibaseItemId":"Q6577004","wgCentralAuthMobileDomain":!1,"wgEditSubmitButtonLabelPublish":!0};RLSTATE={"ext.globalCssJs.user.styles":"ready","site.styles":"ready","noscript":"ready","user.styles":"ready","ext.globalCssJs.user":"ready","user":"ready","user.options":"loading","ext.cite.styles":"ready","jquery.tablesorter.styles":"ready","jquery.makeCollapsible.styles":"ready",\n"mediawiki.toc.styles":"ready","skins.vector.styles.legacy":"ready","wikibase.client.init":"ready","ext.visualEditor.desktopArticleTarget.noscript":"ready","ext.uls.interlanguage":"ready","ext.wikimediaBadges":"ready"};RLPAGEMODULES=["ext.cite.ux-enhancements","site","mediawiki.page.startup","skins.vector.js","mediawiki.page.ready","jquery.tablesorter","jquery.makeCollapsible","mediawiki.toc","ext.gadget.ReferenceTooltips","ext.gadget.charinsert","ext.gadget.refToolbar","ext.gadget.extra-toolbar-buttons","ext.gadget.switcher","ext.centralauth.centralautologin","mmv.head","mmv.bootstrap.autostart","ext.popups","ext.visualEditor.desktopArticleTarget.init","ext.visualEditor.targetLoader","ext.eventLogging","ext.wikimediaEvents","ext.navigationTiming","ext.uls.compactlinks","ext.uls.interface","ext.cx.eventlogging.campaigns","ext.quicksurveys.init","ext.centralNotice.geoIP","ext.centralNotice.startUp"];</script>\n<script>(RLQ=window.RLQ||[]).push(function(){mw.loader.implement("user.options@1hzgi",function($,jQuery,require,module){/*@nomin*/mw.user.tokens.set({"patrolToken":"+\\\\","watchToken":"+\\\\","csrfToken":"+\\\\"});\n});});</script>\n<link href="/w/load.php?lang=en&amp;modules=ext.cite.styles%7Cext.uls.interlanguage%7Cext.visualEditor.desktopArticleTarget.noscript%7Cext.wikimediaBadges%7Cjquery.makeCollapsible.styles%7Cjquery.tablesorter.styles%7Cmediawiki.toc.styles%7Cskins.vector.styles.legacy%7Cwikibase.client.init&amp;only=styles&amp;skin=vector" rel="stylesheet"/>\n<script async="" src="/w/load.php?lang=en&amp;modules=startup&amp;only=scripts&amp;raw=1&amp;skin=vector"></script>\n<meta content="" name="ResourceLoaderDynamicStyles"/>\n<link href="/w/load.php?lang=en&amp;modules=site.styles&amp;only=styles&amp;skin=vector" rel="stylesheet"/>\n<meta content="MediaWiki 1.35.0-wmf.32" name="generator"/>\n<meta content="origin" name="referrer"/>\n<meta content="origin-when-crossorigin" name="referrer"/>\n<meta content="origin-when-cross-origin" name="referrer"/>\n<link href="/w/index.php?title=List_of_London_boroughs&amp;action=edit" rel="alternate" title="Edit this page" type="application/x-wiki"/>\n<link href="/w/index.php?title=List_of_London_boroughs&amp;action=edit" rel="edit" title="Edit this page"/>\n<link href="/static/apple-touch/wikipedia.png" rel="apple-touch-icon"/>\n<link href="/static/favicon/wikipedia.ico" rel="shortcut icon"/>\n<link href="/w/opensearch_desc.php" rel="search" title="Wikipedia (en)" type="application/opensearchdescription+xml"/>\n<link href="//en.wikipedia.org/w/api.php?action=rsd" rel="EditURI" type="application/rsd+xml"/>\n<link href="//creativecommons.org/licenses/by-sa/3.0/" rel="license"/>\n<link href="/w/index.php?title=Special:RecentChanges&amp;feed=atom" rel="alternate" title="Wikipedia Atom feed" type="application/atom+xml"/>\n<link href="https://en.wikipedia.org/wiki/List_of_London_boroughs" rel="canonical"/>\n<link href="//login.wikimedia.org" rel="dns-prefetch"/>\n<link href="//meta.wikimedia.org" rel="dns-prefetch"/>\n<!--[if lt IE 9]><script src="/w/resources/lib/html5shiv/html5shiv.js"></script><![endif]-->\n</head>\n<body class="mediawiki ltr sitedir-ltr mw-hide-empty-elt ns-0 ns-subject mw-editable page-List_of_London_boroughs rootpage-List_of_London_boroughs skin-vector action-view skin-vector-legacy">\n<div class="noprint" id="mw-page-base"></div>\n<div class="noprint" id="mw-head-base"></div>\n<div class="mw-body" id="content" role="main">\n<a id="top"></a>\n<div class="mw-body-content" id="siteNotice"><!-- CentralNotice --></div>\n<div class="mw-indicators mw-body-content">\n</div>\n<h1 class="firstHeading" id="firstHeading" lang="en">List of London boroughs</h1>\n<div class="mw-body-content" id="bodyContent">\n<div class="noprint" id="siteSub">From Wikipedia, the free encyclopedia</div>\n<div id="contentSub"></div>\n<div id="jump-to-nav"></div>\n<a class="mw-jump-link" href="#mw-head">Jump to navigation</a>\n<a class="mw-jump-link" href="#p-search">Jump to search</a>\n<div class="mw-content-ltr" dir="ltr" id="mw-content-text" lang="en"><div class="mw-parser-output"><p class="mw-empty-elt">\n</p>\n<div class="thumb tright"><div class="thumbinner" style="width:302px;"><a class="image" href="/wiki/File:London-boroughs.svg"><img alt="" class="thumbimage" data-file-height="386" data-file-width="489" decoding="async" height="237" src="//upload.wikimedia.org/wikipedia/commons/thumb/2/29/London-boroughs.svg/300px-London-boroughs.svg.png" srcset="//upload.wikimedia.org/wikipedia/commons/thumb/2/29/London-boroughs.svg/450px-London-boroughs.svg.png 1.5x, //upload.wikimedia.org/wikipedia/commons/thumb/2/29/London-boroughs.svg/600px-London-boroughs.svg.png 2x" width="300"/></a> <div class="thumbcaption"><div class="magnify"><a class="internal" href="/wiki/File:London-boroughs.svg" title="Enlarge"></a></div>Map of the 32 London boroughs and the City of London.</div></div></div>\n<p>This is a list of <a href="/wiki/Districts_of_England" title="Districts of England">local authority districts</a> within <a href="/wiki/Greater_London" title="Greater London">Greater London</a>, including 32 <a href="/wiki/London_boroughs" title="London boroughs">London boroughs</a> and the <a href="/wiki/City_of_London" title="City of London">City of London</a>. The London boroughs were all created on 1 April 1965. Upon creation, twelve were designated <a href="/wiki/Inner_London" title="Inner London">Inner London</a> boroughs and the remaining twenty were designated <a href="/wiki/Outer_London" title="Outer London">Outer London</a> boroughs. The <a class="mw-redirect" href="/wiki/Office_for_National_Statistics" title="Office for National Statistics">Office for National Statistics</a> has amended the designations of three boroughs for statistics purposes only. Three boroughs have been granted the designation <a class="mw-redirect" href="/wiki/Royal_borough" title="Royal borough">royal borough</a> and one has <a href="/wiki/City_status_in_the_United_Kingdom" title="City status in the United Kingdom">city status</a>. For planning purposes, in addition to the boroughs and City there are also two active development corporations, the <a href="/wiki/London_Legacy_Development_Corporation" title="London Legacy Development Corporation">London Legacy Development Corporation</a> and <a href="/wiki/Old_Oak_and_Park_Royal_Development_Corporation" title="Old Oak and Park Royal Development Corporation">Old Oak and Park Royal Development Corporation</a>.\n</p>\n<div aria-labelledby="mw-toc-heading" class="toc" id="toc" role="navigation"><input class="toctogglecheckbox" id="toctogglecheckbox" role="button" style="display:none" type="checkbox"/><div class="toctitle" dir="ltr" lang="en"><h2 id="mw-toc-heading">Contents</h2><span class="toctogglespan"><label class="toctogglelabel" for="toctogglecheckbox"></label></span></div>\n<ul>\n<li class="toclevel-1 tocsection-1"><a href="#List_of_boroughs_and_local_authorities"><span class="tocnumber">1</span> <span class="toctext">List of boroughs and local authorities</span></a></li>\n<li class="toclevel-1 tocsection-2"><a href="#City_of_London"><span class="tocnumber">2</span> <span class="toctext">City of London</span></a></li>\n<li class="toclevel-1 tocsection-3"><a href="#See_also"><span class="tocnumber">3</span> <span class="toctext">See also</span></a></li>\n<li class="toclevel-1 tocsection-4"><a href="#Notes"><span class="tocnumber">4</span> <span class="toctext">Notes</span></a></li>\n<li class="toclevel-1 tocsection-5"><a href="#References"><span class="tocnumber">5</span> <span class="toctext">References</span></a></li>\n<li class="toclevel-1 tocsection-6"><a href="#External_links"><span class="tocnumber">6</span> <span class="toctext">External links</span></a></li>\n</ul>\n</div>\n<h2><span class="mw-headline" id="List_of_boroughs_and_local_authorities">List of boroughs and local authorities</span><span class="mw-editsection"><span class="mw-editsection-bracket">[</span><a href="/w/index.php?title=List_of_London_boroughs&amp;action=edit&amp;section=1" title="Edit section: List of boroughs and local authorities">edit</a><span class="mw-editsection-bracket">]</span></span></h2>\n<table class="wikitable sortable" style="font-size:100%" width="100%">\n<tbody><tr>\n<th>Borough\n</th>\n<th>Inner\n</th>\n<th>Status\n</th>\n<th>Local authority\n</th>\n<th>Political control\n</th>\n<th>Headquarters\n</th>\n<th>Area (sq mi)\n</th>\n<th>Population (2013 est)<sup class="reference" id="cite_ref-1"><a href="#cite_note-1">[1]</a></sup>\n</th>\n<th>Co-ordinates\n</th>\n<th><span style="background:#67BCD3"> Nr. in map </span>\n</th></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Barking_and_Dagenham" title="London Borough of Barking and Dagenham">Barking and Dagenham</a> <sup class="reference" id="cite_ref-2"><a href="#cite_note-2">[note 1]</a></sup>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Barking_and_Dagenham_London_Borough_Council" title="Barking and Dagenham London Borough Council">Barking and Dagenham London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Barking_Town_Hall" title="Barking Town Hall">Town Hall</a>, 1 Town Square\n</td>\n<td>13.93\n</td>\n<td>194,352\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.5607_N_0.1557_E_region:GB_type:city&amp;title=Barking+and+Dagenham"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb033\xe2\x80\xb239\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb009\xe2\x80\xb221\xe2\x80\xb3E</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5607\xc2\xb0N 0.1557\xc2\xb0E</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5607; 0.1557</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Barking and Dagenham</span>)</span></span></span></a></span>\n</td>\n<td>25\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Barnet" title="London Borough of Barnet">Barnet</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Barnet_London_Borough_Council" title="Barnet London Borough Council">Barnet London Borough Council</a>\n</td>\n<td><a href="/wiki/Conservative_Party_(UK)" title="Conservative Party (UK)">Conservative</a>\n</td>\n<td><a href="/wiki/Hendon_Town_Hall#Barnet_House" title="Hendon Town Hall">Barnet House</a>, 2 Bristol Avenue, Colindale\n</td>\n<td>33.49\n</td>\n<td>369,088\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.6252_N_0.1517_W_region:GB_type:city&amp;title=Barnet"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb037\xe2\x80\xb231\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb009\xe2\x80\xb206\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.6252\xc2\xb0N 0.1517\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.6252; -0.1517</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Barnet</span>)</span></span></span></a></span>\n</td>\n<td>31\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Bexley" title="London Borough of Bexley">Bexley</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Bexley_London_Borough_Council" title="Bexley London Borough Council">Bexley London Borough Council</a>\n</td>\n<td><a href="/wiki/Conservative_Party_(UK)" title="Conservative Party (UK)">Conservative</a>\n</td>\n<td><a href="/wiki/Bexley_Civic_Offices" title="Bexley Civic Offices">Civic Offices</a>, 2 Watling Street\n</td>\n<td>23.38\n</td>\n<td>236,687\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.4549_N_0.1505_E_region:GB_type:city&amp;title=Bexley"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb027\xe2\x80\xb218\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb009\xe2\x80\xb202\xe2\x80\xb3E</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.4549\xc2\xb0N 0.1505\xc2\xb0E</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.4549; 0.1505</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Bexley</span>)</span></span></span></a></span>\n</td>\n<td>23\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Brent" title="London Borough of Brent">Brent</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Brent_London_Borough_Council" title="Brent London Borough Council">Brent London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Brent_Civic_Centre" title="Brent Civic Centre">Brent Civic Centre</a>, Engineers Way\n</td>\n<td>16.70\n</td>\n<td>317,264\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.5588_N_0.2817_W_region:GB_type:city&amp;title=Brent"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb033\xe2\x80\xb232\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb016\xe2\x80\xb254\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5588\xc2\xb0N 0.2817\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5588; -0.2817</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Brent</span>)</span></span></span></a></span>\n</td>\n<td>12\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Bromley" title="London Borough of Bromley">Bromley</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Bromley_London_Borough_Council" title="Bromley London Borough Council">Bromley London Borough Council</a>\n</td>\n<td><a href="/wiki/Conservative_Party_(UK)" title="Conservative Party (UK)">Conservative</a>\n</td>\n<td><a href="/wiki/Bromley_Palace" title="Bromley Palace">Civic Centre</a>, Stockwell Close\n</td>\n<td>57.97\n</td>\n<td>317,899\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.4039_N_0.0198_E_region:GB_type:city&amp;title=Bromley"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb024\xe2\x80\xb214\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb001\xe2\x80\xb211\xe2\x80\xb3E</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.4039\xc2\xb0N 0.0198\xc2\xb0E</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.4039; 0.0198</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Bromley</span>)</span></span></span></a></span>\n</td>\n<td>20\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Camden" title="London Borough of Camden">Camden</a>\n</td>\n<td><img alt="\xe2\x98\x91" data-file-height="600" data-file-width="600" decoding="async" height="20" src="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/20px-Yes_check.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/30px-Yes_check.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/40px-Yes_check.svg.png 2x" width="20"/><span style="display:none">Y</span>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Camden_London_Borough_Council" title="Camden London Borough Council">Camden London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Camden_Town_Hall" title="Camden Town Hall">Camden Town Hall</a>, Judd Street\n</td>\n<td>8.40\n</td>\n<td>229,719\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.529_N_0.1255_W_region:GB_type:city&amp;title=Camden"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb031\xe2\x80\xb244\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb007\xe2\x80\xb232\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5290\xc2\xb0N 0.1255\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5290; -0.1255</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Camden</span>)</span></span></span></a></span>\n</td>\n<td>11\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Croydon" title="London Borough of Croydon">Croydon</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Croydon_London_Borough_Council" title="Croydon London Borough Council">Croydon London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Bernard_Weatherill_House" title="Bernard Weatherill House">Bernard Weatherill House</a>, Mint Walk\n</td>\n<td>33.41\n</td>\n<td>372,752\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.3714_N_0.0977_W_region:GB_type:city&amp;title=Croydon"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb022\xe2\x80\xb217\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb005\xe2\x80\xb252\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.3714\xc2\xb0N 0.0977\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.3714; -0.0977</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Croydon</span>)</span></span></span></a></span>\n</td>\n<td>19\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Ealing" title="London Borough of Ealing">Ealing</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Ealing_London_Borough_Council" title="Ealing London Borough Council">Ealing London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Ealing_Town_Hall#Perceval_House" title="Ealing Town Hall">Perceval House</a>, 14-16 Uxbridge Road\n</td>\n<td>21.44\n</td>\n<td>342,494\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.513_N_0.3089_W_region:GB_type:city&amp;title=Ealing"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb030\xe2\x80\xb247\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb018\xe2\x80\xb232\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5130\xc2\xb0N 0.3089\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5130; -0.3089</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Ealing</span>)</span></span></span></a></span>\n</td>\n<td>13\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Enfield" title="London Borough of Enfield">Enfield</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Enfield_London_Borough_Council" title="Enfield London Borough Council">Enfield London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Enfield_Civic_Centre" title="Enfield Civic Centre">Civic Centre</a>, Silver Street\n</td>\n<td>31.74\n</td>\n<td>320,524\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.6538_N_0.0799_W_region:GB_type:city&amp;title=Enfield"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb039\xe2\x80\xb214\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb004\xe2\x80\xb248\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.6538\xc2\xb0N 0.0799\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.6538; -0.0799</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Enfield</span>)</span></span></span></a></span>\n</td>\n<td>30\n</td></tr>\n<tr>\n<td><a href="/wiki/Royal_Borough_of_Greenwich" title="Royal Borough of Greenwich">Greenwich</a> <sup class="reference" id="cite_ref-3"><a href="#cite_note-3">[note 2]</a></sup>\n</td>\n<td><img alt="\xe2\x98\x91" data-file-height="600" data-file-width="600" decoding="async" height="20" src="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/20px-Yes_check.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/30px-Yes_check.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/40px-Yes_check.svg.png 2x" width="20"/><span style="display:none">Y</span> <sup class="reference" id="cite_ref-note2_4-0"><a href="#cite_note-note2-4">[note 3]</a></sup>\n</td>\n<td><a class="mw-redirect" href="/wiki/Royal_borough" title="Royal borough">Royal</a>\n</td>\n<td><a href="/wiki/Greenwich_London_Borough_Council" title="Greenwich London Borough Council">Greenwich London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Woolwich_Town_Hall" title="Woolwich Town Hall">Woolwich Town Hall</a>, Wellington Street\n</td>\n<td>18.28\n</td>\n<td>264,008\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.4892_N_0.0648_E_region:GB_type:city&amp;title=Greenwich"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb029\xe2\x80\xb221\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb003\xe2\x80\xb253\xe2\x80\xb3E</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.4892\xc2\xb0N 0.0648\xc2\xb0E</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.4892; 0.0648</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Greenwich</span>)</span></span></span></a></span>\n</td>\n<td>22\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Hackney" title="London Borough of Hackney">Hackney</a>\n</td>\n<td><img alt="\xe2\x98\x91" data-file-height="600" data-file-width="600" decoding="async" height="20" src="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/20px-Yes_check.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/30px-Yes_check.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/40px-Yes_check.svg.png 2x" width="20"/><span style="display:none">Y</span>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Hackney_London_Borough_Council" title="Hackney London Borough Council">Hackney London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Hackney_Town_Hall" title="Hackney Town Hall">Hackney Town Hall</a>, Mare Street\n</td>\n<td>7.36\n</td>\n<td>257,379\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.545_N_0.0553_W_region:GB_type:city&amp;title=Hackney"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb032\xe2\x80\xb242\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb003\xe2\x80\xb219\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5450\xc2\xb0N 0.0553\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5450; -0.0553</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Hackney</span>)</span></span></span></a></span>\n</td>\n<td>9\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Hammersmith_and_Fulham" title="London Borough of Hammersmith and Fulham">Hammersmith and Fulham</a> <sup class="reference" id="cite_ref-5"><a href="#cite_note-5">[note 4]</a></sup>\n</td>\n<td><img alt="\xe2\x98\x91" data-file-height="600" data-file-width="600" decoding="async" height="20" src="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/20px-Yes_check.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/30px-Yes_check.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/40px-Yes_check.svg.png 2x" width="20"/><span style="display:none">Y</span>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Hammersmith_and_Fulham_London_Borough_Council" title="Hammersmith and Fulham London Borough Council">Hammersmith and Fulham London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Hammersmith_Town_Hall" title="Hammersmith Town Hall">Town Hall</a>, King Street\n</td>\n<td>6.33\n</td>\n<td>178,685\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.4927_N_0.2339_W_region:GB_type:city&amp;title=Hammersmith+and+Fulham"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb029\xe2\x80\xb234\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb014\xe2\x80\xb202\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.4927\xc2\xb0N 0.2339\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.4927; -0.2339</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Hammersmith and Fulham</span>)</span></span></span></a></span>\n</td>\n<td>4\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Haringey" title="London Borough of Haringey">Haringey</a>\n</td>\n<td><sup class="reference" id="cite_ref-note2_4-1"><a href="#cite_note-note2-4">[note 3]</a></sup>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Haringey_London_Borough_Council" title="Haringey London Borough Council">Haringey London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Haringey_Civic_Centre" title="Haringey Civic Centre">Civic Centre</a>, High Road\n</td>\n<td>11.42\n</td>\n<td>263,386\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.6_N_0.1119_W_region:GB_type:city&amp;title=Haringey"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb036\xe2\x80\xb200\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb006\xe2\x80\xb243\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.6000\xc2\xb0N 0.1119\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.6000; -0.1119</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Haringey</span>)</span></span></span></a></span>\n</td>\n<td>29\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Harrow" title="London Borough of Harrow">Harrow</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Harrow_London_Borough_Council" title="Harrow London Borough Council">Harrow London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Harrow_Civic_Centre" title="Harrow Civic Centre">Civic Centre</a>, Station Road\n</td>\n<td>19.49\n</td>\n<td>243,372\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.5898_N_0.3346_W_region:GB_type:city&amp;title=Harrow"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb035\xe2\x80\xb223\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb020\xe2\x80\xb205\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5898\xc2\xb0N 0.3346\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5898; -0.3346</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Harrow</span>)</span></span></span></a></span>\n</td>\n<td>32\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Havering" title="London Borough of Havering">Havering</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Havering_London_Borough_Council" title="Havering London Borough Council">Havering London Borough Council</a>\n</td>\n<td><a href="/wiki/Conservative_Party_(UK)" title="Conservative Party (UK)">Conservative</a> (council <a href="/wiki/No_overall_control" title="No overall control">NOC</a>)\n</td>\n<td><a href="/wiki/Havering_Town_Hall" title="Havering Town Hall">Town Hall</a>, Main Road\n</td>\n<td>43.35\n</td>\n<td>242,080\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.5812_N_0.1837_E_region:GB_type:city&amp;title=Havering"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb034\xe2\x80\xb252\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb011\xe2\x80\xb201\xe2\x80\xb3E</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5812\xc2\xb0N 0.1837\xc2\xb0E</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5812; 0.1837</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Havering</span>)</span></span></span></a></span>\n</td>\n<td>24\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Hillingdon" title="London Borough of Hillingdon">Hillingdon</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Hillingdon_London_Borough_Council" title="Hillingdon London Borough Council">Hillingdon London Borough Council</a>\n</td>\n<td><a href="/wiki/Conservative_Party_(UK)" title="Conservative Party (UK)">Conservative</a>\n</td>\n<td><a href="/wiki/Hillingdon_Civic_Centre" title="Hillingdon Civic Centre">Civic Centre</a>, High Street\n</td>\n<td>44.67\n</td>\n<td>286,806\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.5441_N_0.476_W_region:GB_type:city&amp;title=Hillingdon"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb032\xe2\x80\xb239\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb028\xe2\x80\xb234\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5441\xc2\xb0N 0.4760\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5441; -0.4760</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Hillingdon</span>)</span></span></span></a></span>\n</td>\n<td>33\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Hounslow" title="London Borough of Hounslow">Hounslow</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Hounslow_London_Borough_Council" title="Hounslow London Borough Council">Hounslow London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Hounslow_Civic_Centre#Hounslow_House" title="Hounslow Civic Centre">Hounslow House</a>, 7 Bath Road\n</td>\n<td>21.61\n</td>\n<td>262,407\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.4746_N_0.368_W_region:GB_type:city&amp;title=Hounslow"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb028\xe2\x80\xb229\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb022\xe2\x80\xb205\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.4746\xc2\xb0N 0.3680\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.4746; -0.3680</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Hounslow</span>)</span></span></span></a></span>\n</td>\n<td>14\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Islington" title="London Borough of Islington">Islington</a>\n</td>\n<td><img alt="\xe2\x98\x91" data-file-height="600" data-file-width="600" decoding="async" height="20" src="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/20px-Yes_check.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/30px-Yes_check.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/40px-Yes_check.svg.png 2x" width="20"/><span style="display:none">Y</span>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Islington_London_Borough_Council" title="Islington London Borough Council">Islington London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Islington_Town_Hall" title="Islington Town Hall">Customer Centre</a>, 222 Upper Street\n</td>\n<td>5.74\n</td>\n<td>215,667\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.5416_N_0.1022_W_region:GB_type:city&amp;title=Islington"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb032\xe2\x80\xb230\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb006\xe2\x80\xb208\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5416\xc2\xb0N 0.1022\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5416; -0.1022</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Islington</span>)</span></span></span></a></span>\n</td>\n<td>10\n</td></tr>\n<tr>\n<td><a href="/wiki/Royal_Borough_of_Kensington_and_Chelsea" title="Royal Borough of Kensington and Chelsea">Kensington and Chelsea</a>\n</td>\n<td><img alt="\xe2\x98\x91" data-file-height="600" data-file-width="600" decoding="async" height="20" src="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/20px-Yes_check.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/30px-Yes_check.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/40px-Yes_check.svg.png 2x" width="20"/><span style="display:none">Y</span>\n</td>\n<td><a class="mw-redirect" href="/wiki/Royal_borough" title="Royal borough">Royal</a>\n</td>\n<td><a href="/wiki/Kensington_and_Chelsea_London_Borough_Council" title="Kensington and Chelsea London Borough Council">Kensington and Chelsea London Borough Council</a>\n</td>\n<td><a href="/wiki/Conservative_Party_(UK)" title="Conservative Party (UK)">Conservative</a>\n</td>\n<td><a href="/wiki/Kensington_Town_Hall,_London" title="Kensington Town Hall, London">The Town Hall</a>, <a href="/wiki/Hornton_Street" title="Hornton Street">Hornton Street</a>\n</td>\n<td>4.68\n</td>\n<td>155,594\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.502_N_0.1947_W_region:GB_type:city&amp;title=Kensington+and+Chelsea"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb030\xe2\x80\xb207\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb011\xe2\x80\xb241\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5020\xc2\xb0N 0.1947\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5020; -0.1947</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Kensington and Chelsea</span>)</span></span></span></a></span>\n</td>\n<td>3\n</td></tr>\n<tr>\n<td><a href="/wiki/Royal_Borough_of_Kingston_upon_Thames" title="Royal Borough of Kingston upon Thames">Kingston upon Thames</a>\n</td>\n<td>\n</td>\n<td><a class="mw-redirect" href="/wiki/Royal_borough" title="Royal borough">Royal</a>\n</td>\n<td><a href="/wiki/Kingston_upon_Thames_London_Borough_Council" title="Kingston upon Thames London Borough Council">Kingston upon Thames London Borough Council</a>\n</td>\n<td><a href="/wiki/Liberal_Democrats_(UK)" title="Liberal Democrats (UK)">Liberal\xc2\xa0Democrat</a>\n</td>\n<td><a href="/wiki/Kingston_upon_Thames_Guildhall" title="Kingston upon Thames Guildhall">Guildhall</a>, High Street\n</td>\n<td>14.38\n</td>\n<td>166,793\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.4085_N_0.3064_W_region:GB_type:city&amp;title=Kingston+upon+Thames"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb024\xe2\x80\xb231\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb018\xe2\x80\xb223\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.4085\xc2\xb0N 0.3064\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.4085; -0.3064</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Kingston upon Thames</span>)</span></span></span></a></span>\n</td>\n<td>16\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Lambeth" title="London Borough of Lambeth">Lambeth</a>\n</td>\n<td><img alt="\xe2\x98\x91" data-file-height="600" data-file-width="600" decoding="async" height="20" src="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/20px-Yes_check.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/30px-Yes_check.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/40px-Yes_check.svg.png 2x" width="20"/><span style="display:none">Y</span>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Lambeth_London_Borough_Council" title="Lambeth London Borough Council">Lambeth London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Lambeth_Town_Hall" title="Lambeth Town Hall">Lambeth Town Hall</a>, Brixton Hill\n</td>\n<td>10.36\n</td>\n<td>314,242\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.4607_N_0.1163_W_region:GB_type:city&amp;title=Lambeth"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb027\xe2\x80\xb239\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb006\xe2\x80\xb259\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.4607\xc2\xb0N 0.1163\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.4607; -0.1163</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Lambeth</span>)</span></span></span></a></span>\n</td>\n<td>6\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Lewisham" title="London Borough of Lewisham">Lewisham</a>\n</td>\n<td><img alt="\xe2\x98\x91" data-file-height="600" data-file-width="600" decoding="async" height="20" src="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/20px-Yes_check.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/30px-Yes_check.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/40px-Yes_check.svg.png 2x" width="20"/><span style="display:none">Y</span>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Lewisham_London_Borough_Council" title="Lewisham London Borough Council">Lewisham London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Lewisham_Town_Hall" title="Lewisham Town Hall">Town Hall</a>, 1 Catford Road\n</td>\n<td>13.57\n</td>\n<td>286,180\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.4452_N_0.0209_W_region:GB_type:city&amp;title=Lewisham"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb026\xe2\x80\xb243\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb001\xe2\x80\xb215\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.4452\xc2\xb0N 0.0209\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.4452; -0.0209</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Lewisham</span>)</span></span></span></a></span>\n</td>\n<td>21\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Merton" title="London Borough of Merton">Merton</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Merton_London_Borough_Council" title="Merton London Borough Council">Merton London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Merton_Civic_Centre" title="Merton Civic Centre">Civic Centre</a>, London Road\n</td>\n<td>14.52\n</td>\n<td>203,223\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.4014_N_0.1958_W_region:GB_type:city&amp;title=Merton"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb024\xe2\x80\xb205\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb011\xe2\x80\xb245\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.4014\xc2\xb0N 0.1958\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.4014; -0.1958</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Merton</span>)</span></span></span></a></span>\n</td>\n<td>17\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Newham" title="London Borough of Newham">Newham</a>\n</td>\n<td><sup class="reference" id="cite_ref-note2_4-2"><a href="#cite_note-note2-4">[note 3]</a></sup>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Newham_London_Borough_Council" title="Newham London Borough Council">Newham London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Newham_Town_Hall#Newham_Dockside" title="Newham Town Hall">Newham Dockside</a>, 1000 Dockside Road\n</td>\n<td>13.98\n</td>\n<td>318,227\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.5077_N_0.0469_E_region:GB_type:city&amp;title=Newham"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb030\xe2\x80\xb228\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb002\xe2\x80\xb249\xe2\x80\xb3E</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5077\xc2\xb0N 0.0469\xc2\xb0E</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5077; 0.0469</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Newham</span>)</span></span></span></a></span>\n</td>\n<td>27\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Redbridge" title="London Borough of Redbridge">Redbridge</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Redbridge_London_Borough_Council" title="Redbridge London Borough Council">Redbridge London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Redbridge_Town_Hall" title="Redbridge Town Hall">Town Hall</a>, 128-142 High Road\n</td>\n<td>21.78\n</td>\n<td>288,272\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.559_N_0.0741_E_region:GB_type:city&amp;title=Redbridge"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb033\xe2\x80\xb232\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb004\xe2\x80\xb227\xe2\x80\xb3E</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5590\xc2\xb0N 0.0741\xc2\xb0E</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5590; 0.0741</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Redbridge</span>)</span></span></span></a></span>\n</td>\n<td>26\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Richmond_upon_Thames" title="London Borough of Richmond upon Thames">Richmond upon Thames</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Richmond_upon_Thames_London_Borough_Council" title="Richmond upon Thames London Borough Council">Richmond upon Thames London Borough Council</a>\n</td>\n<td><a href="/wiki/Liberal_Democrats_(UK)" title="Liberal Democrats (UK)">Liberal\xc2\xa0Democrat</a>\n</td>\n<td><a href="/wiki/York_House,_Twickenham#London_Borough_of_Richmond_upon_Thames" title="York House, Twickenham">Civic Centre</a>, 44 York Street\n</td>\n<td>22.17\n</td>\n<td>191,365\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.4479_N_0.326_W_region:GB_type:city&amp;title=Richmond+upon+Thames"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb026\xe2\x80\xb252\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb019\xe2\x80\xb234\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.4479\xc2\xb0N 0.3260\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.4479; -0.3260</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Richmond upon Thames</span>)</span></span></span></a></span>\n</td>\n<td>15\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Southwark" title="London Borough of Southwark">Southwark</a>\n</td>\n<td><img alt="\xe2\x98\x91" data-file-height="600" data-file-width="600" decoding="async" height="20" src="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/20px-Yes_check.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/30px-Yes_check.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/40px-Yes_check.svg.png 2x" width="20"/><span style="display:none">Y</span>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Southwark_London_Borough_Council" title="Southwark London Borough Council">Southwark London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/160_Tooley_Street" title="160 Tooley Street">160 Tooley Street</a>\n</td>\n<td>11.14\n</td>\n<td>298,464\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.5035_N_0.0804_W_region:GB_type:city&amp;title=Southwark"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb030\xe2\x80\xb213\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb004\xe2\x80\xb249\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5035\xc2\xb0N 0.0804\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5035; -0.0804</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Southwark</span>)</span></span></span></a></span>\n</td>\n<td>7\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Sutton" title="London Borough of Sutton">Sutton</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Sutton_London_Borough_Council" title="Sutton London Borough Council">Sutton London Borough Council</a>\n</td>\n<td><a href="/wiki/Liberal_Democrats_(UK)" title="Liberal Democrats (UK)">Liberal\xc2\xa0Democrat</a>\n</td>\n<td><a href="/wiki/Sutton_Civic_Offices" title="Sutton Civic Offices">Civic Offices</a>, St Nicholas Way\n</td>\n<td>16.93\n</td>\n<td>195,914\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.3618_N_0.1945_W_region:GB_type:city&amp;title=Sutton"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb021\xe2\x80\xb242\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb011\xe2\x80\xb240\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.3618\xc2\xb0N 0.1945\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.3618; -0.1945</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Sutton</span>)</span></span></span></a></span>\n</td>\n<td>18\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Tower_Hamlets" title="London Borough of Tower Hamlets">Tower Hamlets</a>\n</td>\n<td><img alt="\xe2\x98\x91" data-file-height="600" data-file-width="600" decoding="async" height="20" src="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/20px-Yes_check.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/30px-Yes_check.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/40px-Yes_check.svg.png 2x" width="20"/><span style="display:none">Y</span>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Tower_Hamlets_London_Borough_Council" title="Tower Hamlets London Borough Council">Tower Hamlets London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Tower_Hamlets_Town_Hall" title="Tower Hamlets Town Hall">Town Hall</a>, Mulberry Place, 5 Clove Crescent\n</td>\n<td>7.63\n</td>\n<td>272,890\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.5099_N_0.0059_W_region:GB_type:city&amp;title=Tower+Hamlets"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb030\xe2\x80\xb236\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb000\xe2\x80\xb221\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5099\xc2\xb0N 0.0059\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5099; -0.0059</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Tower Hamlets</span>)</span></span></span></a></span>\n</td>\n<td>8\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Waltham_Forest" title="London Borough of Waltham Forest">Waltham Forest</a>\n</td>\n<td>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Waltham_Forest_London_Borough_Council" title="Waltham Forest London Borough Council">Waltham Forest London Borough Council</a>\n</td>\n<td><a href="/wiki/Labour_Party_(UK)" title="Labour Party (UK)">Labour</a>\n</td>\n<td><a href="/wiki/Waltham_Forest_Town_Hall" title="Waltham Forest Town Hall">Waltham Forest Town Hall</a>, Forest Road\n</td>\n<td>14.99\n</td>\n<td>265,797\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.5908_N_0.0134_W_region:GB_type:city&amp;title=Waltham+Forest"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb035\xe2\x80\xb227\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb000\xe2\x80\xb248\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5908\xc2\xb0N 0.0134\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5908; -0.0134</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Waltham Forest</span>)</span></span></span></a></span>\n</td>\n<td>28\n</td></tr>\n<tr>\n<td><a href="/wiki/London_Borough_of_Wandsworth" title="London Borough of Wandsworth">Wandsworth</a>\n</td>\n<td><img alt="\xe2\x98\x91" data-file-height="600" data-file-width="600" decoding="async" height="20" src="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/20px-Yes_check.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/30px-Yes_check.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/40px-Yes_check.svg.png 2x" width="20"/><span style="display:none">Y</span>\n</td>\n<td>\n</td>\n<td><a href="/wiki/Wandsworth_London_Borough_Council" title="Wandsworth London Borough Council">Wandsworth London Borough Council</a>\n</td>\n<td><a href="/wiki/Conservative_Party_(UK)" title="Conservative Party (UK)">Conservative</a>\n</td>\n<td><a href="/wiki/Wandsworth_Town_Hall" title="Wandsworth Town Hall">The Town Hall</a>, <a href="/wiki/Wandsworth_High_Street" title="Wandsworth High Street">Wandsworth High Street</a>\n</td>\n<td>13.23\n</td>\n<td>310,516\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.4567_N_0.191_W_region:GB_type:city&amp;title=Wandsworth"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb027\xe2\x80\xb224\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb011\xe2\x80\xb228\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.4567\xc2\xb0N 0.1910\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.4567; -0.1910</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Wandsworth</span>)</span></span></span></a></span>\n</td>\n<td>5\n</td></tr>\n<tr>\n<td><a href="/wiki/City_of_Westminster" title="City of Westminster">Westminster</a>\n</td>\n<td><img alt="\xe2\x98\x91" data-file-height="600" data-file-width="600" decoding="async" height="20" src="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/20px-Yes_check.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/30px-Yes_check.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/40px-Yes_check.svg.png 2x" width="20"/><span style="display:none">Y</span>\n</td>\n<td><a href="/wiki/City_status_in_the_United_Kingdom" title="City status in the United Kingdom">City</a>\n</td>\n<td><a href="/wiki/Westminster_City_Council" title="Westminster City Council">Westminster City Council</a>\n</td>\n<td><a href="/wiki/Conservative_Party_(UK)" title="Conservative Party (UK)">Conservative</a>\n</td>\n<td><a href="/wiki/Westminster_City_Hall" title="Westminster City Hall">Westminster City Hall</a>, 64 Victoria Street\n</td>\n<td>8.29\n</td>\n<td>226,841\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.4973_N_0.1372_W_region:GB_type:city&amp;title=Westminster"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb029\xe2\x80\xb250\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb008\xe2\x80\xb214\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.4973\xc2\xb0N 0.1372\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.4973; -0.1372</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">Westminster</span>)</span></span></span></a></span>\n</td>\n<td>2\n</td></tr></tbody></table>\n<h2><span class="mw-headline" id="City_of_London">City of London</span><span class="mw-editsection"><span class="mw-editsection-bracket">[</span><a href="/w/index.php?title=List_of_London_boroughs&amp;action=edit&amp;section=2" title="Edit section: City of London">edit</a><span class="mw-editsection-bracket">]</span></span></h2>\n<p>The <a href="/wiki/City_of_London" title="City of London">City of London</a> is the 33rd principal division of Greater London but it is not a London borough.\n</p>\n<table class="wikitable sortable" style="font-size:95%" width="100%">\n<tbody><tr>\n<th width="100px">Borough\n</th>\n<th>Inner\n</th>\n<th width="100px">Status\n</th>\n<th>Local authority\n</th>\n<th>Political control\n</th>\n<th width="120px">Headquarters\n</th>\n<th>Area (sq mi)\n</th>\n<th>Population<br/>(2011 est)\n</th>\n<th width="20px">Co-ordinates\n</th>\n<th><span style="background:#67BCD3"> Nr. in<br/>map </span>\n</th></tr>\n<tr>\n<td><a href="/wiki/City_of_London" title="City of London">City of London</a>\n</td>\n<td>(<img alt="\xe2\x98\x91" data-file-height="600" data-file-width="600" decoding="async" height="20" src="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/20px-Yes_check.svg.png" srcset="//upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/30px-Yes_check.svg.png 1.5x, //upload.wikimedia.org/wikipedia/en/thumb/f/fb/Yes_check.svg/40px-Yes_check.svg.png 2x" width="20"/><span style="display:none">Y</span>)<br/><sup class="reference" id="cite_ref-6"><a href="#cite_note-6">[note 5]</a></sup>\n</td>\n<td><i><a href="/wiki/Sui_generis" title="Sui generis">Sui generis</a></i>;<br/><a href="/wiki/City_status_in_the_United_Kingdom" title="City status in the United Kingdom">City</a>;<br/><a href="/wiki/Ceremonial_counties_of_England" title="Ceremonial counties of England">Ceremonial county</a>\n</td>\n<td><a class="mw-redirect" href="/wiki/Corporation_of_London" title="Corporation of London">Corporation of London</a>;<br/><a href="/wiki/Inner_Temple" title="Inner Temple">Inner Temple</a>;<br/><a href="/wiki/Middle_Temple" title="Middle Temple">Middle Temple</a>\n</td>\n<td>?\xc2\xa0\n</td>\n<td><a href="/wiki/Guildhall,_London" title="Guildhall, London">Guildhall</a>\n</td>\n<td>1.12\n</td>\n<td>7,000\n</td>\n<td><span class="plainlinks nourlexpansion"><a class="external text" href="//tools.wmflabs.org/geohack/geohack.php?pagename=List_of_London_boroughs&amp;params=51.5155_N_0.0922_W_region:GB_type:city&amp;title=City+of+London"><span class="geo-nondefault"><span class="geo-dms" title="Maps, aerial photos, and other data for this location"><span class="latitude">51\xc2\xb030\xe2\x80\xb256\xe2\x80\xb3N</span> <span class="longitude">0\xc2\xb005\xe2\x80\xb232\xe2\x80\xb3W</span></span></span><span class="geo-multi-punct">\xef\xbb\xbf / \xef\xbb\xbf</span><span class="geo-default"><span class="vcard"><span class="geo-dec" title="Maps, aerial photos, and other data for this location">51.5155\xc2\xb0N 0.0922\xc2\xb0W</span><span style="display:none">\xef\xbb\xbf / <span class="geo">51.5155; -0.0922</span></span><span style="display:none">\xef\xbb\xbf (<span class="fn org">City of London</span>)</span></span></span></a></span>\n</td>\n<td>1\n</td></tr></tbody></table>\n<table class="noprint infobox" id="GeoGroup" style="width: 23em; font-size: 88%; line-height: 1.5em">\n<tbody><tr>\n<td><b>Map all coordinates using:</b> <a class="external text" href="//tools.wmflabs.org/osm4wiki/cgi-bin/wiki/wiki-osm.pl?project=en&amp;article=List_of_London_boroughs">OpenStreetMap</a>\xc2\xa0\n</td></tr>\n<tr>\n<td><b>Download coordinates as:</b> <a class="external text" href="//tools.wmflabs.org/kmlexport?article=List_of_London_boroughs">KML</a>\xc2\xa0<b>\xc2\xb7</b>\xc2\xa0<a class="external text" href="http://tripgang.com/kml2gpx/http%3A%2F%2Ftools.wmflabs.org%2Fkmlexport%3Farticle%3DList_of_London_boroughs?gpx=1" rel="nofollow">GPX</a>\n</td></tr></tbody></table>\n<h2><span class="mw-headline" id="See_also">See also</span><span class="mw-editsection"><span class="mw-editsection-bracket">[</span><a href="/w/index.php?title=List_of_London_boroughs&amp;action=edit&amp;section=3" title="Edit section: See also">edit</a><span class="mw-editsection-bracket">]</span></span></h2>\n<ul><li><a class="mw-redirect" href="/wiki/Political_make-up_of_London_borough_councils" title="Political make-up of London borough councils">Political make-up of London borough councils</a></li>\n<li><a href="/wiki/List_of_areas_of_London" title="List of areas of London">List of areas of London</a></li>\n<li><a href="/wiki/Subdivisions_of_England" title="Subdivisions of England">Subdivisions of England</a></li></ul>\n<h2><span class="mw-headline" id="Notes">Notes</span><span class="mw-editsection"><span class="mw-editsection-bracket">[</span><a href="/w/index.php?title=List_of_London_boroughs&amp;action=edit&amp;section=4" title="Edit section: Notes">edit</a><span class="mw-editsection-bracket">]</span></span></h2>\n<div class="reflist" style="list-style-type: decimal;">\n<div class="mw-references-wrap"><ol class="references">\n<li id="cite_note-2"><span class="mw-cite-backlink"><b><a href="#cite_ref-2">^</a></b></span> <span class="reference-text">Renamed from London Borough of Barking 1 January 1980. <cite class="citation magazine" id="CITEREFGazette48021"><a class="external text" href="https://www.thegazette.co.uk/London/issue/48021/page/15280" rel="nofollow">"No. 48021"</a>. <i><a href="/wiki/The_London_Gazette" title="The London Gazette">The London Gazette</a></i>. 4 December 1979. p.\xc2\xa015280.</cite><span class="Z3988" title="ctx_ver=Z39.88-2004&amp;rft_val_fmt=info%3Aofi%2Ffmt%3Akev%3Amtx%3Ajournal&amp;rft.genre=article&amp;rft.jtitle=The+London+Gazette&amp;rft.atitle=No.+48021&amp;rft.pages=15280&amp;rft.date=1979-12-04&amp;rft_id=https%3A%2F%2Fwww.thegazette.co.uk%2FLondon%2Fissue%2F48021%2Fpage%2F15280&amp;rfr_id=info%3Asid%2Fen.wikipedia.org%3AList+of+London+boroughs"></span><style data-mw-deduplicate="TemplateStyles:r951705291">.mw-parser-output cite.citation{font-style:inherit}.mw-parser-output .citation q{quotes:"\\"""\\"""\'""\'"}.mw-parser-output .id-lock-free a,.mw-parser-output .citation .cs1-lock-free a{background-image:url("//upload.wikimedia.org/wikipedia/commons/thumb/6/65/Lock-green.svg/9px-Lock-green.svg.png");background-image:linear-gradient(transparent,transparent),url("//upload.wikimedia.org/wikipedia/commons/6/65/Lock-green.svg");background-repeat:no-repeat;background-size:9px;background-position:right .1em center}.mw-parser-output .id-lock-limited a,.mw-parser-output .id-lock-registration a,.mw-parser-output .citation .cs1-lock-limited a,.mw-parser-output .citation .cs1-lock-registration a{background-image:url("//upload.wikimedia.org/wikipedia/commons/thumb/d/d6/Lock-gray-alt-2.svg/9px-Lock-gray-alt-2.svg.png");background-image:linear-gradient(transparent,transparent),url("//upload.wikimedia.org/wikipedia/commons/d/d6/Lock-gray-alt-2.svg");background-repeat:no-repeat;background-size:9px;background-position:right .1em center}.mw-parser-output .id-lock-subscription a,.mw-parser-output .citation .cs1-lock-subscription a{background-image:url("//upload.wikimedia.org/wikipedia/commons/thumb/a/aa/Lock-red-alt-2.svg/9px-Lock-red-alt-2.svg.png");background-image:linear-gradient(transparent,transparent),url("//upload.wikimedia.org/wikipedia/commons/a/aa/Lock-red-alt-2.svg");background-repeat:no-repeat;background-size:9px;background-position:right .1em center}.mw-parser-output .cs1-subscription,.mw-parser-output .cs1-registration{color:#555}.mw-parser-output .cs1-subscription span,.mw-parser-output .cs1-registration span{border-bottom:1px dotted;cursor:help}.mw-parser-output .cs1-ws-icon a{background-image:url("//upload.wikimedia.org/wikipedia/commons/thumb/4/4c/Wikisource-logo.svg/12px-Wikisource-logo.svg.png");background-image:linear-gradient(transparent,transparent),url("//upload.wikimedia.org/wikipedia/commons/4/4c/Wikisource-logo.svg");background-repeat:no-repeat;background-size:12px;background-position:right .1em center}.mw-parser-output code.cs1-code{color:inherit;background:inherit;border:inherit;padding:inherit}.mw-parser-output .cs1-hidden-error{display:none;font-size:100%}.mw-parser-output .cs1-visible-error{font-size:100%}.mw-parser-output .cs1-maint{display:none;color:#33aa33;margin-left:0.3em}.mw-parser-output .cs1-subscription,.mw-parser-output .cs1-registration,.mw-parser-output .cs1-format{font-size:95%}.mw-parser-output .cs1-kern-left,.mw-parser-output .cs1-kern-wl-left{padding-left:0.2em}.mw-parser-output .cs1-kern-right,.mw-parser-output .cs1-kern-wl-right{padding-right:0.2em}.mw-parser-output .citation .mw-selflink{font-weight:inherit}</style></span>\n</li>\n<li id="cite_note-3"><span class="mw-cite-backlink"><b><a href="#cite_ref-3">^</a></b></span> <span class="reference-text">Royal borough from 2012</span>\n</li>\n<li id="cite_note-note2-4"><span class="mw-cite-backlink">^ <a href="#cite_ref-note2_4-0"><sup><i><b>a</b></i></sup></a> <a href="#cite_ref-note2_4-1"><sup><i><b>b</b></i></sup></a> <a href="#cite_ref-note2_4-2"><sup><i><b>c</b></i></sup></a></span> <span class="reference-text">Haringey and Newham are Inner London for statistics; Greenwich is Outer London for statistics</span>\n</li>\n<li id="cite_note-5"><span class="mw-cite-backlink"><b><a href="#cite_ref-5">^</a></b></span> <span class="reference-text">Renamed from London Borough of Hammersmith 1 April 1979. <cite class="citation magazine" id="CITEREFGazette47771"><a class="external text" href="https://www.thegazette.co.uk/London/issue/47771/page/2095" rel="nofollow">"No. 47771"</a>. <i><a href="/wiki/The_London_Gazette" title="The London Gazette">The London Gazette</a></i>. 13 February 1979. p.\xc2\xa02095.</cite><span class="Z3988" title="ctx_ver=Z39.88-2004&amp;rft_val_fmt=info%3Aofi%2Ffmt%3Akev%3Amtx%3Ajournal&amp;rft.genre=article&amp;rft.jtitle=The+London+Gazette&amp;rft.atitle=No.+47771&amp;rft.pages=2095&amp;rft.date=1979-02-13&amp;rft_id=https%3A%2F%2Fwww.thegazette.co.uk%2FLondon%2Fissue%2F47771%2Fpage%2F2095&amp;rfr_id=info%3Asid%2Fen.wikipedia.org%3AList+of+London+boroughs"></span><link href="mw-data:TemplateStyles:r951705291" rel="mw-deduplicated-inline-style"/></span>\n</li>\n<li id="cite_note-6"><span class="mw-cite-backlink"><b><a href="#cite_ref-6">^</a></b></span> <span class="reference-text">The City of London was not part of the <a href="/wiki/County_of_London" title="County of London">County of London</a> and is not a London Borough but can be counted to <a href="/wiki/Inner_London" title="Inner London">Inner London</a>.</span>\n</li>\n</ol></div></div>\n<h2><span class="mw-headline" id="References">References</span><span class="mw-editsection"><span class="mw-editsection-bracket">[</span><a href="/w/index.php?title=List_of_London_boroughs&amp;action=edit&amp;section=5" title="Edit section: References">edit</a><span class="mw-editsection-bracket">]</span></span></h2>\n<div class="reflist" style="list-style-type: decimal;">\n<div class="mw-references-wrap"><ol class="references">\n<li id="cite_note-1"><span class="mw-cite-backlink"><b><a href="#cite_ref-1">^</a></b></span> <span class="reference-text"><cite class="citation web" id="CITEREFONS2010">ONS (2 July 2010). <a class="external text" href="https://webarchive.nationalarchives.gov.uk/20160107070948/http://www.ons.gov.uk/ons/publications/re-reference-tables.html" rel="nofollow">"Release Edition Reference Tables"</a>. <i>Webarchive.nationalarchives.gov.uk</i>. Archived from <a class="external text" href="http://www.ons.gov.uk/ons/publications/re-reference-tables.html" rel="nofollow">the original</a> on 7 January 2016<span class="reference-accessdate">. Retrieved <span class="nowrap">5 February</span> 2019</span>.</cite><span class="Z3988" title="ctx_ver=Z39.88-2004&amp;rft_val_fmt=info%3Aofi%2Ffmt%3Akev%3Amtx%3Ajournal&amp;rft.genre=unknown&amp;rft.jtitle=Webarchive.nationalarchives.gov.uk&amp;rft.atitle=Release+Edition+Reference+Tables&amp;rft.date=2010-07-02&amp;rft.au=ONS&amp;rft_id=http%3A%2F%2Fwww.ons.gov.uk%2Fons%2Fpublications%2Fre-reference-tables.html&amp;rfr_id=info%3Asid%2Fen.wikipedia.org%3AList+of+London+boroughs"></span><link href="mw-data:TemplateStyles:r951705291" rel="mw-deduplicated-inline-style"/></span>\n</li>\n</ol></div></div>\n<h2><span class="mw-headline" id="External_links">External links</span><span class="mw-editsection"><span class="mw-editsection-bracket">[</span><a href="/w/index.php?title=List_of_London_boroughs&amp;action=edit&amp;section=6" title="Edit section: External links">edit</a><span class="mw-editsection-bracket">]</span></span></h2>\n<ul><li><a class="external text" href="https://web.archive.org/web/20101010011530/http://londoncouncils.gov.uk/londonlocalgovernment/londonboroughs.htm" rel="nofollow">London Councils: List of inner/outer London boroughs</a></li>\n<li><a class="external text" href="http://londonboroughsmap.co.uk/" rel="nofollow">London Boroughs Map</a></li></ul>\n<div aria-labelledby="Governance_of_Greater_London" class="navbox" role="navigation" style="padding:3px"><table class="nowraplinks hlist mw-collapsible mw-collapsed navbox-inner" style="border-spacing:0;background:transparent;color:inherit"><tbody><tr><th class="navbox-title" colspan="2" scope="col"><div class="plainlinks hlist navbar mini"><ul><li class="nv-view"><a href="/wiki/Template:Governance_of_Greater_London" title="Template:Governance of Greater London"><abbr style=";;background:none transparent;border:none;-moz-box-shadow:none;-webkit-box-shadow:none;box-shadow:none; padding:0;" title="View this template">v</abbr></a></li><li class="nv-talk"><a href="/wiki/Template_talk:Governance_of_Greater_London" title="Template talk:Governance of Greater London"><abbr style=";;background:none transparent;border:none;-moz-box-shadow:none;-webkit-box-shadow:none;box-shadow:none; padding:0;" title="Discuss this template">t</abbr></a></li><li class="nv-edit"><a class="external text" href="https://en.wikipedia.org/w/index.php?title=Template:Governance_of_Greater_London&amp;action=edit"><abbr style=";;background:none transparent;border:none;-moz-box-shadow:none;-webkit-box-shadow:none;box-shadow:none; padding:0;" title="Edit this template">e</abbr></a></li></ul></div><div id="Governance_of_Greater_London" style="font-size:114%;margin:0 4em">Governance of <a href="/wiki/Greater_London" title="Greater London">Greater London</a></div></th></tr><tr><td class="navbox-abovebelow" colspan="2"><div id="*_City_of_London_*_Greater_London_*_London">\n<ul><li><a href="/wiki/City_of_London" title="City of London">City of London</a></li>\n<li><a href="/wiki/Greater_London" title="Greater London">Greater London</a></li>\n<li><a href="/wiki/London" title="London">London</a></li></ul>\n</div></td></tr><tr><th class="navbox-group" scope="row" style="width:1%">Regional</th><td class="navbox-list navbox-odd" style="text-align:left;border-left-width:2px;border-left-style:solid;width:100%;padding:0px"><div style="padding:0em 0.25em">\n<ul><li><b><a href="/wiki/Greater_London_Authority" title="Greater London Authority">Greater London Authority</a>: </b><a href="/wiki/London_Assembly" title="London Assembly">London Assembly</a></li>\n<li><a href="/wiki/Mayor_of_London" title="Mayor of London">Mayor of London</a></li></ul>\n</div></td></tr><tr><th class="navbox-group" scope="row" style="width:1%"><a href="/wiki/London_boroughs" title="London boroughs">Boroughs</a><br/>(<a class="mw-selflink selflink">list</a>)</th><td class="navbox-list navbox-even" style="text-align:left;border-left-width:2px;border-left-style:solid;width:100%;padding:0px"><div style="padding:0em 0.25em">\n<ul><li><b><a href="/wiki/London_Councils" title="London Councils">London Councils</a>: </b><a href="/wiki/London_Borough_of_Barking_and_Dagenham" title="London Borough of Barking and Dagenham">Barking and Dagenham</a></li>\n<li><a href="/wiki/London_Borough_of_Barnet" title="London Borough of Barnet">Barnet</a></li>\n<li><a href="/wiki/London_Borough_of_Bexley" title="London Borough of Bexley">Bexley</a></li>\n<li><a href="/wiki/London_Borough_of_Brent" title="London Borough of Brent">Brent</a></li>\n<li><a href="/wiki/London_Borough_of_Bromley" title="London Borough of Bromley">Bromley</a></li>\n<li><a href="/wiki/London_Borough_of_Camden" title="London Borough of Camden">Camden</a></li>\n<li><a href="/wiki/London_Borough_of_Croydon" title="London Borough of Croydon">Croydon</a></li>\n<li><a href="/wiki/London_Borough_of_Ealing" title="London Borough of Ealing">Ealing</a></li>\n<li><a href="/wiki/London_Borough_of_Enfield" title="London Borough of Enfield">Enfield</a></li>\n<li><a href="/wiki/Royal_Borough_of_Greenwich" title="Royal Borough of Greenwich">Greenwich</a></li>\n<li><a href="/wiki/London_Borough_of_Hackney" title="London Borough of Hackney">Hackney</a></li>\n<li><a href="/wiki/London_Borough_of_Hammersmith_and_Fulham" title="London Borough of Hammersmith and Fulham">Hammersmith and Fulham</a></li>\n<li><a href="/wiki/London_Borough_of_Haringey" title="London Borough of Haringey">Haringey</a></li>\n<li><a href="/wiki/London_Borough_of_Harrow" title="London Borough of Harrow">Harrow</a></li>\n<li><a href="/wiki/London_Borough_of_Havering" title="London Borough of Havering">Havering</a></li>\n<li><a href="/wiki/London_Borough_of_Hillingdon" title="London Borough of Hillingdon">Hillingdon</a></li>\n<li><a href="/wiki/London_Borough_of_Hounslow" title="London Borough of Hounslow">Hounslow</a></li>\n<li><a href="/wiki/London_Borough_of_Islington" title="London Borough of Islington">Islington</a></li>\n<li><a href="/wiki/Royal_Borough_of_Kensington_and_Chelsea" title="Royal Borough of Kensington and Chelsea">Kensington and Chelsea</a></li>\n<li><a href="/wiki/Royal_Borough_of_Kingston_upon_Thames" title="Royal Borough of Kingston upon Thames">Kingston upon Thames</a></li>\n<li><a href="/wiki/London_Borough_of_Lambeth" title="London Borough of Lambeth">Lambeth</a></li>\n<li><a href="/wiki/London_Borough_of_Lewisham" title="London Borough of Lewisham">Lewisham</a></li>\n<li><a href="/wiki/London_Borough_of_Merton" title="London Borough of Merton">Merton</a></li>\n<li><a href="/wiki/London_Borough_of_Newham" title="London Borough of Newham">Newham</a></li>\n<li><a href="/wiki/London_Borough_of_Redbridge" title="London Borough of Redbridge">Redbridge</a></li>\n<li><a href="/wiki/London_Borough_of_Richmond_upon_Thames" title="London Borough of Richmond upon Thames">Richmond upon Thames</a></li>\n<li><a href="/wiki/London_Borough_of_Southwark" title="London Borough of Southwark">Southwark</a></li>\n<li><a href="/wiki/London_Borough_of_Sutton" title="London Borough of Sutton">Sutton</a></li>\n<li><a href="/wiki/London_Borough_of_Tower_Hamlets" title="London Borough of Tower Hamlets">Tower Hamlets</a></li>\n<li><a href="/wiki/London_Borough_of_Waltham_Forest" title="London Borough of Waltham Forest">Waltham Forest</a></li>\n<li><a href="/wiki/London_Borough_of_Wandsworth" title="London Borough of Wandsworth">Wandsworth</a></li>\n<li><a href="/wiki/City_of_Westminster" title="City of Westminster">Westminster</a></li></ul>\n</div></td></tr><tr><th class="navbox-group" scope="row" style="width:1%">Ceremonial</th><td class="navbox-list navbox-odd" style="text-align:left;border-left-width:2px;border-left-style:solid;width:100%;padding:0px"><div style="padding:0em 0.25em">\n<ul><li>City of London\n<ul><li><a href="/wiki/Lord_Mayor_of_London" title="Lord Mayor of London">Lord Mayor</a></li>\n<li><a href="/wiki/Lord_Lieutenant_of_the_City_of_London" title="Lord Lieutenant of the City of London">Lord Lieutenant</a></li>\n<li><a href="/wiki/Sheriffs_of_the_City_of_London" title="Sheriffs of the City of London">Sheriffs</a></li></ul></li>\n<li>Greater London\n<ul><li><a class="mw-redirect" href="/wiki/Lord_Lieutenant_of_Greater_London" title="Lord Lieutenant of Greater London">Lord Lieutenant</a></li>\n<li><a href="/wiki/High_Sheriff_of_Greater_London" title="High Sheriff of Greater London">High Sheriff</a></li></ul></li></ul>\n</div></td></tr><tr><th class="navbox-group" scope="row" style="width:1%"><a href="/wiki/History_of_local_government_in_London" title="History of local government in London">Historical</a></th><td class="navbox-list navbox-even" style="text-align:left;border-left-width:2px;border-left-style:solid;width:100%;padding:0px"><div style="padding:0em 0.25em">\n<ul><li><a href="/wiki/Metropolitan_Board_of_Works" title="Metropolitan Board of Works">Metropolitan Board of Works</a> <span style="font-size:85%;">(MBW) 1855\xe2\x80\x931889</span></li>\n<li><a href="/wiki/London_County_Council" title="London County Council">London County Council</a> <span style="font-size:85%;">(LCC) 1889\xe2\x80\x931965</span></li>\n<li><a href="/wiki/Greater_London_Council" title="Greater London Council">Greater London Council</a> <span style="font-size:85%;">(GLC) 1965\xe2\x80\x931986</span></li>\n<li><a href="/wiki/List_of_heads_of_London_government" title="List of heads of London government">Leaders</a></li></ul>\n</div></td></tr></tbody></table></div>\n<!-- \nNewPP limit report\nParsed by mw1352\nCached time: 20200528111313\nCache expiry: 2592000\nDynamic content: false\nComplications: [vary\xe2\x80\x90revision\xe2\x80\x90sha1]\nCPU time usage: 0.404 seconds\nReal time usage: 0.525 seconds\nPreprocessor visited node count: 5111/1000000\nPost\xe2\x80\x90expand include size: 79923/2097152 bytes\nTemplate argument size: 1057/2097152 bytes\nHighest expansion depth: 13/40\nExpensive parser function count: 2/500\nUnstrip recursion depth: 1/20\nUnstrip post\xe2\x80\x90expand size: 12959/5000000 bytes\nNumber of Wikibase entities loaded: 0/400\nLua time usage: 0.138/10.000 seconds\nLua memory usage: 3.33 MB/50 MB\n-->\n<!--\nTransclusion expansion time report (%,ms,calls,template)\n100.00%  364.638      1 -total\n 37.73%  137.589      2 Template:Reflist\n 31.28%  114.068      2 Template:London_Gazette\n 29.63%  108.045      2 Template:Cite_magazine\n 19.19%   69.968     33 Template:Coord\n 14.49%   52.833      1 Template:Use_dmy_dates\n 10.53%   38.412     33 Template:English_district_control\n  7.22%   26.315      1 Template:London\n  5.69%   20.745      2 Template:DMCA\n  4.93%   17.965      2 Template:Dated_maintenance_category\n-->\n<!-- Saved in parser cache with key enwiki:pcache:idhash:28092685-0!canonical and timestamp 20200528111313 and revision id 958873870\n -->\n</div><noscript><img alt="" height="1" src="//en.wikipedia.org/wiki/Special:CentralAutoLogin/start?type=1x1" style="border: none; position: absolute;" title="" width="1"/></noscript></div>\n<div class="printfooter">Retrieved from "<a dir="ltr" href="https://en.wikipedia.org/w/index.php?title=List_of_London_boroughs&amp;oldid=958873870">https://en.wikipedia.org/w/index.php?title=List_of_London_boroughs&amp;oldid=958873870</a>"</div>\n<div class="catlinks" data-mw="interface" id="catlinks"><div class="mw-normal-catlinks" id="mw-normal-catlinks"><a href="/wiki/Help:Category" title="Help:Category">Categories</a>: <ul><li><a href="/wiki/Category:London_boroughs" title="Category:London boroughs">London boroughs</a></li><li><a href="/wiki/Category:Lists_of_places_in_London" title="Category:Lists of places in London">Lists of places in London</a></li></ul></div><div class="mw-hidden-catlinks mw-hidden-cats-hidden" id="mw-hidden-catlinks">Hidden categories: <ul><li><a href="/wiki/Category:Use_dmy_dates_from_August_2015" title="Category:Use dmy dates from August 2015">Use dmy dates from August 2015</a></li><li><a href="/wiki/Category:Use_British_English_from_August_2015" title="Category:Use British English from August 2015">Use British English from August 2015</a></li><li><a href="/wiki/Category:Lists_of_coordinates" title="Category:Lists of coordinates">Lists of coordinates</a></li><li><a href="/wiki/Category:Geographic_coordinate_lists" title="Category:Geographic coordinate lists">Geographic coordinate lists</a></li><li><a href="/wiki/Category:Articles_with_Geo" title="Category:Articles with Geo">Articles with Geo</a></li></ul></div></div>\n<div class="visualClear"></div>\n</div>\n</div>\n<div id="mw-data-after-content">\n<div class="read-more-container"></div>\n</div>\n<div id="mw-navigation">\n<h2>Navigation menu</h2>\n<div id="mw-head">\n<div aria-labelledby="p-personal-label" class="vector-menu" id="p-personal" role="navigation">\n<h3 id="p-personal-label">Personal tools</h3>\n<ul class="menu">\n<li id="pt-anonuserpage">Not logged in</li><li id="pt-anontalk"><a accesskey="n" href="/wiki/Special:MyTalk" title="Discussion about edits from this IP address [n]">Talk</a></li><li id="pt-anoncontribs"><a accesskey="y" href="/wiki/Special:MyContributions" title="A list of edits made from this IP address [y]">Contributions</a></li><li id="pt-createaccount"><a href="/w/index.php?title=Special:CreateAccount&amp;returnto=List+of+London+boroughs" title="You are encouraged to create an account and log in; however, it is not mandatory">Create account</a></li><li id="pt-login"><a accesskey="o" href="/w/index.php?title=Special:UserLogin&amp;returnto=List+of+London+boroughs" title="You\'re encouraged to log in; however, it\'s not mandatory. [o]">Log in</a></li>\n</ul>\n</div>\n<div id="left-navigation">\n<div aria-labelledby="p-namespaces-label" class="vector-menu-tabs vectorTabs" id="p-namespaces" role="navigation">\n<h3 id="p-namespaces-label">Namespaces</h3>\n<ul class="menu">\n<li class="selected" id="ca-nstab-main"><a accesskey="c" href="/wiki/List_of_London_boroughs" title="View the content page [c]">Article</a></li><li id="ca-talk"><a accesskey="t" href="/wiki/Talk:List_of_London_boroughs" rel="discussion" title="Discuss improvements to the content page [t]">Talk</a></li>\n</ul>\n</div>\n<div aria-labelledby="p-variants-label" class="emptyPortlet vector-menu-dropdown vectorMenu" id="p-variants" role="navigation">\n<input aria-labelledby="p-variants-label" class="vectorMenuCheckbox" type="checkbox"/>\n<h3 id="p-variants-label">\n<span>Variants</span>\n</h3>\n<ul class="menu">\n</ul>\n</div>\n</div>\n<div id="right-navigation">\n<div aria-labelledby="p-views-label" class="vector-menu-tabs vectorTabs" id="p-views" role="navigation">\n<h3 id="p-views-label">Views</h3>\n<ul class="menu">\n<li class="collapsible selected" id="ca-view"><a href="/wiki/List_of_London_boroughs">Read</a></li><li class="collapsible" id="ca-edit"><a accesskey="e" href="/w/index.php?title=List_of_London_boroughs&amp;action=edit" title="Edit this page [e]">Edit</a></li><li class="collapsible" id="ca-history"><a accesskey="h" href="/w/index.php?title=List_of_London_boroughs&amp;action=history" title="Past revisions of this page [h]">View history</a></li>\n</ul>\n</div>\n<div aria-labelledby="p-cactions-label" class="emptyPortlet vector-menu-dropdown vectorMenu" id="p-cactions" role="navigation">\n<input aria-labelledby="p-cactions-label" class="vectorMenuCheckbox" type="checkbox"/>\n<h3 id="p-cactions-label">\n<span>More</span>\n</h3>\n<ul class="menu">\n</ul>\n</div>\n<div id="p-search" role="search">\n<h3>\n<label for="searchInput">Search</label>\n</h3>\n<form action="/w/index.php" id="searchform">\n<div id="simpleSearch">\n<input accesskey="f" id="searchInput" name="search" placeholder="Search Wikipedia" title="Search Wikipedia [f]" type="search"/>\n<input name="title" type="hidden" value="Special:Search"/>\n<input class="searchButton mw-fallbackSearchButton" id="mw-searchButton" name="fulltext" title="Search Wikipedia for this text" type="submit" value="Search"/>\n<input class="searchButton" id="searchButton" name="go" title="Go to a page with this exact name if it exists" type="submit" value="Go"/>\n</div>\n</form>\n</div>\n</div>\n</div>\n<div id="mw-panel">\n<div id="p-logo" role="banner">\n<a class="mw-wiki-logo" href="/wiki/Main_Page" title="Visit the main page"></a>\n</div>\n<div aria-labelledby="p-navigation-label" class="vector-menu-portal portal portal-first" id="p-navigation" role="navigation">\n<h3 id="p-navigation-label">Navigation</h3>\n<div class="body">\n<ul><li id="n-mainpage-description"><a accesskey="z" href="/wiki/Main_Page" title="Visit the main page [z]">Main page</a></li><li id="n-contents"><a href="/wiki/Wikipedia:Contents" title="Guides to browsing Wikipedia">Contents</a></li><li id="n-featuredcontent"><a href="/wiki/Wikipedia:Featured_content" title="Featured content \xe2\x80\x93 the best of Wikipedia">Featured content</a></li><li id="n-currentevents"><a href="/wiki/Portal:Current_events" title="Find background information on current events">Current events</a></li><li id="n-randompage"><a accesskey="x" href="/wiki/Special:Random" title="Load a random article [x]">Random article</a></li><li id="n-sitesupport"><a href="https://donate.wikimedia.org/wiki/Special:FundraiserRedirector?utm_source=donate&amp;utm_medium=sidebar&amp;utm_campaign=C13_en.wikipedia.org&amp;uselang=en" title="Support us">Donate to Wikipedia</a></li><li id="n-shoplink"><a href="//shop.wikimedia.org" title="Visit the Wikipedia store">Wikipedia store</a></li></ul>\n</div>\n</div>\n<div aria-labelledby="p-interaction-label" class="vector-menu-portal portal" id="p-interaction" role="navigation">\n<h3 id="p-interaction-label">Interaction</h3>\n<div class="body">\n<ul><li id="n-help"><a href="/wiki/Help:Contents" title="Guidance on how to use and edit Wikipedia">Help</a></li><li id="n-aboutsite"><a href="/wiki/Wikipedia:About" title="Find out about Wikipedia">About Wikipedia</a></li><li id="n-portal"><a href="/wiki/Wikipedia:Community_portal" title="About the project, what you can do, where to find things">Community portal</a></li><li id="n-recentchanges"><a accesskey="r" href="/wiki/Special:RecentChanges" title="A list of recent changes in the wiki [r]">Recent changes</a></li><li id="n-contactpage"><a href="//en.wikipedia.org/wiki/Wikipedia:Contact_us" title="How to contact Wikipedia">Contact page</a></li></ul>\n</div>\n</div>\n<div aria-labelledby="p-tb-label" class="vector-menu-portal portal" id="p-tb" role="navigation">\n<h3 id="p-tb-label">Tools</h3>\n<div class="body">\n<ul><li id="t-whatlinkshere"><a accesskey="j" href="/wiki/Special:WhatLinksHere/List_of_London_boroughs" title="List of all English Wikipedia pages containing links to this page [j]">What links here</a></li><li id="t-recentchangeslinked"><a accesskey="k" href="/wiki/Special:RecentChangesLinked/List_of_London_boroughs" rel="nofollow" title="Recent changes in pages linked from this page [k]">Related changes</a></li><li id="t-upload"><a accesskey="u" href="/wiki/Wikipedia:File_Upload_Wizard" title="Upload files [u]">Upload file</a></li><li id="t-specialpages"><a accesskey="q" href="/wiki/Special:SpecialPages" title="A list of all special pages [q]">Special pages</a></li><li id="t-permalink"><a href="/w/index.php?title=List_of_London_boroughs&amp;oldid=958873870" title="Permanent link to this revision of the page">Permanent link</a></li><li id="t-info"><a href="/w/index.php?title=List_of_London_boroughs&amp;action=info" title="More information about this page">Page information</a></li><li id="t-wikibase"><a accesskey="g" href="https://www.wikidata.org/wiki/Special:EntityPage/Q6577004" title="Link to connected data repository item [g]">Wikidata item</a></li><li id="t-cite"><a href="/w/index.php?title=Special:CiteThisPage&amp;page=List_of_London_boroughs&amp;id=958873870&amp;wpFormIdentifier=titleform" title="Information on how to cite this page">Cite this page</a></li></ul>\n</div>\n</div>\n<div aria-labelledby="p-coll-print_export-label" class="vector-menu-portal portal" id="p-coll-print_export" role="navigation">\n<h3 id="p-coll-print_export-label">Print/export</h3>\n<div class="body">\n<ul><li id="coll-download-as-rl"><a href="/w/index.php?title=Special:ElectronPdf&amp;page=List+of+London+boroughs&amp;action=show-download-screen">Download as PDF</a></li><li id="t-print"><a accesskey="p" href="/w/index.php?title=List_of_London_boroughs&amp;printable=yes" title="Printable version of this page [p]">Printable version</a></li></ul>\n</div>\n</div>\n<div aria-labelledby="p-lang-label" class="vector-menu-portal portal" id="p-lang" role="navigation">\n<h3 id="p-lang-label">Languages</h3>\n<div class="body">\n<ul><li class="interlanguage-link interwiki-ru"><a class="interlanguage-link-target" href="https://ru.wikipedia.org/wiki/%D0%A1%D0%BF%D0%B8%D1%81%D0%BE%D0%BA_%D0%BB%D0%BE%D0%BD%D0%B4%D0%BE%D0%BD%D1%81%D0%BA%D0%B8%D1%85_%D0%B1%D0%BE%D1%80%D0%BE" hreflang="ru" lang="ru" title="\xd0\xa1\xd0\xbf\xd0\xb8\xd1\x81\xd0\xbe\xd0\xba \xd0\xbb\xd0\xbe\xd0\xbd\xd0\xb4\xd0\xbe\xd0\xbd\xd1\x81\xd0\xba\xd0\xb8\xd1\x85 \xd0\xb1\xd0\xbe\xd1\x80\xd0\xbe \xe2\x80\x93 Russian">\xd0\xa0\xd1\x83\xd1\x81\xd1\x81\xd0\xba\xd0\xb8\xd0\xb9</a></li></ul>\n<div class="after-portlet after-portlet-lang"><span class="wb-langlinks-edit wb-langlinks-link"><a class="wbc-editpage" href="https://www.wikidata.org/wiki/Special:EntityPage/Q6577004#sitelinks-wikipedia" title="Edit interlanguage links">Edit links</a></span></div>\n</div>\n</div>\n</div>\n</div>\n<div class="mw-footer" id="footer" role="contentinfo">\n<ul id="footer-info">\n<li id="footer-info-lastmod"> This page was last edited on 26 May 2020, at 03:30<span class="anonymous-show">\xc2\xa0(UTC)</span>.</li>\n<li id="footer-info-copyright">Text is available under the <a href="//en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License" rel="license">Creative Commons Attribution-ShareAlike License</a><a href="//creativecommons.org/licenses/by-sa/3.0/" rel="license" style="display:none;"></a>;\nadditional terms may apply.  By using this site, you agree to the <a href="//foundation.wikimedia.org/wiki/Terms_of_Use">Terms of Use</a> and <a href="//foundation.wikimedia.org/wiki/Privacy_policy">Privacy Policy</a>. Wikipedia\xc2\xae is a registered trademark of the <a href="//www.wikimediafoundation.org/">Wikimedia Foundation, Inc.</a>, a non-profit organization.</li>\n</ul>\n<ul id="footer-places">\n<li id="footer-places-privacy"><a class="extiw" href="https://foundation.wikimedia.org/wiki/Privacy_policy" title="wmf:Privacy policy">Privacy policy</a></li>\n<li id="footer-places-about"><a href="/wiki/Wikipedia:About" title="Wikipedia:About">About Wikipedia</a></li>\n<li id="footer-places-disclaimer"><a href="/wiki/Wikipedia:General_disclaimer" title="Wikipedia:General disclaimer">Disclaimers</a></li>\n<li id="footer-places-contact"><a href="//en.wikipedia.org/wiki/Wikipedia:Contact_us">Contact Wikipedia</a></li>\n<li id="footer-places-developers"><a href="https://www.mediawiki.org/wiki/Special:MyLanguage/How_to_contribute">Developers</a></li>\n<li id="footer-places-statslink"><a href="https://stats.wikimedia.org/#/en.wikipedia.org">Statistics</a></li>\n<li id="footer-places-cookiestatement"><a href="https://foundation.wikimedia.org/wiki/Cookie_statement">Cookie statement</a></li>\n<li id="footer-places-mobileview"><a class="noprint stopMobileRedirectToggle" href="//en.m.wikipedia.org/w/index.php?title=List_of_London_boroughs&amp;mobileaction=toggle_view_mobile">Mobile view</a></li>\n</ul>\n<ul class="noprint" id="footer-icons">\n<li id="footer-copyrightico"><a href="https://wikimediafoundation.org/"><img alt="Wikimedia Foundation" height="31" loading="lazy" src="/static/images/wikimedia-button.png" srcset="/static/images/wikimedia-button-1.5x.png 1.5x, /static/images/wikimedia-button-2x.png 2x" width="88"/></a></li>\n<li id="footer-poweredbyico"><a href="https://www.mediawiki.org/"><img alt="Powered by MediaWiki" height="31" src="/static/images/poweredby_mediawiki_88x31.png" srcset="/static/images/poweredby_mediawiki_132x47.png 1.5x, /static/images/poweredby_mediawiki_176x62.png 2x" width="88"/></a></li>\n</ul>\n<div style="clear: both;"></div>\n</div>\n<script>(RLQ=window.RLQ||[]).push(function(){mw.config.set({"wgPageParseReport":{"limitreport":{"cputime":"0.404","walltime":"0.525","ppvisitednodes":{"value":5111,"limit":1000000},"postexpandincludesize":{"value":79923,"limit":2097152},"templateargumentsize":{"value":1057,"limit":2097152},"expansiondepth":{"value":13,"limit":40},"expensivefunctioncount":{"value":2,"limit":500},"unstrip-depth":{"value":1,"limit":20},"unstrip-size":{"value":12959,"limit":5000000},"entityaccesscount":{"value":0,"limit":400},"timingprofile":["100.00%  364.638      1 -total"," 37.73%  137.589      2 Template:Reflist"," 31.28%  114.068      2 Template:London_Gazette"," 29.63%  108.045      2 Template:Cite_magazine"," 19.19%   69.968     33 Template:Coord"," 14.49%   52.833      1 Template:Use_dmy_dates"," 10.53%   38.412     33 Template:English_district_control","  7.22%   26.315      1 Template:London","  5.69%   20.745      2 Template:DMCA","  4.93%   17.965      2 Template:Dated_maintenance_category"]},"scribunto":{"limitreport-timeusage":{"value":"0.138","limit":"10.000"},"limitreport-memusage":{"value":3486992,"limit":52428800}},"cachereport":{"origin":"mw1352","timestamp":"20200528111313","ttl":2592000,"transientcontent":false}}});});</script>\n<script type="application/ld+json">{"@context":"https:\\/\\/schema.org","@type":"Article","name":"List of London boroughs","url":"https:\\/\\/en.wikipedia.org\\/wiki\\/List_of_London_boroughs","sameAs":"http:\\/\\/www.wikidata.org\\/entity\\/Q6577004","mainEntity":"http:\\/\\/www.wikidata.org\\/entity\\/Q6577004","author":{"@type":"Organization","name":"Contributors to Wikimedia projects"},"publisher":{"@type":"Organization","name":"Wikimedia Foundation, Inc.","logo":{"@type":"ImageObject","url":"https:\\/\\/www.wikimedia.org\\/static\\/images\\/wmf-hor-googpub.png"}},"datePublished":"2010-07-20T07:28:35Z","dateModified":"2020-05-26T03:30:43Z","headline":"Wikimedia list article"}</script>\n<script>(RLQ=window.RLQ||[]).push(function(){mw.config.set({"wgBackendResponseTime":103,"wgHostname":"mw1332"});});</script></body></html>\n'




```python
# start making the basis of the dataframe
BoroughName = []
Population = []
Coordinates = []

for row in soup.find('table').find_all('tr'):
    cells = row.find_all('td')
    if len(cells) > 0:
        BoroughName.append(cells[0].text.rstrip('\n'))
        Population.append(cells[7].text.rstrip('\n'))
        Coordinates.append(cells[8].text.rstrip('\n'))
```


```python
# Form a dataframe
dict = {'BoroughName' : BoroughName,
       'Population' : Population,
       'Coordinates': Coordinates}
info = pd.DataFrame.from_dict(dict)
info.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BoroughName</th>
      <th>Population</th>
      <th>Coordinates</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barking and Dagenham [note 1]</td>
      <td>194,352</td>
      <td>51°33′39″N 0°09′21″E﻿ / ﻿51.5607°N 0.1557°E﻿ /...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barnet</td>
      <td>369,088</td>
      <td>51°37′31″N 0°09′06″W﻿ / ﻿51.6252°N 0.1517°W﻿ /...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bexley</td>
      <td>236,687</td>
      <td>51°27′18″N 0°09′02″E﻿ / ﻿51.4549°N 0.1505°E﻿ /...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Brent</td>
      <td>317,264</td>
      <td>51°33′32″N 0°16′54″W﻿ / ﻿51.5588°N 0.2817°W﻿ /...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bromley</td>
      <td>317,899</td>
      <td>51°24′14″N 0°01′11″E﻿ / ﻿51.4039°N 0.0198°E﻿ /...</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Strip unwanted texts
info['BoroughName'] = info['BoroughName'].map(lambda x: x.rstrip(']'))
info['BoroughName'] = info['BoroughName'].map(lambda x: x.rstrip('1234567890.'))
info['BoroughName'] = info['BoroughName'].str.replace('note','')
info['BoroughName'] = info['BoroughName'].map(lambda x: x.rstrip(' ['))
info.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BoroughName</th>
      <th>Population</th>
      <th>Coordinates</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barking and Dagenham</td>
      <td>194,352</td>
      <td>51°33′39″N 0°09′21″E﻿ / ﻿51.5607°N 0.1557°E﻿ /...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barnet</td>
      <td>369,088</td>
      <td>51°37′31″N 0°09′06″W﻿ / ﻿51.6252°N 0.1517°W﻿ /...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bexley</td>
      <td>236,687</td>
      <td>51°27′18″N 0°09′02″E﻿ / ﻿51.4549°N 0.1505°E﻿ /...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Brent</td>
      <td>317,264</td>
      <td>51°33′32″N 0°16′54″W﻿ / ﻿51.5588°N 0.2817°W﻿ /...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bromley</td>
      <td>317,899</td>
      <td>51°24′14″N 0°01′11″E﻿ / ﻿51.4039°N 0.0198°E﻿ /...</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Clean coordinates
info[['Coordinates1','Coordinates2','Coordinates3']] = info['Coordinates'].str.split('/',expand=True)
info.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BoroughName</th>
      <th>Population</th>
      <th>Coordinates</th>
      <th>Coordinates1</th>
      <th>Coordinates2</th>
      <th>Coordinates3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barking and Dagenham</td>
      <td>194,352</td>
      <td>51°33′39″N 0°09′21″E﻿ / ﻿51.5607°N 0.1557°E﻿ /...</td>
      <td>51°33′39″N 0°09′21″E﻿</td>
      <td>﻿51.5607°N 0.1557°E﻿</td>
      <td>51.5607; 0.1557﻿ (Barking and Dagenham)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barnet</td>
      <td>369,088</td>
      <td>51°37′31″N 0°09′06″W﻿ / ﻿51.6252°N 0.1517°W﻿ /...</td>
      <td>51°37′31″N 0°09′06″W﻿</td>
      <td>﻿51.6252°N 0.1517°W﻿</td>
      <td>51.6252; -0.1517﻿ (Barnet)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bexley</td>
      <td>236,687</td>
      <td>51°27′18″N 0°09′02″E﻿ / ﻿51.4549°N 0.1505°E﻿ /...</td>
      <td>51°27′18″N 0°09′02″E﻿</td>
      <td>﻿51.4549°N 0.1505°E﻿</td>
      <td>51.4549; 0.1505﻿ (Bexley)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Brent</td>
      <td>317,264</td>
      <td>51°33′32″N 0°16′54″W﻿ / ﻿51.5588°N 0.2817°W﻿ /...</td>
      <td>51°33′32″N 0°16′54″W﻿</td>
      <td>﻿51.5588°N 0.2817°W﻿</td>
      <td>51.5588; -0.2817﻿ (Brent)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bromley</td>
      <td>317,899</td>
      <td>51°24′14″N 0°01′11″E﻿ / ﻿51.4039°N 0.0198°E﻿ /...</td>
      <td>51°24′14″N 0°01′11″E﻿</td>
      <td>﻿51.4039°N 0.0198°E﻿</td>
      <td>51.4039; 0.0198﻿ (Bromley)</td>
    </tr>
  </tbody>
</table>
</div>




```python
info.drop(labels=['Coordinates','Coordinates1','Coordinates2'], axis=1,inplace = True)
info[['Latitude','Longitude']] = info['Coordinates3'].str.split(';',expand=True)
info.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BoroughName</th>
      <th>Population</th>
      <th>Coordinates3</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barking and Dagenham</td>
      <td>194,352</td>
      <td>51.5607; 0.1557﻿ (Barking and Dagenham)</td>
      <td>51.5607</td>
      <td>0.1557﻿ (Barking and Dagenham)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barnet</td>
      <td>369,088</td>
      <td>51.6252; -0.1517﻿ (Barnet)</td>
      <td>51.6252</td>
      <td>-0.1517﻿ (Barnet)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bexley</td>
      <td>236,687</td>
      <td>51.4549; 0.1505﻿ (Bexley)</td>
      <td>51.4549</td>
      <td>0.1505﻿ (Bexley)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Brent</td>
      <td>317,264</td>
      <td>51.5588; -0.2817﻿ (Brent)</td>
      <td>51.5588</td>
      <td>-0.2817﻿ (Brent)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bromley</td>
      <td>317,899</td>
      <td>51.4039; 0.0198﻿ (Bromley)</td>
      <td>51.4039</td>
      <td>0.0198﻿ (Bromley)</td>
    </tr>
  </tbody>
</table>
</div>




```python
info.drop(labels=['Coordinates3'], axis=1,inplace = True)
info['Latitude'] = info['Latitude'].map(lambda x: x.rstrip(u'\ufeff'))
info['Latitude'] = info['Latitude'].map(lambda x: x.lstrip())
info['Longitude'] = info['Longitude'].map(lambda x: x.rstrip(')'))
info['Longitude'] = info['Longitude'].map(lambda x: x.rstrip('abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ '))
info['Longitude'] = info['Longitude'].map(lambda x: x.rstrip(' ('))
info['Longitude'] = info['Longitude'].map(lambda x: x.rstrip(u'\ufeff'))
info['Longitude'] = info['Longitude'].map(lambda x: x.lstrip())
info['Population'] = info['Population'].str.replace(',','')
info.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BoroughName</th>
      <th>Population</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barking and Dagenham</td>
      <td>194352</td>
      <td>51.5607</td>
      <td>0.1557</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barnet</td>
      <td>369088</td>
      <td>51.6252</td>
      <td>-0.1517</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bexley</td>
      <td>236687</td>
      <td>51.4549</td>
      <td>0.1505</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Brent</td>
      <td>317264</td>
      <td>51.5588</td>
      <td>-0.2817</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bromley</td>
      <td>317899</td>
      <td>51.4039</td>
      <td>0.0198</td>
    </tr>
  </tbody>
</table>
</div>




```python
info['BoroughName'].unique()
```




    array(['Barking and Dagenham', 'Barnet', 'Bexley', 'Brent', 'Bromley',
           'Camden', 'Croydon', 'Ealing', 'Enfield', 'Greenwich', 'Hackney',
           'Hammersmith and Fulham', 'Haringey', 'Harrow', 'Havering',
           'Hillingdon', 'Hounslow', 'Islington', 'Kensington and Chelsea',
           'Kingston upon Thames', 'Lambeth', 'Lewisham', 'Merton', 'Newham',
           'Redbridge', 'Richmond upon Thames', 'Southwark', 'Sutton',
           'Tower Hamlets', 'Waltham Forest', 'Wandsworth', 'Westminster'],
          dtype=object)




```python
# Foursquare credentials
CLIENT_ID = 'FWK0PAW5O3QNARLTJQJ4UVMDCVFPE0JGXRUU424A1RGVE5Y5' # your Foursquare ID
CLIENT_SECRET = 'I5HKU4M2QZNP2OOU3ZMX5V5ME51UQ4UMPKKDXA2GOJ51QKJM' # your Foursquare Secret
VERSION = '20180605'

```


```python
#Create a function to explore all borough
def getNearbyVenues(names, latitudes, longitudes, radius=500):
    
    venues_list=[]
    for name, lat, lng in zip(names, latitudes, longitudes):
        print(name)
            
        # create the API request URL
        url = 'https://api.foursquare.com/v2/venues/explore?&client_id={}&client_secret={}&v={}&ll={},{}&radius={}&limit={}'.format(
            CLIENT_ID, 
            CLIENT_SECRET, 
            VERSION, 
            lat, 
            lng, 
            radius, 
            LIMIT)
            
        # make the GET request
        results = requests.get(url).json()["response"]['groups'][0]['items']
        
        # return only relevant information for each nearby venue
        venues_list.append([(
            name, 
            lat, 
            lng, 
            v['venue']['name'], 
            v['venue']['location']['lat'], 
            v['venue']['location']['lng'],  
            v['venue']['categories'][0]['name']) for v in results])

    nearby_venues = pd.DataFrame([item for venue_list in venues_list for item in venue_list])
    nearby_venues.columns = ['BoroughName', 
                  'Borough Latitude', 
                  'Borough Longitude', 
                  'Venue', 
                  'Venue Latitude', 
                  'Venue Longitude', 
                  'Venue Category']
    
    return(nearby_venues)
```


```python
#Get top 50 venues in 500m radius of the center of each Borough
LIMIT = 50
venues = getNearbyVenues(names=info['BoroughName'],
                                   latitudes=info['Latitude'],
                                   longitudes=info['Longitude']
                                  )
```

    Barking and Dagenham
    Barnet
    Bexley
    Brent
    Bromley
    Camden
    Croydon
    Ealing
    Enfield
    Greenwich
    Hackney
    Hammersmith and Fulham
    Haringey
    Harrow
    Havering
    Hillingdon
    Hounslow
    Islington
    Kensington and Chelsea
    Kingston upon Thames
    Lambeth
    Lewisham
    Merton
    Newham
    Redbridge
    Richmond upon Thames
    Southwark
    Sutton
    Tower Hamlets
    Waltham Forest
    Wandsworth
    Westminster
    

#### 2) List of crimes in London’s boroughs with their addresses:
In this section i will import the data of crimes in london


```python
# Read crime records data for the last 24 months 
crime = pd.read_csv(r"C:\Users\dalal\Downloads\crimes.csv")
crime.head(5)

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>maj</th>
      <th>min</th>
      <th>BoroughName</th>
      <th>201805</th>
      <th>201806</th>
      <th>201807</th>
      <th>201808</th>
      <th>201809</th>
      <th>201810</th>
      <th>201811</th>
      <th>201812</th>
      <th>201901</th>
      <th>201902</th>
      <th>201903</th>
      <th>201904</th>
      <th>201905</th>
      <th>201906</th>
      <th>201907</th>
      <th>201908</th>
      <th>201909</th>
      <th>201910</th>
      <th>201911</th>
      <th>201912</th>
      <th>202001</th>
      <th>202002</th>
      <th>202003</th>
      <th>202004</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Arson and Criminal Damage</td>
      <td>Arson</td>
      <td>Barking and Dagenham</td>
      <td>4</td>
      <td>12</td>
      <td>6</td>
      <td>5</td>
      <td>3</td>
      <td>8</td>
      <td>5</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
      <td>5</td>
      <td>5</td>
      <td>11</td>
      <td>3</td>
      <td>5</td>
      <td>3</td>
      <td>6</td>
      <td>9</td>
      <td>8</td>
      <td>6</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Arson and Criminal Damage</td>
      <td>Criminal Damage</td>
      <td>Barking and Dagenham</td>
      <td>126</td>
      <td>123</td>
      <td>127</td>
      <td>101</td>
      <td>107</td>
      <td>132</td>
      <td>105</td>
      <td>88</td>
      <td>97</td>
      <td>127</td>
      <td>138</td>
      <td>130</td>
      <td>139</td>
      <td>113</td>
      <td>134</td>
      <td>118</td>
      <td>109</td>
      <td>109</td>
      <td>97</td>
      <td>121</td>
      <td>97</td>
      <td>103</td>
      <td>108</td>
      <td>82</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Burglary</td>
      <td>Burglary - Business and Community</td>
      <td>Barking and Dagenham</td>
      <td>24</td>
      <td>33</td>
      <td>30</td>
      <td>18</td>
      <td>33</td>
      <td>32</td>
      <td>39</td>
      <td>33</td>
      <td>45</td>
      <td>24</td>
      <td>29</td>
      <td>27</td>
      <td>22</td>
      <td>27</td>
      <td>31</td>
      <td>35</td>
      <td>37</td>
      <td>30</td>
      <td>30</td>
      <td>25</td>
      <td>31</td>
      <td>17</td>
      <td>27</td>
      <td>29</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Burglary</td>
      <td>Burglary - Residential</td>
      <td>Barking and Dagenham</td>
      <td>93</td>
      <td>77</td>
      <td>94</td>
      <td>84</td>
      <td>99</td>
      <td>94</td>
      <td>106</td>
      <td>164</td>
      <td>114</td>
      <td>107</td>
      <td>99</td>
      <td>96</td>
      <td>114</td>
      <td>96</td>
      <td>71</td>
      <td>67</td>
      <td>80</td>
      <td>97</td>
      <td>114</td>
      <td>130</td>
      <td>116</td>
      <td>123</td>
      <td>97</td>
      <td>56</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Drug Offences</td>
      <td>Drug Trafficking</td>
      <td>Barking and Dagenham</td>
      <td>8</td>
      <td>6</td>
      <td>8</td>
      <td>7</td>
      <td>10</td>
      <td>7</td>
      <td>7</td>
      <td>4</td>
      <td>5</td>
      <td>2</td>
      <td>6</td>
      <td>5</td>
      <td>9</td>
      <td>6</td>
      <td>11</td>
      <td>7</td>
      <td>7</td>
      <td>10</td>
      <td>12</td>
      <td>3</td>
      <td>11</td>
      <td>3</td>
      <td>6</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>




```python
#crime.shape
#(1567,27)
# Names of Boroughs
crime['BoroughName'].unique()
```




    array(['Barking and Dagenham', 'Barnet', 'Bexley', 'Brent', 'Bromley',
           'Camden', 'Croydon', 'Ealing', 'Enfield', 'Greenwich', 'Hackney',
           'Hammersmith and Fulham', 'Haringey', 'Harrow', 'Havering',
           'Hillingdon', 'Hounslow', 'Islington', 'Kensington and Chelsea',
           'Kingston upon Thames', 'Lambeth', 'Lewisham',
           'London Heathrow and London City Airports', 'Merton', 'Newham',
           'Redbridge', 'Richmond upon Thames', 'Southwark', 'Sutton',
           'Tower Hamlets', 'Waltham Forest', 'Wandsworth', 'Westminster'],
          dtype=object)




```python
# add new columns that sum up the 24 incident in each Borough
crime['sum']=crime.iloc[:,3:27].sum(axis=1)
crimesum=crime['sum']
crime.drop(labels=['sum'], axis=1, inplace= True)
crime.insert(3, 'sum',crimesum)
crime.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>maj</th>
      <th>min</th>
      <th>BoroughName</th>
      <th>sum</th>
      <th>201805</th>
      <th>201806</th>
      <th>201807</th>
      <th>201808</th>
      <th>201809</th>
      <th>201810</th>
      <th>201811</th>
      <th>201812</th>
      <th>201901</th>
      <th>201902</th>
      <th>201903</th>
      <th>201904</th>
      <th>201905</th>
      <th>201906</th>
      <th>201907</th>
      <th>201908</th>
      <th>201909</th>
      <th>201910</th>
      <th>201911</th>
      <th>201912</th>
      <th>202001</th>
      <th>202002</th>
      <th>202003</th>
      <th>202004</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Arson and Criminal Damage</td>
      <td>Arson</td>
      <td>Barking and Dagenham</td>
      <td>129</td>
      <td>4</td>
      <td>12</td>
      <td>6</td>
      <td>5</td>
      <td>3</td>
      <td>8</td>
      <td>5</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
      <td>5</td>
      <td>5</td>
      <td>11</td>
      <td>3</td>
      <td>5</td>
      <td>3</td>
      <td>6</td>
      <td>9</td>
      <td>8</td>
      <td>6</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Arson and Criminal Damage</td>
      <td>Criminal Damage</td>
      <td>Barking and Dagenham</td>
      <td>2731</td>
      <td>126</td>
      <td>123</td>
      <td>127</td>
      <td>101</td>
      <td>107</td>
      <td>132</td>
      <td>105</td>
      <td>88</td>
      <td>97</td>
      <td>127</td>
      <td>138</td>
      <td>130</td>
      <td>139</td>
      <td>113</td>
      <td>134</td>
      <td>118</td>
      <td>109</td>
      <td>109</td>
      <td>97</td>
      <td>121</td>
      <td>97</td>
      <td>103</td>
      <td>108</td>
      <td>82</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Burglary</td>
      <td>Burglary - Business and Community</td>
      <td>Barking and Dagenham</td>
      <td>708</td>
      <td>24</td>
      <td>33</td>
      <td>30</td>
      <td>18</td>
      <td>33</td>
      <td>32</td>
      <td>39</td>
      <td>33</td>
      <td>45</td>
      <td>24</td>
      <td>29</td>
      <td>27</td>
      <td>22</td>
      <td>27</td>
      <td>31</td>
      <td>35</td>
      <td>37</td>
      <td>30</td>
      <td>30</td>
      <td>25</td>
      <td>31</td>
      <td>17</td>
      <td>27</td>
      <td>29</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Burglary</td>
      <td>Burglary - Residential</td>
      <td>Barking and Dagenham</td>
      <td>2388</td>
      <td>93</td>
      <td>77</td>
      <td>94</td>
      <td>84</td>
      <td>99</td>
      <td>94</td>
      <td>106</td>
      <td>164</td>
      <td>114</td>
      <td>107</td>
      <td>99</td>
      <td>96</td>
      <td>114</td>
      <td>96</td>
      <td>71</td>
      <td>67</td>
      <td>80</td>
      <td>97</td>
      <td>114</td>
      <td>130</td>
      <td>116</td>
      <td>123</td>
      <td>97</td>
      <td>56</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Drug Offences</td>
      <td>Drug Trafficking</td>
      <td>Barking and Dagenham</td>
      <td>169</td>
      <td>8</td>
      <td>6</td>
      <td>8</td>
      <td>7</td>
      <td>10</td>
      <td>7</td>
      <td>7</td>
      <td>4</td>
      <td>5</td>
      <td>2</td>
      <td>6</td>
      <td>5</td>
      <td>9</td>
      <td>6</td>
      <td>11</td>
      <td>7</td>
      <td>7</td>
      <td>10</td>
      <td>12</td>
      <td>3</td>
      <td>11</td>
      <td>3</td>
      <td>6</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>




```python
# make a table that has two columns of Borough name and number of crimes 
crime.drop(crime.columns[0:2], axis=1, inplace=True)
crime.drop(crime.columns[2:27], axis=1, inplace=True)
crime.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BoroughName</th>
      <th>sum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barking and Dagenham</td>
      <td>129</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barking and Dagenham</td>
      <td>2731</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Barking and Dagenham</td>
      <td>708</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Barking and Dagenham</td>
      <td>2388</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Barking and Dagenham</td>
      <td>169</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate sum of incidents in each Borough for the last 24 months
crime=crime.groupby(['BoroughName'], as_index=False).sum()
crime.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BoroughName</th>
      <th>sum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barking and Dagenham</td>
      <td>38786</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barnet</td>
      <td>59877</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bexley</td>
      <td>33907</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Brent</td>
      <td>60584</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bromley</td>
      <td>48235</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Change the sum into monthly average
crime['sum'] = crime['sum']/24
crime.rename(columns={crime.columns[1]:'MonthlyAverage'}, inplace=True)
crime.head(30)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BoroughName</th>
      <th>MonthlyAverage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barking and Dagenham</td>
      <td>1616.083333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barnet</td>
      <td>2494.875000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bexley</td>
      <td>1412.791667</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Brent</td>
      <td>2524.333333</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bromley</td>
      <td>2009.791667</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Camden</td>
      <td>3117.750000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Croydon</td>
      <td>2748.791667</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Ealing</td>
      <td>2525.083333</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Enfield</td>
      <td>2454.000000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Greenwich</td>
      <td>2293.750000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Hackney</td>
      <td>2734.125000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Hammersmith and Fulham</td>
      <td>1895.666667</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Haringey</td>
      <td>2623.083333</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Harrow</td>
      <td>1380.416667</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Havering</td>
      <td>1560.875000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Hillingdon</td>
      <td>2183.916667</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Hounslow</td>
      <td>2182.750000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Islington</td>
      <td>2442.875000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Kensington and Chelsea</td>
      <td>1941.916667</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Kingston upon Thames</td>
      <td>1061.375000</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Lambeth</td>
      <td>2920.541667</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Lewisham</td>
      <td>2324.541667</td>
    </tr>
    <tr>
      <th>22</th>
      <td>London Heathrow and London City Airports</td>
      <td>281.750000</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Merton</td>
      <td>1179.666667</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Newham</td>
      <td>2973.875000</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Redbridge</td>
      <td>1976.708333</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Richmond upon Thames</td>
      <td>1064.000000</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Southwark</td>
      <td>3122.083333</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Sutton</td>
      <td>1115.375000</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Tower Hamlets</td>
      <td>2853.416667</td>
    </tr>
  </tbody>
</table>
</div>



##### 2) Done with the list of of crimes above.



##### 3) List of rental prices in each Borough in London:
The imported data will represent all the categories of accomodations (room, studio..) for more ease with average prices from 2018 till 2019 for each Borough.



```python
# with the folowing codes we will import a data about renting prices 
rent = pd.read_excel(r"C:\Users\dalal\OneDrive\Desktop\renting.xls", sheet_name='Rent')
rent.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Quarter</th>
      <th>Code</th>
      <th>Area</th>
      <th>Category</th>
      <th>Count of rents</th>
      <th>Average</th>
      <th>Lower quartile</th>
      <th>Median</th>
      <th>Upper quartile</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018</td>
      <td>Q1</td>
      <td>E09000002</td>
      <td>Barking and Dagenham</td>
      <td>All categories</td>
      <td>780</td>
      <td>1184</td>
      <td>1000</td>
      <td>1200</td>
      <td>1350</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018</td>
      <td>Q1</td>
      <td>E09000003</td>
      <td>Barnet</td>
      <td>All categories</td>
      <td>2100</td>
      <td>1508</td>
      <td>1158</td>
      <td>1350</td>
      <td>1690</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018</td>
      <td>Q1</td>
      <td>E09000004</td>
      <td>Bexley</td>
      <td>All categories</td>
      <td>770</td>
      <td>1044</td>
      <td>850</td>
      <td>1023</td>
      <td>1200</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018</td>
      <td>Q1</td>
      <td>E09000005</td>
      <td>Brent</td>
      <td>All categories</td>
      <td>2020</td>
      <td>1571</td>
      <td>1235</td>
      <td>1480</td>
      <td>1800</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018</td>
      <td>Q1</td>
      <td>E09000006</td>
      <td>Bromley</td>
      <td>All categories</td>
      <td>1520</td>
      <td>1226</td>
      <td>965</td>
      <td>1150</td>
      <td>1380</td>
    </tr>
  </tbody>
</table>
</div>




```python
#drp unnecessery columns 
rent.drop(rent.columns[0:3], axis=1, inplace= True)
rent.drop(rent.columns[1:3], axis=1, inplace= True)
rent.drop(rent.columns[2:5], axis=1, inplace=True)
rent
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area</th>
      <th>Average</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barking and Dagenham</td>
      <td>1184</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barnet</td>
      <td>1508</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bexley</td>
      <td>1044</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Brent</td>
      <td>1571</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bromley</td>
      <td>1226</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Camden</td>
      <td>1961</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Croydon</td>
      <td>1144</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Ealing</td>
      <td>1514</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Enfield</td>
      <td>1324</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Greenwich</td>
      <td>1434</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Hackney</td>
      <td>1795</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Hammersmith and Fulham</td>
      <td>1741</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Haringey</td>
      <td>1496</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Harrow</td>
      <td>1361</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Havering</td>
      <td>1135</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Hillingdon</td>
      <td>1281</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Hounslow</td>
      <td>1309</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Islington</td>
      <td>1860</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Kensington and Chelsea</td>
      <td>2882</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Kingston upon Thames</td>
      <td>1406</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Lambeth</td>
      <td>1647</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Lewisham</td>
      <td>1306</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Merton</td>
      <td>1584</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Newham</td>
      <td>1374</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Redbridge</td>
      <td>1270</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Richmond upon Thames</td>
      <td>1891</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Southwark</td>
      <td>1623</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Sutton</td>
      <td>1137</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Tower Hamlets</td>
      <td>1739</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Waltham Forest</td>
      <td>1300</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Wandsworth</td>
      <td>1873</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Westminster</td>
      <td>2784</td>
    </tr>
    <tr>
      <th>32</th>
      <td>LONDON</td>
      <td>1605</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Barking and Dagenham</td>
      <td>1193</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Barnet</td>
      <td>1535</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Bexley</td>
      <td>1026</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Brent</td>
      <td>1582</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Bromley</td>
      <td>1250</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Camden</td>
      <td>2117</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Croydon</td>
      <td>1133</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Ealing</td>
      <td>1532</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Enfield</td>
      <td>1357</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Greenwich</td>
      <td>1392</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Hackney</td>
      <td>1856</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Hammersmith and Fulham</td>
      <td>2005</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Haringey</td>
      <td>1520</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Harrow</td>
      <td>1359</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Havering</td>
      <td>1135</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Hillingdon</td>
      <td>1245</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Hounslow</td>
      <td>1296</td>
    </tr>
    <tr>
      <th>50</th>
      <td>Islington</td>
      <td>1904</td>
    </tr>
    <tr>
      <th>51</th>
      <td>Kensington and Chelsea</td>
      <td>3173</td>
    </tr>
    <tr>
      <th>52</th>
      <td>Kingston upon Thames</td>
      <td>1355</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Lambeth</td>
      <td>1670</td>
    </tr>
    <tr>
      <th>54</th>
      <td>Lewisham</td>
      <td>1280</td>
    </tr>
    <tr>
      <th>55</th>
      <td>Merton</td>
      <td>1576</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Newham</td>
      <td>1413</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Redbridge</td>
      <td>1267</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Richmond upon Thames</td>
      <td>2000</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Southwark</td>
      <td>1705</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Sutton</td>
      <td>1114</td>
    </tr>
    <tr>
      <th>61</th>
      <td>Tower Hamlets</td>
      <td>1762</td>
    </tr>
    <tr>
      <th>62</th>
      <td>Waltham Forest</td>
      <td>1303</td>
    </tr>
    <tr>
      <th>63</th>
      <td>Wandsworth</td>
      <td>1855</td>
    </tr>
    <tr>
      <th>64</th>
      <td>Westminster</td>
      <td>2709</td>
    </tr>
    <tr>
      <th>65</th>
      <td>LONDON</td>
      <td>1679</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Barking and Dagenham</td>
      <td>1192</td>
    </tr>
    <tr>
      <th>67</th>
      <td>Barnet</td>
      <td>1548</td>
    </tr>
    <tr>
      <th>68</th>
      <td>Bexley</td>
      <td>1084</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Brent</td>
      <td>1578</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Bromley</td>
      <td>1318</td>
    </tr>
    <tr>
      <th>71</th>
      <td>Camden</td>
      <td>2427</td>
    </tr>
    <tr>
      <th>72</th>
      <td>Croydon</td>
      <td>1112</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Ealing</td>
      <td>1484</td>
    </tr>
    <tr>
      <th>74</th>
      <td>Enfield</td>
      <td>1325</td>
    </tr>
    <tr>
      <th>75</th>
      <td>Greenwich</td>
      <td>1380</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Hackney</td>
      <td>1834</td>
    </tr>
    <tr>
      <th>77</th>
      <td>Hammersmith and Fulham</td>
      <td>2070</td>
    </tr>
    <tr>
      <th>78</th>
      <td>Haringey</td>
      <td>1513</td>
    </tr>
    <tr>
      <th>79</th>
      <td>Harrow</td>
      <td>1396</td>
    </tr>
    <tr>
      <th>80</th>
      <td>Havering</td>
      <td>1131</td>
    </tr>
    <tr>
      <th>81</th>
      <td>Hillingdon</td>
      <td>1268</td>
    </tr>
    <tr>
      <th>82</th>
      <td>Hounslow</td>
      <td>1410</td>
    </tr>
    <tr>
      <th>83</th>
      <td>Islington</td>
      <td>1895</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Kensington and Chelsea</td>
      <td>3208</td>
    </tr>
    <tr>
      <th>85</th>
      <td>Kingston upon Thames</td>
      <td>1396</td>
    </tr>
    <tr>
      <th>86</th>
      <td>Lambeth</td>
      <td>1751</td>
    </tr>
    <tr>
      <th>87</th>
      <td>Lewisham</td>
      <td>1318</td>
    </tr>
    <tr>
      <th>88</th>
      <td>Merton</td>
      <td>1542</td>
    </tr>
    <tr>
      <th>89</th>
      <td>Newham</td>
      <td>1422</td>
    </tr>
    <tr>
      <th>90</th>
      <td>Redbridge</td>
      <td>1303</td>
    </tr>
    <tr>
      <th>91</th>
      <td>Richmond upon Thames</td>
      <td>1896</td>
    </tr>
    <tr>
      <th>92</th>
      <td>Southwark</td>
      <td>1676</td>
    </tr>
    <tr>
      <th>93</th>
      <td>Sutton</td>
      <td>1130</td>
    </tr>
    <tr>
      <th>94</th>
      <td>Tower Hamlets</td>
      <td>1773</td>
    </tr>
    <tr>
      <th>95</th>
      <td>Waltham Forest</td>
      <td>1314</td>
    </tr>
    <tr>
      <th>96</th>
      <td>Wandsworth</td>
      <td>1828</td>
    </tr>
    <tr>
      <th>97</th>
      <td>Westminster</td>
      <td>2832</td>
    </tr>
    <tr>
      <th>98</th>
      <td>LONDON</td>
      <td>1727</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Sum the average cost of renting for each boroughs from 2018 to the first quarter of 2019
Borough_rent= rent.groupby(['Area'], as_index=False).sum() #Area is Borough dont mix up ;}
Borough_rent

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area</th>
      <th>Average</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barking and Dagenham</td>
      <td>3569</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barnet</td>
      <td>4591</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bexley</td>
      <td>3154</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Brent</td>
      <td>4731</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bromley</td>
      <td>3794</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Camden</td>
      <td>6505</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Croydon</td>
      <td>3389</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Ealing</td>
      <td>4530</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Enfield</td>
      <td>4006</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Greenwich</td>
      <td>4206</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Hackney</td>
      <td>5485</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Hammersmith and Fulham</td>
      <td>5816</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Haringey</td>
      <td>4529</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Harrow</td>
      <td>4116</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Havering</td>
      <td>3401</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Hillingdon</td>
      <td>3794</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Hounslow</td>
      <td>4015</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Islington</td>
      <td>5659</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Kensington and Chelsea</td>
      <td>9263</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Kingston upon Thames</td>
      <td>4157</td>
    </tr>
    <tr>
      <th>20</th>
      <td>LONDON</td>
      <td>5011</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Lambeth</td>
      <td>5068</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Lewisham</td>
      <td>3904</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Merton</td>
      <td>4702</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Newham</td>
      <td>4209</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Redbridge</td>
      <td>3840</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Richmond upon Thames</td>
      <td>5787</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Southwark</td>
      <td>5004</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Sutton</td>
      <td>3381</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Tower Hamlets</td>
      <td>5274</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Waltham Forest</td>
      <td>3917</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Wandsworth</td>
      <td>5556</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Westminster</td>
      <td>8325</td>
    </tr>
  </tbody>
</table>
</div>



##### Done with avg rent for each of London Boroughs 


## So our data is prepard for further analysis :

So let us double check with our data after being arranged and deployed:


```python
#list of Boroughs with population , long and lat
info.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BoroughName</th>
      <th>Population</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barking and Dagenham</td>
      <td>194352</td>
      <td>51.5607</td>
      <td>0.1557</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barnet</td>
      <td>369088</td>
      <td>51.6252</td>
      <td>-0.1517</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bexley</td>
      <td>236687</td>
      <td>51.4549</td>
      <td>0.1505</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Brent</td>
      <td>317264</td>
      <td>51.5588</td>
      <td>-0.2817</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bromley</td>
      <td>317899</td>
      <td>51.4039</td>
      <td>0.0198</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Camden</td>
      <td>229719</td>
      <td>51.5290</td>
      <td>-0.1255</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Croydon</td>
      <td>372752</td>
      <td>51.3714</td>
      <td>-0.0977</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Ealing</td>
      <td>342494</td>
      <td>51.5130</td>
      <td>-0.3089</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Enfield</td>
      <td>320524</td>
      <td>51.6538</td>
      <td>-0.0799</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Greenwich</td>
      <td>264008</td>
      <td>51.4892</td>
      <td>0.0648</td>
    </tr>
  </tbody>
</table>
</div>




```python
#list of crimes in each boroughs for on monthly basis
crime.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BoroughName</th>
      <th>MonthlyAverage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barking and Dagenham</td>
      <td>1616.083333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barnet</td>
      <td>2494.875000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bexley</td>
      <td>1412.791667</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Brent</td>
      <td>2524.333333</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bromley</td>
      <td>2009.791667</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Camden</td>
      <td>3117.750000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Croydon</td>
      <td>2748.791667</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Ealing</td>
      <td>2525.083333</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Enfield</td>
      <td>2454.000000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Greenwich</td>
      <td>2293.750000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#list of borough based on avg rent for all categories of accomodation 
Borough_rent.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area</th>
      <th>Average</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barking and Dagenham</td>
      <td>3569</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barnet</td>
      <td>4591</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bexley</td>
      <td>3154</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Brent</td>
      <td>4731</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bromley</td>
      <td>3794</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Camden</td>
      <td>6505</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Croydon</td>
      <td>3389</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Ealing</td>
      <td>4530</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Enfield</td>
      <td>4006</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Greenwich</td>
      <td>4206</td>
    </tr>
  </tbody>
</table>
</div>


