# Solar Project

```
#
# Written by: 	Orf Gelbrich
# Date: 	26-July-2025
# E-mail:	orf@gelbrich.com
#
# Pupose:	The bassic idea for this code is to obtain the Solar production and then control various electrica items with in the house.
#		Meaning the Solar panels (12x500W) on the roof of the house produce a cetain amount of electricity and rather then sending
#		that back to the electric company (pays ~4c per 1KW) use it with in the house.  Result is I do not have to pay ~24c per 1KW.
#		The hosue happens to run on all electric appliances.  The water heaters are electric and the individual radiators in the 
#		rooms are electric as well.  There are 2 houses (main and guest) on the property hence certain end points only need to run 
#		when guests are present.  Further there is summer and winter (sensors) to run various radiators in certain rooms.  
#		The idea is to use the procuced electric during the day to heat the house and water heaters and turn every thing off at night.
#		Yes the electricity used at night is cheaper but I am trying to use at night as little as possible. There is an emergency
#		section in the code to over ride the radiators if certain rooms go below a pre set temperatur. This code it to further 
#		show case that certain API call can be made to the enphase local system and to the Shelly relays that are deployed to the 
#		end point locations. Hiding the little Shelly relays in the waterheater enclosure should be simple and for the various radiatos
#		it depends how big the wall box is but my plan b is the have a littel box with the relay in the supply line to the radiator 
#		with the little over ride switch to manually turn on and off the radiator. Further I am using the production and consumption
#		numbers produced by the enphase box/system, if that does not work for someone you cloud also buy a Shelly module for the braker 
#		box to obtain the same numbers. In the end this whole idea use what I produce vs. shipping it off during the day and buy back at 
#		night should work very well here in southern France.  THis may not tolly work in more nother climates and if you do not like 
#		waking up to a colder house which should warm back up during the day.  In our case during the winter we run a fireplace/stove 
#		in the evening.  Further the house is an old frensh farm house with massive walls. 
#		This should work very well in the south of france may not apply to more nothern locations.  
#
#		Program was developed with python3 on a Mac and then for production is is going to run on a Rasphery Pi (quad core, 16 GB, 256GBSD card)
#		I am using Shelly 1PM Gen4 relays and its add-on sensor package. 
#		I am also getting the temp of the sensor (which seems to run very high not sure why at this point)
#		I chose not to use the Shelly pythin lib since it was nto clear to me if that lib works with Gen 4 since the API calls all changed. 
#		To make this code work you will need the token for your local enphase system and you will need a Shelly account to get the relays set up. 
#		I also use Meraki networking and I have in the DHCP scope all the MAC addresses and IP's / hostnames preset in order to always get the 
#		same IP address.
#
#		And yes I do not code in python every day and I am sure the code could improved upon so do not complain it is atleast a good
#		starting point for you. 
#
#		As to the matrix:  
#
#		The Control column is for the general yes I want to have this item turned on or off via this program. Meaning
#		in the summer for example the radiators would be set to NO.  If there are no guests I can set the 3rd. water heater to NO as well. 
#		In the winter all or certain radiators would be set to YES.  
#
#		The MAXTime column is for how long would I like to run this item once it is detected that there is more production then consumption. 
#
#		The R. Watt column is for comparison purposes used when the consumption plus the registerd watt column are less then production and
#		and the ithem can be turnd on with out going over the production number.  This column needs to be adjusted after the C. Watt column
#		is observed on what the endpoint is truly using.  Meaning you can have a radiator where is says on the box it will use 600 Watt 
#		when in reality it may only use 550 Watt or may use 700 Watt.  
#
#		The maxtime column will run the device for that amount of time in ON mode.  This may need to be adjusted as well if you notice that
#		in the enphase GUI you see that you are running over.  Meaning you consume more then you produce and you do not check often enough. 
#		This will be a fine line in making or not making your enpoint into a diso teck situation where they contanly turn on and off. 
#
#		The Item control column is for the sensors to controll any item in the matrix.  So for example it may be easier in the water heater to 
#		also add the sensor addon and then controll the radiator in that room since the radiator electrical wall box is small.  
#
#		The Last column is for the sole purpose of not turning on the item that the previous loop iterion just turnd OFF. 



```



```
Not running on a Raspberry Pi.
Time: 2025-07-27 22:18:57.414898

+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
Current Status vor 44 sec...(2025-07-27 22:18:57.414898)
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Item | Time on | Status | Control | Index | C. Watt | Temp | Last | MaxTime | Item CTRL | R. Watt |      MAC       |          IP          |            Long Name             | 
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|  WH0 |       0 |    OFF |      NO |     0 |       0 |    0 |    - |     300 |        -- |     600 |   A085E3C76B5C |      192.168.128.200 | Water Heater down stairs         | 
|  WH1 |       0 |    OFF |      NO |     1 |       0 |    0 |    - |     200 |        -- |     600 |   A085E3C76B5C |      192.168.128.201 | Water Heater up stairs           | 
|  WH2 |       0 |    OFF |      NO |     2 |       0 |    0 |    - |     100 |        -- |     600 |   A085E3C76B5C |      192.168.128.202 | Water Heater Apartment           | 
|  R00 |       0 |    OFF |     YES |     3 |       0 |    0 |    - |     500 |        -- |     100 |   A085E3C76B5C |           10.1.0.133 | Radiator Livingroom 1            | 
|  R01 |       0 |    OFF |      NO |     4 |       0 |    0 |    - |     500 |        -- |     400 |   A085E3C76B5C |      192.168.128.204 | Radiator Livingroom 2            | 
|  R02 |       0 |    OFF |      NO |     5 |       0 |    0 |    - |     150 |        -- |      50 |   A085E3C76B5C |      192.168.128.205 | Radiator kitchen                 | 
|  R03 |       0 |    OFF |      NO |     6 |       0 |    0 |    - |     100 |        -- |     400 |   A085E3C76B5C |      192.168.128.206 | Radiator Guest Room              | 
|  R04 |       0 |    OFF |      NO |     7 |       0 |    0 |    - |     100 |        -- |     400 |   A085E3C76B5C |      192.168.128.207 | Radiator Cody Room               | 
|  R05 |       0 |    OFF |      NO |     8 |       0 |    0 |    - |     140 |        -- |     400 |   A085E3C76B5C |      192.168.128.208 | Radiator Master Bed Room         | 
|  R06 |       0 |    OFF |      NO |     9 |       0 |    0 |    - |     140 |        -- |     400 |   A085E3C76B5C |      192.168.128.209 | Radiator Master Bed Room         | 
|  R07 |       0 |    OFF |      NO |    10 |       0 |    0 |    - |     050 |        -- |     400 |   A085E3C76B5C |      192.168.128.210 | Radiator Appartment Living Room  | 
|  R08 |       0 |    OFF |      NO |    11 |       0 |    0 |    - |     050 |        -- |     400 |   A085E3C76B5C |      192.168.128.211 | Radiator Appartment Bed Room     | 
|  R09 |       0 |    OFF |      NO |    12 |       0 |    0 |    - |     300 |        -- |     400 |   A085E3C76B5C |      192.168.128.212 | Radiator Appartment Bath Room    | 
|  R10 |       0 |    OFF |      NO |    13 |       0 |    0 |    - |     070 |        -- |     400 |   A085E3C76B5C |      192.168.128.213 | Radiator Upstairs Hall Way       | 
|  R11 |       0 |    OFF |      NO |    14 |       0 |    0 |    - |     350 |        -- |     400 |   A085E3C76B5C |      192.168.128.214 | Radiator Downstairs Bath Room    | 
|  R12 |       0 |    OFF |      NO |    15 |       0 |    0 |    - |     350 |        -- |     400 |   A085E3C76B5C |      192.168.128.215 | Radiator Upstairs Bath Room      | 
|  S01 |       0 |    OFF |      NO |    16 |       0 |    0 |    - |     200 |        12 |     000 |   A085E3C76B5C |      192.168.128.216 | Sensor Apartment Bathroom        | 
|  S02 |       0 |    OFF |     YES |    17 |       0 |    0 |    - |     200 |        03 |     000 |   A085E3C76B5C |           10.1.0.133 | Sensor Living Room               | 
|  S03 |       0 |    OFF |      NO |    18 |       0 |    0 |    - |     200 |        15 |     000 |   A085E3C76B5C |      192.168.128.218 | Sensor Upstairs Bathroom         | 
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
Production:         -5 Consumption:        116 
```

