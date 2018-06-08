---
layout: post
title: "Come for the company formation.  Stay for the beaches!"
author: Jason Shea
description: >
    Tour of ADS-B aircraft tracking capabilities and an outline of discoveries relating money laundering hubs to Russian oligarch travels.

---


### Who Watches the Wealthy

Roman Abramovich is famously a man of expensive tastes. Many times a month, he flies between Moscow and London, unsurprising given his well-known business interests in both capitals. He frequents St. Petersburg and New York as well and makes regular appearances in Monaco, Sochi, and the Black Forest in Germany.  Recently in pursuit of his newly acquired Israeli citizenship, he’s flown multiple times to and from Tel Aviv.

It would be uncouth for a man of such refined airs to travel on a commercial airline, and indeed Mr. Abramovich maintains a sleekly decorated Boeing 767, among other aircraft, to escort him in comfort and style.  

Like most aircraft, Mr. Abramovich’s Boeing regularly, though not always, transmits its location along with various other data about the aircraft on a public high-frequency radio band, primarily for use by air-traffic control systems and other aircraft, known as the ADS-B system. 

<figure>
  <img src="https://www.adsbexchange.com/wp-content/uploads/Everythingoncan-225x300.jpg" align="right" text-align="right">
<!--  <caption>The bleeding edge of aircraft detection.  Credit: www.adsbexchange.com</caption> -->
</figure>


These signals can be monitored with any appropriately tuned antenna – you can make one at home! – and several ‘flight tracking’ websites operate data gathering networks and present their findings to anyone who is interested. Some examples of these efforts below.

- [Plane Finder](https://planefinder.net/)
- [Flight Radar](https://www.flightradar24.com/)
- [Flight Aware](https://flightaware.com/)

Curious about Mr. Abramovich’s travels, we can input the tail number of his 767, visible from photographs as ‘P4-MES’, into one of the sites above.  Unfortunately, we would find no records of his travels.  Most flight tracking sites regularly redact data from governments and other select parties.  Rats. 

These barriers and the implied complicity of the flight-tracking community have prevented investigative reporters from examining the travels of the rich and powerful.  Until now. OCCRP data team has partnered with the world’s largest source of unfiltered flight data, the flight tracking network [ADSBexchange](www.adsbexchange.com), to gather and examine the travels of a host of oligarchs and political leaders as they have travelled the world in private aircraft.  

### The Data Trail in Five Minutes

Receivers in this co-operative network record information including the aircraft position, speed, altitude, and timestamp, all signed with an ‘address’ known as an ICAO address or Mode-S code, which uniquely identifies the aircraft.  Each of these receivers in turn has an effective range, in terms of distance from the receiver and altitude of the aircraft, to a maximum of around four hundred kilometers.  

<figure>
  <img src="https://github.com/occrp/tech.occrp.org/blob/adsb_post/assets/images/2018-06/600_si_fl_ads-b_22_fa2_flat_rev_5-15.jpg" align="right"  width="400px" text-align="right">
<!--  <caption>Credit: National Air and Space Museum, Smithsonian Institution.</caption> -->
</figure>

This network of receivers is located mostly on densely populated landmasses so depending on where an aircraft is flying, it will sometimes leave the network’s range entirely, potentially when they’ve flown across the Atlantic, or the Sahara.  Other wrinkles to assembling a clear picture of activity emerge when an aircraft descends under the effective altitude for signal transmission - predominantly to land - or when overlapping signals are detected by multiple receivers.  Any attempt to piece together a picture of travels by hand will inevitably be complex and painstaking - Abramovich alone has generated over forty-eight thousand aircraft position readings in a nearly two year period.

To assemble collections of readings that constitute distinct flights, OCCRP’s data team has designed a small collection of data analysis algorithms which, among other things, group observations of any aircraft across multiple atennae by timestamp and infer takeoff & landing through a combination of first & last position relative to major airports and aircraft altitude at the time.  

For example, if a collection shows an aircraft’s final position reading for (at least) many hours near London Heathrow at a low altitude, our algorithm assumes it’s landed there.  

<figure>
  <img src="https://github.com/occrp/tech.occrp.org/blob/adsb_post/assets/images/2018-06/48434F2016-07-01_Moscow_London.png" align="right" width="200px" text-align="right">
</figure>

By contrast, if we find the last reading at a high altitude above the North Atlantic off the Irish coast, with readings resuming after a couple of hours above Iceland, we stitch the two sets of positions together.  

<figure>
  <img src="https://github.com/occrp/tech.occrp.org/blob/adsb_post/assets/images/2018-06/48434F2016-08-14_UNKNOWN_London.png" align="right" width="200px" text-align="right">
</figure>

Note that flight data from this network should not be considered complete.  In many parts of the world, including many potential regions of interest, the available receivers may be few and far apart, and the sparse ADS-B data can force a ‘best guess’ methodology for projecting what airport the aircraft is departing / arriving.  In some cases, an aircraft’s signal is seen to disappear over an ocean and isn’t seen for weeks, due either to staying grounded / out of the network’s coverage area or potentially deactivating its’ transponder.

After observations are algorithmically grouped into distinct flights, we can zoom in on interesting travels by ‘geofencing’ destinations of interest, selecting all flights by aircraft of interest whose flight endpoints fall within range of select airports.  

In one example, Cyprus, long described as a gateway into Europe for Russian money, has seen many interesting visitors (each oligarch has his own color!... see below for details).


<img src="https://github.com/occrp/tech.occrp.org/blob/adsb_post/assets/images/2018-06/blog_cyprus_1.png" align="center">


### Come for Company Formation, Stay for the Beaches

While the wealthy and powerful travel European capitals and the exclusive resorts of the ultra-rich quite frequently, insight into their travels goes well beyond Monaco and Sochi, and visits to more tropical climates may prove of greater interest.  OCCRPs’ initial analysis has centered on sunny money laundering hubs including the British Virgin Islands, Cyprus, or Panama City, as well as the considerably less sunny Isle of Man.  To understand how these destinations might be of interest, look no further than OCCRP's fine reporting on two global operations.

<div class="captioned">
    <img src="https://www.occrp.org/images/panamapapers.jpg" width="400px" class="img-left">
    <a href="https://www.occrp.org/en/panamapapers/">
    <div class="caption">
        Credit: Dig deeper : The OCCRP reports on The Panama Papers
    </div>
</div>

<div class="captioned">
    <img src="https://github.com/occrp/tech.occrp.org/blob/adsb_post/assets/images/2018-06/laundromat-infographic-short.png" width="400px" class="img-right">
    <a href="https://www.occrp.org/en/laundromat/">
    <div class="caption">
        Credit: Dig deeper : The OCCRP reports on The Russian Laundromat
    </div>
</div>


Returning to Mr. Abramovich, we know his jet visited the area of the British Virgin Islands several times in early 2017, including this trip from New Jersey on February 17 2017, staying until February 27.  Similar trips occured on January 3, and March 24, and December 22. 

<img src="https://github.com/occrp/tech.occrp.org/blob/adsb_post/assets/images/2018-06/roma_VG.png" width="200px" class="img-right">


Are billionaires not allowed to soak in long sea-breeze Caribbean evenings on the beach along with the rest of us (who can afford to step foot in these exclusive communities)?  One imagines his evenings,enjoying peaceful Gulf sunsets while sipping from a bottle of 1780 Barbados Private Estate, attended by a small army of dolphins trained for his exclusive entertainment from the comfort of a glass-walled swimming pool levitating above a flawless peach-hued bay.  
OCCRP alleges no illegal activity by Mr. Abramovich or any of the other billionaires detailed below, only that they are frequent fliers to certain locations.  They may well be simply vacationing via private jet.  
It might just be of interest to see if any companies were registered during these afternoons. 

[British Virgin Islands Company Gazette](https://eservices.gov.vg/gazette/)



### Boyars in Paradise (and the Irish Sea)
OCCRP has initially examined 35 aircraft of interest flying more than 7.5 million kilometers over some 5800 flights.  This analysis has uncovered trips to known money laundering resorts from a number of notable figures including.
##### Roman Abramovich : Russian diversified businessman, philanthropist, and owner of Chelsea FC.
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ed/Roman_Abramovich_2.jpg/220px-Roman_Abramovich_2.jpg" width="200px" class="img-right">

| Date | Origin  | Destination |
| ------------- | ------------- | ------------- |
| 1/3/2017  | New Jersey, USA  | Anegada, British VG  |
| 3/24/2017  | Denver, USA  | Anegada, British VG  |

##### Alisher Usmanov : Uzbek-Russian industrial magnate and investor.

| Date | Origin  | Destination |
| ------------- | ------------- | ------------- |
| 2/20/2018  | Larnaca, Cyprus  | Mulhouse, France  |

##### Alexander Abramov : Russian steel magnate, chairman of Evraz Group.

| Date | Origin  | Destination |
| ------------- | ------------- | ------------- |
| 12/5/2016  | Van Nuys, USA  | Anegada, British VG  |
| 2/6/2017  | Houston, USA  | Anegada, British VG  |
| 2/23/2017  | Miami, USA  | Anegada, British VG  |

##### Farkhad Akhmedov : Azeri-Russian businessman and politician.

| Date | Origin  | Destination |
| ------------- | ------------- | ------------- |
| 1/17/2017  | Spanish Town, British VG  | Miami, USA  |

##### Len Blavatnik : Soviet-born British-American diversified businessman.  Active in American politics and longtime friend of Viktor Vekselberg.

| Date | Origin  | Destination |
| ------------- | ------------- | ------------- |
| 12/29/2016  | San Juan, Puerto Rico  | St. Thomas, Virgin Islands  |
| 4/9/2017  | Alice Town, Bahamas  | Anegada, British VG  |

##### Igor Makarov : Turkmen-Russian businessman, owner of ARETI International Group.  

| Date | Origin  | Destination |
| ------------- | ------------- | ------------- |
| 7/14/2016  | Paphos, Cyprus  | Unknown in France  |
| 10/12/2016  | Unknown, North Atlantic | Nicosia, Cyprus  |
| 10/15/2016  | Cairo, Egypt  | Larnaca, Cyprus  |

##### Andrey Guryev : Dominant in the Russian fertilizer industry; beneficial owner of PhosAgro.  

| Date | Origin  | Destination |
| ------------- | ------------- | ------------- |
| 8/13/2016  | Unknown in Greece  | Paphos, Cyprus  |


##### Dmytro Firtash : Ukranian businessman and ally of the Yuschenko & Yanukovich administrations.  US currently seeking extradition.

| Date | Origin  | Destination |
| ------------- | ------------- | ------------- |
| 3/25/2017  | Unknown in South Pacific  | Auckland, NZ  |

##### Leonid Mikhelson : Owner of Russian gas company Novatek and subject to US sanctions.  

| Date | Origin  | Destination |
| ------------- | ------------- | ------------- |
| 9/25/2016  | Akrotiri, Cyprus  | Strasbourg Neudorf, France  |

##### Oleg Tinkov : Russian diversified businessman and founder of Tinkoff Bank.  

| Date | Origin  | Destination |
| ------------- | ------------- | ------------- |
| 4/27/2018  | Florence, Italy  | Larnaca, Cyprus  |

##### Alexei Mordashov : Russian owner of Severstal.  

| Date | Origin  | Destination |
| ------------- | ------------- | ------------- |
| 5/10/2017  | Isle of Man  | London, UK  |

##### Alexander Mashkevitch : Kazakh-Israeli businessman and major shareholder in Eurasian National Resources Corporation. 

| Date | Origin  | Destination |
| ------------- | ------------- | ------------- |
| 12/16/2017  | Isle of Man  | London, UK  |
| 2/25/2018  | London, UK | Panama City, Panama  |
| 4/11/2018  | Panama City, Panama  | Punta Cana, Dominican Republic  |

{link to table : https://docs.google.com/spreadsheets/d/1TqrOlnOvxx38xUqIoWlkGkCofOEJpXHMjvAvsYxbNAk/edit?usp=sharing}

Investigative curiosities from the jet-setting frontier don’t stop at money laundering.  Witness Mr. Abramovich on 3-9-18, flying (it appears) from Crimea / Rostov-on-Don area, amusingly looping the hundreds of kilometers all the way round Ukrainian airspace, to London. 

<img src="https://github.com/occrp/tech.occrp.org/blob/adsb_post/assets/images/2018-06/48434F2018-03-09_Taganrog_Beccles.png">
 
Or his visit to the Iranian island oil and gas outpost of Sirri on January 28 2018...or perhaps the government of Equatorial Guinea ferrying honest public servants to Singapore, Mykonos, Mytilini, Winnipeg, Los Angeles, and Rio de Janeiro might interest you.

These findings are only the beginning.  We encourage interested journalists to contact data@occrp.org with research requests and opinions on how best to make this data available to reporters.  We want to hear from you!  
Food for thought:

- Should we publish a weekly digest of the movements of select aircraft or travel into certain locations?  
- Can we identify events whose airborne attendees we could study?  
- If you had access to a web resource which allowed you to browse the travels of select aircraft or ownership organizations (think ‘Azerbaijan Airlines’ or other pseudo-governmental carriers), how would you use it?
- [A live version of the ADSB Exchange tracking network is always viewable](https://global.adsbexchange.com/VirtualRadar/desktop.html)

<img src="https://github.com/occrp/tech.occrp.org/blob/adsb_post/assets/images/2018-06/adsb_exchange_screencap.jpg">
