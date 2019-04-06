# Mentha 1.1

## Background

For the NeurotechX Fixed Challenge, MINT is building an EEG collection system from scratch, that will accurately and effectively collect scalp potentials and minimise noise.
The system will use an Arudino microcontroller to send the data aqcuired to the computer. We will then use a python script to live-plot the fourier transform of the data recieved. 

The UBC MINT EEG acquisition system consists of the following components:
- Differential amplifiers,
- Active analog low-pass filters,
- Analog to digital conversion,
- Digital 60 Hz notch filtering, and
- Display of the acquired EEG signals

The block diagram in Figure 1 shows the system as a whole.
![block diagram](https://github.com/UBCMint/MINT_FixedChallenge_2019/blob/master/NeuroTechX%202019%20System%20Block%20Diagram.png)

Figure 1. System block diagram


We made our design choices based on availability and affordabiliity such that other teams can recreate our system. 

## Usage
This four channel EEG collection system can be easily set up and used. The scripts can be downloaded from the github repository and can be run with any Python interpreter. The four electrodes should be clipped onto the hair and placed closely against the scalp. Alternatively, gel electrodes can be placed on the forehead. 
The four electrode signals will be processed through the circuit and received by the Arduino Leo pins A0-A3. The 4 signals will be fourier transformed and displayed on the computer through a python plotting script. 

**Important**: Remember to change the port name to what you see in your Arduino IDE (e.g. "Arduino Leonardo on COM4") and to run the correct Arduino program for whichever script you're using (2Reads for 2Time2FFT.py, 4Reads for 4Time.py and 4FFT.py)

## Electrodes
The chosen electrodes are gel electrodes, chosen for their ease of accessibility and use.

## Differential Amplifier
The chosen differential amplifier used in each channel is the AD620 instrumental amplifier. The AD620 is chosen for its low cost and high accuracy. Furthermore, it has low noise, low input bias current, and low power consumption characteristics.

The circuit schematic for the differential amplifier is shown in Figure 2 below.
![Differential Amplifier](https://github.com/UBCMint/MINT_FixedChallenge_2019/blob/master/Differential%20Amplifier.png)


## Software:
We are using python to collect data from an Arduino and plotting the fourier transform of the the EEG signal in realtime. This allows us to see the peaks in amplitude of the different frequencies of brain signals.

![Python GUI](https://raw.githubusercontent.com/UBCMint/FixedChallenge/master/PythonGUI/screenshots/plot.PNG)

We chose to use pyqtgraph instead of matplotlib to improve the speed of live plotting. We originally prototyped with Matlab, but found that Python provided a better GUI and realtime plotting.
As seen in the above figure, the top two graphs depict the original sine wave and the bottom two graphs depict the transformed graph. The script can be easily changed to display 4 FFT plots from 4 channels.

Some challenges we had in designing this script was sending the signals through the serial port and making sure python was able to designate the right label to each signal. For example, when a signal from a single channel was missed such as A1, the remaining signals would shift and the previous
A2 would become A1. This was solved by sending all four signals in a string from the Arduino and then parsing in Python accordingly. 

### Limitations
The limitations of the software application include the number of channels that acquire data and the delay in the real-time plotting.

To increase the number of channels, we could use more of the pins available on the Arduino. This can be easily added into the python script, albeit we are limited to the 
number of pins on the Arduino or microcontroller we are using. In the future, we could add more interative components to our system, such as a GUI that allows the user to start and stop data acquisition. Another improvement would include the ability to save data and retrieve previous data. These
extra features could also be implemented through a phone app.

## Budget

## About Us

Mentha was created by the [(Multifaceted Innovation in NeuroTechnology](https://ubcmint.github.io/) (MINT) team of undergraduate students, part of the group [Biomedical Engineering Student Team](http://www.ubcbest.com/) at the University of British Columbia in Vancouver, Canada.

Mentha was submitted as a project for the Fixed Challenge category of the [NeuroTechX 2019 Student Club competition](https://neurotechx.github.io/studentclubs/competition/).
