Repository for micromanager script 

The various scripts focus on stage control (X, Y, Z) and image acquisition with a single camera.

The idea of this repository is to provide a base for people that would like to automatize or control their experiment with Micromanager, using the beanshell script tool provided inside.

Various stage motion are available (X only, y Only, XY, X-Y-Z including helicoidal motion)  these motions are related to the following paper: https://opg.optica.org/oe/fulltext.cfm?uri=oe-28-19-28656 and were use for calibration of the system.
and are found in the MM1.4 folder

In the MM2.0 folder, code related to an ongoing projects are found.

One of which includes bidirectional scanning (still with the microscopy stage, here PI motors).

The idea is to perform spectral mapping, the stage scans while the light is sent to a spectrometer where the pattern is imaged on the camera, the camera acquires after the motor finished moving for a given amount of datapoints, allowing a 2D mapping of the spectra.
