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
6. Open the carbogen (95% O2, 5% CO2) cylinder. Do not touch pressure regulator. Start 02 flow by opening the smaller flow valve.
7. Ensure all tubing is connected to the heater, pump, bath, and beaker containing ACSF buffer. Open the bleed valve for O2 aeration stone to begin oxygenating the ACSF beaker and then turn on the parastaltic pump for the bath (World Precision Instruments | 64-1940	DWV)
   * Using a plastic transfer pipette, transfer buffer from the main chamber of the bath on the microscope stage to the aspiration chamber to ensure steady flow. Ensure the bath level reaches equilibrium. If not, adjust the height of the aspiration needle.
   * For leaky tubing connections, apply vacuum grease when inserting smaller tubing into larger tubing to improve seal.
8. Once chamber bath is level, turn on the in-line heater and controller (Warner | TC-324C).
   * Insert the thermocouple into the bath chamber.
   * Using the thermocouple and the T2 monitor channel, check to see if the bath temperature is 32 C. If not, increase the heater temperature by manually turning the Temperature Set knob. Note: the control setpoint temperature is for the temperature as measured for solution as it leaves the inline heater and NOT the temperature at the bath. This model cannot control temperature from the thermocouple input. The solution cools dramatically as it travels to the bath, hence the reason we need to increase the heater temperature to above 32 C.
9. Once temperature has equilibrated, use a cut transfer pipette to place brain slice into the imaging chamber. Cover the slice with the harp to hold it in place.
   * The nylon strings on the harp will pull NIRCAT off the tissue. Try and avoid moving the harp once placed so that the tissue and NIRCAT remains undisturbed.
   * It helps to turn off the perfusion pump while placing the slice. However, be sure to turn it back and and wait for it to equilibrate back to temperature.
#### Software Setup
1. Open MicroManager and load the default configeration for the Ninox and Arduino.
   * You should hear the fan turn on once the config file loads. If not and you get an error, open Windows Device Manager and Disable and the re-Enable the device for the Ninox and reopen MM.
   * The config settings are setup for you and likely do not need to be adjusted for most experiments.
   * In the main MM window, set your desired exposure time and hit Tab.
2. Open Multi-D Acq. window in MM.
   * Under the Time Points box, set the number of frames and frame interval for your acqusition.
   * Set the root directory and filename. E.g. Mouse01_Slice01_ROI01_Stim01.
   * You must click **Close** in the Multi-D window for the filename and other settings to update.
   * Note: you must hit Tab or click another part of the window for any settings changes, such as exposure time, to take effect.
   * Note: Setting `Exposure` longer than the MDA `Interval` will increase the acquisition interval to accomodate the longer exposure time. The true frame interval is given by the difference between frames in the metadata file (generated with each OME TIFF stack) under the label `"ElapsedTime-ms"`. It is good practice to ensure `Interval`>`Exposure` `+ N*40ms`, where `N` is the number of Arduino state switches per frame (generally 2 to send TTL pulse). It should also be noted the USB-Serial communication is slow and each state switch to Arduino takes about 40 ms. Currently, it is required to switch states twice to trigger ThorCam, which limits frame interval times to approximately 94 ms. You can remove this switching to increase the frame rate for the Ninox at the cost of losing ThorCam triggering.
3. Click the ```Live``` button for navigating and viewing the tissue sample. 
4. Often, the 4X and 60X objectives are not concentric, meaning if the electrodes are centered in the FOV at 4X, they will be off-center and outside the field of view when you switch to 60X. To fix this, we have an ROI saved in MM to provide an overlay of where to place the electrodes at 4X so they are centered at 60X.
   * From the menu, navigate to Analyze->Tools->ROI Manager.
   * From the right button menu, click More->Open-> Then select ```ELECTRODE_LOC_OVERLAY``` located in ~\Documents\
   * Clicking ELECTRODE_LIST_OVERLAY from the list in the ROI Manager should create a yellow circular overlay indicating the position you should place electrodes at 4X.
   * If the relative centers of the objectives changes, create a new ROI using either the Square or Oval selection tool from the ImageJ menu (hold shift for circle).
   * From the menu, navigate to Image->Overlay->To ROI Manager... which will create a new ROI.
   * To save this new ROI for future overlay use, in ROI Mangaer, click More->Save and save it as ```ELECTRODE_LOC_OVERLAY```.
   **Please please please do not put tape on the LCD displays. If you must use tape, please bring your own monitors to use. Or get a thick transparent sheet to place over the screens.**
### Image Acqusition
#### Timeseries Image Acquisition with Tissue Stimulation
1. In the MM window menu, navigate to Tools -> Script Panel...
2. Open ```acquire_and_stim.bsh``` and set th
e ```STIM_FRAME``` parameter to the frame when tissue will be stimulated via electrode or laser.
3. If doing simultaneous imaging using the sCMOS, click the Record button in ThorCam (which will then wait for a trigger from MM to start acquisition).
3. To acquire a video and stimulate the tissue, click the ```Run``` button.
   * The file will automatically be named (with enumeration if doing multiple videos) and saved to disk.
   * Remember, to update the filename prefix, open the Multi-D Acq. window, change the file name, and then **Close** the window. 
#### Setting up ThorCam for Simultaneous Imaging
1. Open ThorCam and select the CS2100M-USB camera from the Play camera button dropdown menu.
2. Click Settings
   * Set desired Exposure Time (ensure it is shorter than frame interval by at least 20ms)
   * Under Hardware Trigger Settings, select Bulb (PDX) Mode to enable triggering of the camera through MM
3. Adjust contrast (LUT) by opening the Display Settings window. You can view a histogram by clicking the Histogram button.
4. Click the Setup Image Save button.
   * Select the Quick Save checkbox
   * Set Image Save to 16-bit TIFF without Annotations and select folder path and FileName prefix. Note, you will need to update the FileName in coordination with MM to keep track of your data.
   * Click Start Recording. The acquisition will then begin when the acquire_and_stim script is run in MM.
<a name="installation"></a> 

## Installation and Setup
### Microscope Construction

### Hardware Control
#### Arduino Circuit
#### Hardware Control
![hardware_connections](/images/schematic_of_control_hardware.png)

*Figure 1. Schematic depicting the connections for controling image acquisition and electrical or optical stimulation. For optogenetic stimulation, the input connection to the IsoFLEX is instead connected to the 473 nm laser.*
