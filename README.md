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

## Execution

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

## Code

```
#
# libraries needed
#
import sys
import requests
import json
import time
import random
import os
#import ShellyPy
import urllib3
import datetime
import re
import subprocess
import platform
import numpy as np
import socket
from pathlib import Path
import logging
import logging.handlers as handlers

# 
Line1 = "+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+"
Line2 = [ "Item" , "Time on" , "Status" , "Control" , "Index" , "C. Watt" , "Temp" , "Last" , "MaxTime" , "Item CTRL" , "R. Watt" ,  "MAC" , "IP" , "Long Name" ]
col_width = [ 4 , 7 , 6 , 7 , 5 , 7 , 4 , 4 , 7 , 9 , 7 , 14 , 20 , 32 ]

#  f"{str(item):<{col_width[i]}}"

#
matrix1 = [ 
	[ "WH0" , 0 , "OFF" , "NO" , "0" ,  "0" , "0" , "-" , "300" , "--" , "600" , "A085E3C76B5C" , "192.168.128.200" , "Water Heater down stairs        " ] ,
	[ "WH1" , 0 , "OFF" , "NO" , "1" ,  "0" , "0" , "-" , "200" , "--" , "600" , "A085E3C76B5C" , "192.168.128.201" , "Water Heater up stairs          " ] , 
	[ "WH2" , 0 , "OFF" , "NO" , "2" ,  "0" , "0" , "-" , "100" , "--" , "600" , "A085E3C76B5C" , "192.168.128.202" , "Water Heater Apartment          " ] , 
	[ "R00" , 0 , "OFF" , "YES" , "3" ,  "0" , "0" , "-" , "500" , "--" , "100" , "A085E3C76B5C" , "10.1.0.133" , "Radiator Livingroom 1           " ] , 
	[ "R01" , 0 , "OFF" , "NO" , "4" ,  "0" , "0" , "-" , "500" , "--" , "400" , "A085E3C76B5C" , "192.168.128.204" , "Radiator Livingroom 2           " ] , 
	[ "R02" , 0 , "OFF" , "NO" , "5" ,  "0" , "0" , "-" , "150" , "--" , "50" , "A085E3C76B5C" , "192.168.128.205" , "Radiator kitchen                " ] , 
	[ "R03" , 0 , "OFF" , "NO" , "6" ,  "0" , "0" , "-" , "100" , "--" , "400" , "A085E3C76B5C" , "192.168.128.206" , "Radiator Guest Room             " ] , 
	[ "R04" , 0 , "OFF" , "NO" , "7" ,  "0" , "0" , "-" , "100" , "--" , "400" , "A085E3C76B5C" , "192.168.128.207" , "Radiator Guest2 Room            " ] , 
	[ "R05" , 0 , "OFF" , "NO" , "8" ,  "0" , "0" , "-" , "140" , "--" , "400" , "A085E3C76B5C" , "192.168.128.208" , "Radiator Master Bed Room        " ] , 
	[ "R06" , 0 , "OFF" , "NO" , "9" ,  "0" , "0" , "-" , "140" , "--" , "400" , "A085E3C76B5C" , "192.168.128.209" , "Radiator Master Bed Room        " ] , 
	[ "R07" , 0 , "OFF" , "NO" , "10" , "0" , "0" , "-" , "050" , "--" , "400" , "A085E3C76B5C" , "192.168.128.210" , "Radiator Apartment Living Room " ] , 
	[ "R08" , 0 , "OFF" , "NO" , "11" , "0" , "0" , "-" , "050" , "--" , "400" , "A085E3C76B5C" , "192.168.128.211" , "Radiator Apartment Bed Room    " ] , 
	[ "R09" , 0 , "OFF" , "NO" , "12" , "0" , "0" , "-" , "300" , "--" , "400" , "A085E3C76B5C" , "192.168.128.212" , "Radiator Apartment Bath Room   " ] , 
	[ "R10" , 0 , "OFF" , "NO" , "13" , "0" , "0" , "-" , "070" , "--" , "400" , "A085E3C76B5C" , "192.168.128.213" , "Radiator Upstairs Hall Way      " ] , 
	[ "R11" , 0 , "OFF" , "NO" , "14" , "0" , "0" , "-" , "350" , "--" , "400" , "A085E3C76B5C" , "192.168.128.214" , "Radiator Downstairs Bath Room   " ] , 
	[ "R12" , 0 , "OFF" , "NO" , "15" , "0" , "0" , "-" , "350" , "--" , "400" , "A085E3C76B5C" , "192.168.128.215" , "Radiator Upstairs Bath Room     " ] , 
	[ "S01" , 0 , "OFF" , "NO" , "16" , "0" , "0" , "-" , "200" , "12" , "000" , "A085E3C76B5C" , "192.168.128.216" , "Sensor Apartment Bathroom       " ] , 
	[ "S02" , 0 , "OFF" , "YES" , "17" , "0" , "0" , "-" , "200" , "03" , "000" , "A085E3C76B5C" , "10.1.0.133" , "Sensor Living Room              " ] , 
	[ "S03" , 0 , "OFF" , "NO" , "18" , "0" , "0" , "-" , "200" , "15" , "000" , "A085E3C76B5C" , "192.168.128.218" , "Sensor Upstairs Bathroom        " ] , 
]
#
#DEBUG = True
DEBUG = False
#
globalConsumption = 0
globalProduction = 0
sleepnumbermax = 60*17     #wait 17 min
MINTEMP       = int("5")
sleepnumber   = 44					# seems to be 60 sec with all the API calls
pattern       = r"S[0-9][0-9]"				# pattern for finding the sensor in the matrix
#
RED           = '\033[31m'  				# ANSI code for bright red
GREEN         = '\033[92m'				# ANSI code for bright green
RESET         = '\033[0m'				# ANSI code to reset text formatting
YELLOW        = '\033[33m'
#
LONGNAME      = int("13")
IP            = int("12")
MAC           = int("11")
WATTRATED     = int("10")
ITEMtoCONTROL = int("9")
MAXTIME       = int("8")
LAST          = int("7")
TEMP          = int("6")
CURRENTWATT   = int("5")
INDEX         = int("4")
CONTROL       = int("3")
STATUS        = int("2")
TIMEON        = int("1")
ITEM          = int("0")
#
token = "eyJ....bla....bla....7ea4wkng"
#
# Envoy IP
#
ip = "192.168.128.63"
urlp = "https://192.168.128.63/api/v1/production"
urlc = "https://192.168.128.63/api/v1/consumption"
#
#
#
###############################################################################################################################################################
# To get local tocken ENVOY token
# http://envoy.local/home
#
#https://entrez.enphaseenergy.com/
#System: xxxx9
#SN: Gateway:4xxxxxxx9
#Token=eyJr...bla....bal...wkng
#
# Test
#
#curl -f -k -H 'Accept: application/json' -H 'Authorization: Bearer {token}' -X GET https://{my_ipaddress}/api/v1/production/inverters
#
#curl -f -k -H 'Accept: application/json' -H 'Authorization: Bearer e...bla...bla...bla..wkng' -X GET https://192.168.128.63/api/v1/production/inverters
#
#gelbrio@OrfMacBookAir ~ % curl -f -k -H 'Accept: application/json' -H 'Authorization: Bearer e..blO..bla...bla...bla...wkng' -X GET https://192.168.128.63/api/v1/production
#{
#  "wattHoursToday": 9580,
#  "wattHoursSevenDays": 224811,
#  "wattHoursLifetime": 1532475,
#  "wattsNow": 1442
#}
#gelbrio@OrfMacBookAir ~ % curl -f -k -H 'Accept: application/json' -H 'Authorization: Bearer e..blla...bla..wkng' -X GET https://192.168.128.63/api/v1/consumption
#{
#  "wattHoursToday": 2084,
#  "wattHoursSevenDays": 0,
#  "wattHoursLifetime": 496081,
#  "wattsNow": 149
#}
#
# 
#Python3 activate virtual env
#============================
#source /Users/gelbrio/A_orf/bin/activate
#
#install bumpy for matrix read/write
#pip3 install numpy
#
#see all installed python3 lib(s)
#/Users/gelbrio/A_orf/bin/python3 pip3 list
#
#execute code
#/Users/gelbrio/A_orf/bin/python3  ./savemoney.py
#
###############################################################################################################################################################
#
#
#
#
#
# SHelly Relay Examples to test with
#
#curl -v --digest http://10.1.0.133/rpc/Shelly.GetStatus     #temperature":{"tC":41.1, "tF":105.9
#curl -X POST -d '{"id":1,"method":"HTTP.GET","params":{"url":"http://10.1.0.133/rpc/Shelly.GetDeviceInfo"}}' http://10.1.0.133/rpc
#curl -X POST -d '{"id":1,"method":"Switch.Set","params":{"id":0,"on":true}}' http://10.1.0.133/rpc
#curl -X POST -d '{"id":1,"method":"Switch.Set","params":{"id":0,"on":false}}' http://10.1.0.133/rpc
#
#
#
#
def myPrintHeader(a, b):
	print(a)
	print("| " , end ="")
	i = 0
	for element in b:
		print(f"{str(element):^{int(col_width[i])}}" , end="")
		#print(f"%7s" % (element) , end="")
		print(" | " , end="")
		i += 1
	print()
	print(a)
 
def printMatrix(m):
	for row in m: 
		print("| " , end ="")
		i = 0
		for element in row:
			if DEBUG:
				print(f"{col_width[i]}")
			if element == "OFF":
				#print(f"{RED}%7s{RESET}" % (element) , end="")
				print(f"{RED}{str(element):>{int(col_width[int(i)])}}{RESET}" , end="")
			elif element == "ON":
				#print(f"{GREEN}%7s{RESET}" % (element) , end="")
				print(f"{GREEN}{str(element):>{int(col_width[int(i)])}}{RESET}" , end="")
			else:
				print(f"{str(element):>{int(col_width[i])}}" , end="")
				#print(f"%7s" % (element) , end="")
			print(" | " , end="")
			i += 1
		print()
#  f"{str(item):<{col_width[i]}}"
		

def ping_host(host):
	try:
		timeout = 1
		port = 80
		ip_address = host
		s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
		s.settimeout(timeout)
		if DEBUG:
			print(f"Trying to connect for {host}")
		# connect_ex returns 0 on success, or an error code on failure
		result = s.connect_ex((ip_address, port))
		s.close()
		if DEBUG:
			print(f"Connected to {host} Result: {result}")
			print(f"Connected to connect for {host}")
		if int(result) == int(0):
			return True
		else:
			return False
	except socket.error:
		if DEBUG:
			print(f"Unable to connect for {host}")
		return False

	"""
	Pings a given host and returns True if successful, False otherwise.
	"""
	# Determine the correct ping command arguments based on the operating system
	param = '-n' if platform.system().lower() == 'windows' else '-c'

	# Construct the ping command
	command = ['ping', param, '1', host]

	try:
		# Execute the ping command and capture the output
		# `capture_output=True` captures stdout and stderr
		# `text=True` decodes output as text
		# `check=False` prevents raising an exception for non-zero exit codes
		result = subprocess.run(command, capture_output=True, text=True, check=False)

		# Check for successful ping based on the output
		# The exact string to check might vary slightly across OS versions
		if "Reply from" in result.stdout or "bytes from" in result.stdout:
			if DEBUG:
				print(f"Ping successful for {host}")
			return True
		else:
			if DEBUG:
				print(f"Ping failed for {host}")
				print(f"Output: {result.stdout}")
				print(f"Error: {result.stderr}")
			return False
	except FileNotFoundError:
		print(f"Error: 'ping' command not found. Ensure it's in your system's PATH.")
		return False
	except Exception as e:
		print(f"An error occurred: {e}")
		return False


def getConsumption():
	urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
	#print(Line1)
	headers = {
		"Authorization": f"Bearer {token}",
		"Content-Type": "application/json"  # Example: adjust content type as needed
	}
	try:
		# Make a GET request with the specified headers
		response = requests.get(urlc, headers=headers, verify=False)
		if DEBUG:
			print(response.status_code)

		# Check if the request was successful (status code 200)
		if response.status_code == 200:
			if DEBUG:
				print("Request successful!")
				print("Response data:", response.json())
			data = response.json()
			watts_today = data['wattHoursToday']
			watts_now = data['wattsNow']
			return watts_now
		else:
			print(f"Request failed with status code: {response.status_code}")
			print("Response text:", response.text)
			return -1
	except requests.exceptions.RequestException as e:
		print(f"An error occurred: {e}")
		return -2

def getProduction():
	urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
	headers = {
		"Authorization": f"Bearer {token}",
		"Content-Type": "application/json"  # Example: adjust content type as needed
	}
	try:
		# Make a GET request with the specified headers
		response = requests.get(urlp, headers=headers, verify=False)
		if DEBUG:
			print(response.status_code)
		# Check if the request was successful (status code 200)
		if response.status_code == 200:
			if DEBUG:
				print("Request successful!")
				print("Response data:", response.json())
			data = response.json()
			watts_today = data['wattHoursToday']
			watts_now = data['wattsNow']
			return watts_now
		else:
			if DEBUG:
				print(f"Request failed with status code: {response.status_code}")
				print("Response text:", response.text)
			return -1
	except requests.exceptions.RequestException as e:
		print(f"An error occurred: {e}")
		return -2

def mySleep(num):
	print()
	print(Line1)
	for i in range(1,num+1):
		print(f"{i} ", end="", flush=True)
		time.sleep(1) 
	print()
	print(Line1)
	print()

def turnBreakerON(ip):
	if ping_host(ip):
		if DEBUG:
			print("Turning Item %s on..." % ip)
		data = '{"id":1,"method":"Switch.Set","params":{"id":0,"on":true}}' 
		URLwithIP = "http://" + ip + "/rpc"
		urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
		try:
			# Make post to turn off
			if DEBUG:
				print(URLwithIP)
			response = requests.post(URLwithIP, data=data, verify=False)
			if DEBUG:
				print(response.status_code)
			# Check if the request was successful (status code 200)
			if response.status_code == 200:
				if DEBUG:
					print("Request successful!")
					print("Response data:", response.json())
				pass
			else:
				pass
				if DEBUG:
					print(f"Request failed with status code: {response.status_code}")
					print("Response text:", response.text)
		except requests.exceptions.RequestException as e:
			print(f"An error occurred: {e}")
	else:
		if DEBUG:
			print(f"Can not ping {ip} Error in Matrix?...")
		pass


def turnBreakerOFF(ip):
	if ping_host(ip):
		if DEBUG:
			print("Turning Item %s on..." % ip)
		data = '{"id":1,"method":"Switch.Set","params":{"id":0,"on":false}}' 
		URLwithIP = "http://" + ip + "/rpc"
		urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
		try:
			# Make post to turn off
			if DEBUG:
				print(URLwithIP)
			response = requests.post(URLwithIP, data=data, verify=False)
			if DEBUG:
				print(response.status_code)
			# Check if the request was successful (status code 200)
			if response.status_code == 200:
				if DEBUG:
					print("Request successful!")
					print("Response data:", response.json())
				pass
			else:
				pass
				if DEBUG:
					print(f"Request failed with status code: {response.status_code}")
					print("Response text:", response.text)
		except requests.exceptions.RequestException as e:
			print(f"An error occurred: {e}")
	else:
		if DEBUG:
			print(f"Can not ping {ip} Error in Matrix?...")
		pass

def getItemConsumption(ip):
	if DEBUG:
		print("Get Item: %s  Cosumption ..." % ip)
	if ping_host(ip):
		URLwithIP = "http://" + ip + "/rpc/Shelly.GetStatus"
		urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
		try:
			if DEBUG:
				print(URLwithIP)
			response = requests.get(URLwithIP, verify=False)
			if DEBUG:
				print(response.status_code)
			# Check if the request was successful (status code 200)
			if response.status_code == 200:
				if DEBUG:
					print("Request successful!")
					print("Response data:", response.json())
				data = response.json()
				apower = data['switch:0']['apower']
				if DEBUG:
					print(f"Watt: {apower}")
				return (apower)
			else:
				if DEBUG:
					print(f"Request failed with status code: {response.status_code}")
					print("Response text:", response.text)
				return (-100)

		except requests.exceptions.RequestException as e:
			print(f"An error occurred: {e}")
			return (-200)
	else:
		if DEBUG:
			print(f"Can not ping {ip} Error in Matrix?...")
		return (-300)

def getItemTemp(ip,rr):
	#
	# get Temperatur of Item
	#
	if DEBUG:
		print("Get Item: %s  Temp ..." % ip)
	if ping_host(ip):
		URLwithIP = "http://" + ip + "/rpc/Shelly.GetStatus"
		urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
		try:
			if DEBUG:
				print(URLwithIP)
			response = requests.get(URLwithIP, verify=False)
			if DEBUG:
				print(response.status_code)
			# Check if the request was successful (status code 200)
			if response.status_code == 200:
				if DEBUG:
					print("Request successful!")
					print("Response data:", response.json())
				data = response.json()
				#if ture must be a sensor
				#making sure it is a sensor in the matrix
				if data['temperature:100']['tC'] and re.match(pattern, matrix1[int(rr)][ITEM]):
					temperatureC100 = data['temperature:100']['tC']
					if DEBUG:
						print(f"Temperature in C: {temperatureC100}")
					return (temperatureC100)
				if data['switch:0']['temperature']['tC']:
					temperatureC = data['switch:0']['temperature']['tC']
					if DEBUG:
						print(f"Temperature in C: {temperatureC}")
					return (temperatureC)
			else:
				if DEBUG:
					print(f"Request failed with status code: {response.status_code}")
					print("Response text:", response.text)
				return (100)

		except requests.exceptions.RequestException as e:
			print(f"An error occurred: {e}")
			return (200)
	else:
		if DEBUG:
			print(f"Can not ping {ip} Error in Matrix?...")
		return (300)

def write2file(mystring):
	f=open('./solar.log', 'a')
	f.write(mystring)
	f.close()

def writeMatrix2file(x):
	matrix2 = np.matrix(x)
	array_string = np.array2string(matrix2,
                               precision=2,        # Set decimal precision to 2
                               separator=', ',     # Separate elements with comma and space
                               max_line_width=200)  # Wrap lines if they exceed 80 characters
	# Write the string to a text file

	with open('solarmatrix.txt', 'w') as f:
		f.write(array_string)
	#

def displayMatrix(m,l1,l2,s,n):
	print()
	print(l1)
	print("Current Status vor %d sec...(%s)" % (s,n))
	print(l1)
	myPrintHeader(l1,l2)
	printMatrix(m)
	print(l1)


def is_raspberry_pi():
	"""
	Checks if the current system is a Raspberry Pi.
	"""
	cpuinfo_path = Path("/proc/cpuinfo")
	if not cpuinfo_path.exists():
		return False

	with open(cpuinfo_path) as f:
		cpuinfo = f.read()
		return re.search(r"^Model\s*:\s*Raspberry Pi", cpuinfo, flags=re.M) is not None

def whatos():
	if is_raspberry_pi():
		print("Running on a Raspberry Pi.")
		t = get_raspberry_pi_temperature()
		return t
	else:
		print("Not running on a Raspberry Pi.")
		return None

def get_raspberry_pi_temperature():
	"""
	Executes 'vcgencmd measure_temp' and extracts the temperature.
	Returns the temperature in Celsius as a float, or None if an error occurs.
	"""
	try:
		# Execute the command and capture its output
		result = subprocess.run(['vcgencmd', 'measure_temp'], capture_output=True, text=True, check=True)
		output = result.stdout
		print(f"Origional outout: {output}")
		
		# Use a regular expression to extract the numerical temperature value
		match = re.search(r"[-+]?\d*\.?\d+([eE][-+]?\d+)?", output)
		print(f"Match output: {match}")
		print(f"Match group0 output: {match.group(0)}")
		print(f"Match group1 output: {match.group(1)}")
		if match:
			temperature_str = match.group(0)
			print(f"Temperature of PI is: {temperature_str}")
			return float(temperature_str)
		else:
			print(f"Could not parse temperature from: {output}")
			return None
	except subprocess.CalledProcessError as e:
		print(f"Error executing vcgencmd: {e.stderr}")
		return None
	except FileNotFoundError:
		print("vcgencmd command not found. Ensure it's installed and in your PATH.")
		return None


def main():
	logger = logging.getLogger('solar_orf')
	logger.setLevel(logging.INFO)

	log_file_path = 'solar_info.log'
	max_bytes = 512000 # 1 KB for demonstration
	backup_count = 3

	file_handler = handlers.RotatingFileHandler(
		log_file_path,
		maxBytes=max_bytes,
		backupCount=backup_count
	)

	formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')
	file_handler.setFormatter(formatter)

	logger.addHandler(file_handler)

	PItemp = whatos()
	if PItemp is not None:
		logger.info(f"Raspberry Pi CPU Temperature: {PItemp}°C")
		print(f"Raspberry Pi CPU Temperature: {PItemp}°C")
	#
	# Write matrix to file
	#
	writeMatrix2file(matrix1)
	logger.info(f"{matrix1}")
	#
	rows = len(matrix1)
	cols = len(matrix1[0])
	#
	# the forever loop
	#
	while 5>4:   # do forever until ctrl-C
		#
		# Get the current date and time
		#
		now = datetime.datetime.now()
		# Print the full datetime object
		print(f"Time: {YELLOW}%s{RESET}" % (now))
		if PItemp is not None:
			logger.info(f"Raspberry Pi CPU Temperature: {PItemp}°C")
			print(f"Raspberry Pi CPU Temperature: {PItemp}°C")

		# Print only the time component
		if DEBUG:
			print(now.time())
			# Print the time in a specific format (e.g., HH:MM:SS)
			print(now.strftime("%H:%M:%S"))
		logger.info(f"{now.time()}")
		#
		# print Matrix
		#
		displayMatrix(matrix1,Line1,Line2,sleepnumber,now)
		#
		#
		# Run though the matrix rows 
		#
		globalProduction = getProduction()
		globalConsumption = getConsumption()
		#
		#
		# Get initial status of ENVOY
		#
		write2file(f"{now} Production: {globalProduction} Consumption: {globalConsumption}\n")
		#
		if int(globalProduction) > int(globalConsumption):
			print("Production: %10s Consumption: %10s " % ( globalProduction ,  globalConsumption))
		else:
			print(f"Production: {RED}%10s{RESET} Consumption: %10s " % ( globalProduction ,  globalConsumption))
		#
		# start loop through every element in matrix
		#	
		for r in range(rows):
			#
			# Getting temerature of item
			#
# this call results in every item a connection check!!!!!!!!!!!! Slow... 
			matrix1[r][TEMP] = getItemTemp(matrix1[r][IP],matrix1[r][INDEX])
			if matrix1[r][CONTROL] == "YES" and int(matrix1[r][TEMP]) <= int(MINTEMP):
				#
				# Emergency turn on Radiators regless of production
				#
				print(f"{RED}Emergency{RESET} Turning ON %s (min temp: %s)" % (matrix1[r][LONGNAME],MINTEMP))
				matrix1[r][STATUS] ="ON" 
				turnBreakerON(matrix1[r][IP])
				matrix1[r][LAST] = "-" #no Last on Emergency
				matrix1[r][TIMEON] = "0" #no timer on Emergency
				#
				# Check Temperature on sensor
				#
				if re.match(pattern, matrix1[r][ITEM]):
					print(f"Pattern Matched: %s -> %s " % (matrix1[r][ITEM] , matrix1[r][ITEMtoCONTROL]))
					rr = int(matrix1[r][ITEMtoCONTROL])
					matrix1[rr][STATUS] ="ON" 
					turnBreakerON(matrix1[rr][IP])
					matrix1[rr][LAST] = "-" #no Last on Emergency
					matrix1[rr][TIMEON] = "0" #no timer on Emergency
					print(f"{RED}From Sensor Emergency{RESET} Turning ON %s " % (matrix1[rr][LONGNAME]))
				#
			else:
				#
				# Check if we need to manage this Item at this time
				#
				if matrix1[r][CONTROL] == "YES" and matrix1[r][7] != "L":
					globalProduction = getProduction()
					globalConsumption = getConsumption()
					write2file(f"{now} Production: {globalProduction} Consumption: {globalConsumption} + {matrix1[r][WATTRATED]}\n")
					if int(globalProduction) > int(globalConsumption) + int(matrix1[r][WATTRATED]):
						print("Production: %10s Consumption: %10s " % ( globalProduction ,  globalConsumption))
						print(f"Checking if {YELLOW}%s{RESET} status can be changed?" %  (matrix1[r][LONGNAME]))
						if matrix1[r][STATUS] == "ON":
							print("Item %s is alread on - nothing to do" % (matrix1[r][LONGNAME]))
							matrix1[r][TIMEON] = str(int(matrix1[r][TIMEON]) + int(sleepnumber))
						else:
							print("Turning ON %s " % (matrix1[r][LONGNAME]))
							matrix1[r][STATUS] ="ON" 
							turnBreakerON(matrix1[r][IP])
							matrix1[r][CURRENTWATT] = getItemConsumption(matrix1[r][IP]) 
					else:
						if matrix1[r][STATUS] == "OFF":
							print("Item %s is alread off - nothing to do" % (matrix1[r][LONGNAME]))
						else:		
							print("Turning OFF %s " % (matrix1[r][LONGNAME]))
							matrix1[r][STATUS] ="OFF" 
							turnBreakerOFF(matrix1[r][IP])
							matrix1[r][TIMEON] = "0"
							matrix1[r][CURRENTWATT] = "0"
							matrix1[r][LAST] = "L"
			#
			# reached max time
			#
			if int(matrix1[r][TIMEON]) >= int(matrix1[r][MAXTIME]):
				matrix1[r][LAST] = "L"
				matrix1[r][TIMEON] = "0"
				matrix1[r][CURRENTWATT] = "0"
				print("Turning OFF %s max time reached %s.......%s.........!!!" % (matrix1[r][LONGNAME] , matrix1[r][TIMEON] , matrix1[r][MAXTIME]))
				matrix1[r][STATUS] ="OFF" 
				turnBreakerOFF(matrix1[r][IP])
			else:
				pass
		#
		# end loop
		#
		#displayMatrix(matrix1,Line1,Line2,sleepnumber,now)
		#
		# Rest the Last column (exclude temp sensors )
		#
		x = 0
		y = 0
		for r in range(rows):
			if matrix1[r][LAST] == "L" and matrix1[r][CONTROL] == "YES":
				x += 1
			else:
				if matrix1[r][CONTROL] == "NO":
					y += 1
		#
		# if all rows have L then reset everything
		#
		if int(len(matrix1)) == int(x) + int(y):
			print(" im here.....%s  ==  %s + %s "  % (int(len(matrix1)) , int(x) ,  int(y)))
			for r in range(rows):
				if matrix1[r][LAST] == "L" and matrix1[r][CONTROL] == "YES":
					matrix1[r][LAST] = "-"
		else:
			mySleep(sleepnumber)

		os.system('clear')

# call main
main()



```


