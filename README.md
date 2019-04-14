# Mentha 1.1

## Background

For the NeurotechX Fixed Challenge, MINT is building an EEG collection system from scratch, that will accurately and effectively collect scalp potentials and minimise noise.
The system will use an Arudino microcontroller to send the data aqcuired to the computer. We will then use a python script to live-plot the fourier transform of the data received. 

The UBC MINT EEG acquisition system consists of the following components:
- Differential amplifiers,
- Active analog low-pass filters,
- Analog to digital conversion,
- Digital 60 Hz notch filtering, and
- Display of the acquired EEG signals

The block diagram in Figure 1 shows the system as a whole.
![block diagram](https://github.com/UBCMint/MINT_FixedChallenge_2019/blob/master/Figures/NeuroTechX%202019%20System%20Block%20Diagram.png)

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
![Differential Amplifier](https://github.com/UBCMint/MINT_FixedChallenge_2019/blob/master/Figures/Differential%20Amplifier.png)

Figure 2. Differential amplifier circuit schematic

The positive input (AD620_IN+) is from an electrode placed on the scalp. The negative input (AD620_IN-) is from an electrode placed on a bony body part to use as a reference signal.
 
The AD620 gain is given by the following equation.

![Gain equation](https://github.com/UBCMint/MINT_FixedChallenge_2019/blob/master/Figures/Gain%20equation.png)

The AD620 is configured to have a gain of approximately 1000 by setting the single external gain resistor to 49Ω.  From this, the output signal of the AD620 (AD620_OUT) is a 1000x amplified version of the differential signal between the channel and the reference signal.

## Low Pass Filter
The low pass filter is an active, second-order Butterworth filter that attenuates frequencies above 100 Hz since the frequency range of EEG signals has a maximum of 100 Hz. The schematic of the Butterworth filter is shown in Figure 3. The OP281 is the dual operational amplifier that was chosen for the filter.

![Low pass Filter](https://github.com/UBCMint/MINT_FixedChallenge_2019/blob/master/Figures/LowpassFilter.png)

Figure 3. Butterworth low pass filter amplifier circuit schematic

The analog outputs of the differential amplifiers are filtered before being converted to digital signals.

## PCB

![PCB](https://github.com/UBCMint/MINT_FixedChallenge_2019/blob/master/Figures/PCB_resize.png){:height="50%" width="50%"}

## Analog to Digital Conversion
An Arduino Leonardo in conjunction with a Mayhew Labs extended ADC shield was chosen to perform analog to digital conversion on the amplified and filtered EEG signals. The extended ADC shield has 8 single-ended ADC inputs that can be sampled at 100 000 samples/s at a 16-bit resolution. It also allows for an input voltage range of -25 V to +25 V, enabling the direct use of the filtered EEG signal without requiring rectification.

![ADC Shield](https://github.com/UBCMint/MINT_FixedChallenge_2019/blob/master/Figures/ADC%20shield.png)

Figure 4. Mayhew Labs extended ADC shield

## Digital Notch Filter & EEG Signal Display
The digital notch filter was done using Python in conjunction with the Num.py and PyQtGraph libraries to assist with the signal processing  and signal display. The filter takes the input and converts it into a 2D array which is then run through a FFT algorithm. Then, using a simple loop to remove all entries between index 59 and 61 (a particularly noisy frequency range) via the enumerate function, we were able to create the effect of a notch filter. The signal was then plotted using the PyQt graphing functions. To calibrate the graphing function to match with real time input we have to scale the index down by a factor of 2.13 and so the actual filter index we used was a range of 27-30hz.  


## Budget
- PCB $20
- Extended ADC Shield $48
- PCB Components $170
- Arduino Leonardo $40

Total = $278

## About Us

Mentha was created by the [(Multifaceted Innovation in NeuroTechnology](https://ubcmint.github.io/) (MINT) team of undergraduate students, part of the group [Biomedical Engineering Student Team](http://www.ubcbest.com/) at the University of British Columbia in Vancouver, Canada.

Mentha was submitted as a project for the Fixed Challenge category of the [NeuroTechX 2019 Student Club competition](https://neurotechx.github.io/studentclubs/competition/).
