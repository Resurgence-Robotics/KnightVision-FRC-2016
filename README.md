# KnightVision FRC 2016

[![Join the chat at https://gitter.im/frcteam195/KnightVision-FRC-2016](https://badges.gitter.im/frcteam195/KnightVision-FRC-2016.svg)](https://gitter.im/frcteam195/KnightVision-FRC-2016?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Drop-in vision code for FRC 2016 Stronghold target identification

Use this library to give instant vision targeting to your robot using a Kangaroo MiniPC offboard vision processor and a USB camera. We recommend the Microsoft LifeCam NX-3000. Requires some simple pre-configuration described below.

#Getting Started with KnightVision

<ol>
<li>
<h3>Kangaroo Prequirements</h3>
<p><ul>
<li>This project is dependent upon running the vision app on a <a href="http://www.kangaroo.cc/">Kangaroo MiniPC</a>. The KnightVision.exe app runs on this offboard computer and uses a USB camera and a USB to ethernet adapter to send a UDP stream of relevant data to the RoboRIO to be processed for targeting. It is designed for the <a href="http://www.amazon.com/dp/B008ZVRAQS/ref=pd_lpo_sbs_dp_ss_1?pf_rd_p=1944687762&pf_rd_s=lpo-top-stripe-1&pf_rd_t=201&pf_rd_i=B004ETQHDM&pf_rd_m=ATVPDKIKX0DER&pf_rd_r=1A5ZD9HEMA93AV7844PT">Microsoft HD3000 LifeCam</a> and the <a href="http://www.amazon.com/Cable-Matters-SuperSpeed-Gigabit-Ethernet/dp/B00BBD7NFU?ie=UTF8&psc=1&redirect=true&ref_=oh_aui_detailpage_o05_s00">AX88179 USB to Ethernet Adapter</a>.<br><br></li>

<li>
The Kangaroo must have the LabVIEW and NI Vision runtimes installed from the FRC bundled software installation in order for KnightVision.exe to run successfully. Make sure you install the license key and allow the NI Vision software to register, prior to use. Once KnightVision.exe successfully starts and sees a camera when run on the Kangaroo, proceed to step 2.
</li>

</ul></p>
</li>
<li>
<h3>Configuring the Camera</h3>
<p><ul>
<li>
First, you must calibrate the Camera Video Mode (in the KnightVisionConfig INI file). This sets the frame rate and resolution of the camera. Open NI MAX softawre. Search for your camera under installed devices. Scroll down in the properties to the Video Mode property. This is a zero-indexed array. Count the number of entries for the video mode you want to choose (remember the first entry is 0) and enter that into the CameraVideoMode property in the INI file. Our INI file particle filters are setup for 640x360 image size @ 30fps. Note that you still need to perform this step even if you wish to choose this setting as the 640x360@30fps setting may be a different video mode for your camera, so make sure you enter the proper number into your INI.<br><br>
</li>
<li>
Now you must open the KnightVision.exe app with your targeting LEDs on. Click the Save Image button and save the image as a jpg file in an easily accessible location. We recommend saving multiple images from all different areas of the field (this procedure should be performed at every competition during the vision calibration time). Once you have a good-sized data set to calibrate with, you can begin processing the images. Open the NI Vision Assistant and open the script file from our repo named NIVisionScript.vascr. Now open an image that you just saved. On the script window near the bottom of the screen, you should see a box labeled Color Threshold 1. Double click that. You should see several histograms open on the left side of the screen. Take the cursor and draw a small rectangle on the image that only covers the retro-reflective area of the target (so you are choosing this area of the image for processing in the histogram). Adjust the values in the histrogram so that all of the identified ranges are covered by the sliders ranges. Make note of the values on the screen. These are the values that will be entered into your INI file. Once you click okay, you can proceed to select the last block in the script window to ensure that the target is identified successfully. Continue processing every image in your data set and adjusting the Color Threshold values until you are satisfied with how the target is identified. Then copy these final values into the INI file.
<br><br>
</li>
<li>
Next, you must configure the camera's horizontal field of view in the INI file. This should be provided by the manufacturer's specs. Note that if the angle is given on a diagonal, you will have to convert that angle to horizontal. There are several charts available online to help assist with this, if need be. Make sure to enter this value in the INI file. You can perform a custom calibration on the cameras field of view, if you wish. A simple way to do this is to setup a proportion and modify the FOV by the deviation of the calculated target's distance from KnightVision to an actual measured value in real life. Note, if you are not able to identify the target, please perform the previous step of calibrating your color threshold values to your LEDs.<br><br>
</li>
<li>
Now that the camera is calibrated, you can make adjustments to the particle filter size and acceptable target size parameters. These parameters are able to be modified in the NI Vision Script so you can see the results. These values are really only useful if you wish to change the resolution from 640x360 to another setting. If you change the resolution, make sure to recalibrate these values, or the targets will not detect properly.<br><br>
</li>
<li>
Lastly, the AllowedDeviation and HorizontalOffset parameters in the INI must be adjusted for your robot. AllowedDeviation is a parameter that filters when the OnTarget parameter is set to true. This is the number in degrees that the robot is allowed to be +- away from the center of the target. We have found +- 0.47 deg to be an accurate value from shooting almost anywhere on the field. HorizontalOffset is used to shift the center of the image (if your camera is not centered on the robot and the target is not identifying on center correctly). This is a software adjustment for moving the center of the camera frame.<br><br>
</li>
</ul>
</p>
</li>
</ol>
