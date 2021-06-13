# Growatt inverter data to PVouput via Raspberry Pi
When starting with solar panels in 2013, I have searched for a workable solution to upload the production data from my Growatt 3600MTL (dual tracker) inverter to PVoutput. Up till then, most solutions documented on the internet were around the Growatt Bluetooth adapter. Or it involved a USB to RS232 serial cable in combination with a Raspberry Pi closely placed to the Growatt inverter.

After finding starting points, I wrote the required code to be able to correctly capture the data for my inverter and perform the required error checking. I completely documented the code as well as the setup on a Raspberry Pi and published this on Vereniging Eigenhuis Energy Community webpages. That blog post had accumulated over 22k views and over 500 comments, until the whole community was discarded off unexpectedly.

![alt text](https://github.com/sanderplug/growatt-to-pvoutput/raw/main/docs/Growatt%20and%20Rasberry%20Pi%20to%20PVOutput%20diagram.jpg "Architecture Overview Diagram")
*Architecture overview diagram of automatic upload from Growatt inverter, via Raspberry Pi, to Growatt servers and PVoutput.org.*

The code includes dynamic data analysis (to find the right offset position), error handling and to create a full step-by-step description to allow others to perform the installation and configuration. A separate script is included to view all data within the captured Growatt record. This is useful for debugging purposed or to further extend the capture script in the future to store additional data into a local data store.

This version is known to work with Growatt Wifi Modules up to version 4. It does not support the newer versions of the Growatt adapters (Shine Wifi-S and ShineWifi-X) as these have encrypted/masked data records. The comments of the Energy Community had this code added by one of the users, which unfortunately was not added to the main document.

It is assumed that the Growatt Wifi Module is currently configured correctly into the local network and that the upload to the Growatt servers is working correctly.
