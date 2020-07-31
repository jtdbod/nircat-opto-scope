# nircat-opto-scope (NOBraINIR)
Parts, plans, code, and setup for a NIRCAT/opto SWIR imaging scope for acute brain slice physiology, also known as a Nanotube Optimized Brain Imaging in Near-Infrared (NOBraINIR) scope. Designed and built by JTDelBonisODonnell at University of California Berkeley.

## Table of Contents
* [Description](#description)
* [Standard Operation](#sop)
* [Installation and Setup](#installation)
* [File List](#filelist)
* [Parts List](#partslist)
* [Support](#support)
* [Troubleshooting](#troubleshooting)
* [Acknowledgements](#acknowledgements)
* [License](#license)

<a name="description"></a> 
## Description

<a name="sop"></a> 
## Standard Operation

### Setup
#### Tissue Preparation
1. 
#### Microscope and Perfusion Bath
1. Open the valve on the compressed air cylinder to float the table.
2. Ensure all shutters are closed.
3. Turn on the lasers by turning on the power switch and turning the key to the on position. For your saftey, ensure no light is coming out of objective during setup.
4. Turn on the following by toggling their power switch:
  * Computer (if not already on)
  * Master-8
  * Micromanipulator controller (Scientifica)
  * Stage and objective controller (Thorlabs)
5. Turn on the vacuum pump for the aspirator under the optical table. (Warner Instruments | 64-1940	DWV)
6. Ensure all tubing is connected to the heater, pump, bath, and beaker containing ACSF buffer and then turn on the parastaltic pump for the bath (World Precision Instruments | 64-1940	DWV)
  * Using a plastic transfer pipette, transfer buffer from the main chamber of the bath on the microscope stage to the aspirator chamber 
  

Setting `Exposure` longer than the MDA `Interval` will increase the acquisition interval to accomodate the longer exposure time. The true frame interval is given by the difference between frames in the metadata file (generated with each OME TIFF stack) under the label `"ElapsedTime-ms"`. It is good practice to ensure `Interval`>`Exposure` `+ N*40ms`, where `N` is the number of Arduino state switches per frame (generally 2 to send TTL pulse). It should also be noted the USB-Serial communication is slow and each state switch to Arduino takes about 40 ms. Currently, it is required to switch states twice to trigger ThorCam, which limits frame interval times to approximately 94 ms. You can remove this switching to increase the frame rate for the Ninox at the cost of losing ThorCam triggering.
<a name="installation"></a> 
## Installation and Setup
