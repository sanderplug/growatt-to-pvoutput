# Scripts directory
This directory contains the scripts which are used for capturing, processing and viewing Growatt data record information.

## Diagnosis script: view_growatt_data.sh
This script was created to get (known) data from a capture record. It has been tested on a Growatt 3600MTL,
4400TL and other models. There might be differences in where the data is stored. With this script it can be verified
if the data in the captured record corresponds with the data in the `server.growatt.com` server.

Ensure that the user variables are changed according to your local setup (e.g. CS00000000 as serial number and
1.0.0.0 / 2.0.0.0 / 3.0.0.0 as Growatt Wifi Module firmware version):
```
GROWATTSERIAL="<your Growatt inverter serial number>"
GROWATTMODULEVER=1.0.0.0
```
Execute the script with the capture record as the variable.
```
sudo /home/pvoutput/scripts/view_growatt_data.sh /home/pvoutput/processed/growatt_20140606_18\:22_55.preproc.ok
```
This will give output in the following format:
```
---------------------------------------------------------------------------------------
Mon Jun  9 12:33:50 CEST 2014
---------------------------------------------------------------------------------------
Growatt Inverter serial    (CS00000000)                  Capture sample date : 20140608
Growatt Wifi Module serial (AH00000000)                  Capture sample time : 10:53
Growatt Inverter status:    normal (1)                   Growatt temperature   37.5 C
---------------------------------------------------------------------------------------
E_Today          1.2 kWh       E_Total       1785.1 kWh       Total time    1868.0 hrs
---------------------------------------------------------------------------------------
Ppv            898.5  W        Pac            858.0  W        Fac            50.02 Hz
---------------------------------------------------------------------------------------
Vpv1           210.2  V        Vpv2           202.5  V
Ipv1             2.1  A        Ipv2             2.0  A
Ppv1           432.2  W        Ppv2           466.3  W
---------------------------------------------------------------------------------------
Vac1           238.2  V        Vac2             0.0  V        Vac3             0.0  V
Iac1             3.5  A        Iac2             0.0  A        Iac2             0.0  A
Pac1           858.0  W        Pac2             0.0  W        Pac2             0.0  W
---------------------------------------------------------------------------------------
Epvtotal      1842.4 kW
Epv1today        0.6 kW        Epv2today        0.7 kW
Epv1total      881.8 kW        Epv2total      960.6 kW
---------------------------------------------------------------------------------------
ISO Fault        0.0  V        Vpvfault         0.0  V        Tempfault        0.0  C
GFCI Fault       0.0 mA        Vacfault         0.0  V        Faultcode             0
DCI Fault        0.0  A        Facfault        0.00 Hz
---------------------------------------------------------------------------------------
IPMtemp          0.0  C        Rac              0.0 Var
Pbusvolt       394.7  V        E_Rac_today      0.0 Var
Nbusvolt         0.0  V        E_Rac_total      0.0 Var
---------------------------------------------------------------------------------------
```

##	Processing script: process_growatt_pvoutput.sh
This script is created to capture the minimum required data from a capture record that is needed to upload to pvoutput.org. The minimum data required are the date (20140607), time (09:51), energy generated in Watts – E_Today/E_Total (8800) and current power in Watts – Pac (841.9).

The script includes error handling if the data records are incorrect, but also when the upload fails.

Ensure that the user variables are changed according to your local setup:
```
PVOUTPUTKEY="<your PVOutput API key>"
PVOUTPUTSID="<your PVOutput SID>"
GROWATTSERIAL="<your Growatt inverter serial number>"
```
These two options are added to indicate if Vpv1 or Vpv1/Vpv2 need to be uploaded to PVOutput. The default options are:
```
PVOUTPUTVPV1=NO
PVOUTPUTVPV2=NO
```
The following table shows the possibilities:

||`PVOUTPUTVPV1`|`PVOUTPUTVPV2`|
|---|---|---|
|Do not upload Vpv1/Vpv2 data|`NO`|`NO`|
|Upload Vpv1 data only|`YES`|`NO`|
|Upload Vpv1 and Vpv2 data|`YES`|`YES`|

This option is used to change which power generation variable is uploaded (E_Today or E_Total). This flag adds support for the PVOutput c1 option. With this cumulative flag it is possible to use the E_Total generation number instead of the E_Today number. This can be helpful if the inverter is restarted during the day, caused by insufficient power (e.g. bad weather) or restarts during failures. Normally the E_Today counter starts at zero again, whereas E_Today keeps counting.
```
PVCUMFLAG=1
```
||`PVCUMFLAG`|
|---|---|
|Upload E_Today (default)|`0`|
|Upload E_Total|`1`|

The `DESTINATION` variable needs to be set to the fully qualified domain name (FQDN) of the Growatt server to which the data is forwarded. The default value is server.growatt.com. Normally the Raspberry Pi has eth0 as value for INTERFACE. If your Raspberry Pi has a different interface name then also change it accordingly in the file above to ensure capturing from the right ethernet device.
```
DESTINATION=server.growatt.com
INTERFACE=eth0
```
This variable is used to take into account the different data positions for later firmwares of the Growatt Wifi Module. At this moment there are two known versions (which are both supported).
```
GROWATTMODULEVER=1.0.0.0
```
||`GROWATTMODULEVER`|
|---|---|
|First Growatt Wifi Module (default)|`1.0.0.0`|
|Later release of Growatt Wifi Module|`2.0.0.0`|
|Latest release of Growatt Wifi Module|`3.0.0.0`|

The firmware version can be found in the http://server.growatt.com interface. Navigate down the tree on the left from “Plant List” to the “Plant” to the serial number of the data logger (starting with AH). The Firmware version is shown on the bottom right.
