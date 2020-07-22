# nircat-opto-scope
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
Setting `Exposure` longer than the MDA `Interval` will increase the acquisition interval to accomodate the longer exposure time. The true frame interval is given by the difference between frames in the metadata file generated with each OME TIFF stack under the label `"ElapsedTime-ms"`.
<a name="installation"></a> 
## Installation and Setup
