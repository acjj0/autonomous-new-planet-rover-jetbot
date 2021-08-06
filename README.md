# autonomous-rover

* Final Report: PDF [JiajiaChen - Final Report - Autonomous New Planet Rover.pdf](https://github.com/acjj0/autonomous-rover/blob/main/JiajiaChen%20-%20Final%20Report%20-%20Autonomous%20New%20Planet%20Rover.pdf)
* Slides: PDF [JiajiaChen - Final Presentation - Autonomous New Planet Rover.pdf](https://github.com/acjj0/autonomous-rover/blob/main/JiajiaChen%20-%20Final%20Presentation%20-%20Autonomous%20New%20Planet%20Rover.pdf)
* Demo videos: https://www.youtube.com/watch?v=3tTfUyWM6wI
* Code: 3 ipynb files - rover_data_collection.ipynb, rover_logic.ipynb, rover_model_training.ipynb
* Data: file dataset.zip
* Instructions how to install and use the software: Section 4 (pasted below for quick reference) in the [Report](https://github.com/acjj0/autonomous-rover/blob/main/JiajiaChen%20-%20Final%20Report%20-%20Autonomous%20New%20Planet%20Rover.pdf) goes over hardware, software, development tools, infrastructure, setup, data collection, ML & Deep Learning Models, Challenges to expect, and Troubleshooting.


Hardware, Software and Development Tools
This is a physical prototype. It is important to pay careful attention to the hardware and infrastructure, software, setup, data collection, and model training to get this functioning right.

4.1 Hardware & Infrastructure - Minimum Requirements
- Jetson Nano 4GB 
- Waveshare Jetbot AI Kit with single camera
- SD card with at least 64GB
- Laptop
- Ethernet wired & WiFi Network
- Monitor, keyboard, mouse
- Physical test environment for the rover to explore

4.2 Software - Minimum Requirements
- Jetbot latest image from Nvidia / Waveshare 
- Jupyter Lab server running on the Jetbot which also offers Python and its standard libraries 
- Balena Etcher software on the laptop to write the image to the SD card
- Ubuntu/Windows OS on laptop, and browser (Chrome, Firefox, etc)
- Torch, OpenCV, Numpy, etc Python libraries on the Jetbot via Jupyter Lab

4.3 Project Setup 
- Following the guidance at the following pages, obtain the Jetbot image and write it to the SD card using Balena Etcher https://github.com/NVIDIA-AI-IOT/jetbot/wiki/Create-SD-Card-Image-From-Scratch and https://www.waveshare.com/wiki/JetBot_AI_Kit 
- The Balena Etcher software worked better on Windows, Ubuntu has more troubleshooting capabilities for issues with Jetbot
- Assemble the Waveshare kit into a Jetbot using instructions here https://www.waveshare.com/wiki/JetBot_AI_Kit_Assemble_Manual 
- Connect the Jetbot to a monitor, keyboard and mouse, and connect a wired connection for the Jetbot. 
- Update all Ubuntu software on the Jetbot and set up a known username and password
- Set up the WiFi connection for the Jetbot to operate untethered
- Make sure the Jetbot’s IP address shows up on the little Waveshare OLED screen for both the wired and wireless connections
- Detach the wired connection and make sure that the Jetbot still gets a wireless connection when untethered when turned off and on
- Once the wireless connection works on the Jetbot, from a laptop computer go to http://<jetbot_ip_address>:8888 and enter the Jetbot’s username and password
- Try the Nvidia examples starting with basic motion to make sure that the Jetbot works
- For coding, the Jupyter Lab site provides all functions and serves as a development environment with Python included. To install additional libraries, either do so from the Jetbot setup above with a monitor, keyboard and mouse, or type ! followed by the terminal command, such as:
!pip install keras
- To use my code, download from Github my repository. Transfer to the Jetbot by drag-drop into Jupyter Lab. First run all cells in rover_model_training.ipynb, which will generate the dataset folder and the model best_model.pth on the Jetbot. Download ssd_mobilenet_v2_coco.engine from https://jetbot.org/master/examples/object_following.html and place it in the same folder as the ipynb files. Set up your physical environment and place the Jetbot where you wish to test the rover’s autonomous features. Run all cells in rover_logic.ipynb, except the final cell which stops the Jetbot. To change from teddy bear to another class, find its number in ms-coco-labels.txt and enter the new number in rover_logic.ipynb in the text area (not in the code) where you see the number 88 currently.

4.4 Data Collection
250 images of 2 classes - 
- Blocked = Jetbot will hit the object in the image if it moves forward
- Free = Jetbot can safely move forward

4.5 Machine Learning and Deep Learning Models
I used the following two models: 
- AlexNet - To perform transfer learning to train 2 classes of Obstacle Avoidance
- COCO - To perform object identification into generic classes, rather than specific object names, as would be the case with an unexplored planet or moon

4.6 Changes to The Plan
The project was not smooth sailing from start to finish. 
- Given sufficient time, a rover on an unexplored planet or moon could theoretically explore all accessible parts of that planet with random motion. Rovers don’t have an infinite lifetime and they also need to weather harsh conditions that limit their lifetime. To help the rover more efficiently complete its mission, I added intentional navigation towards objects of interest identified from long distances. To mimic this use-case, I added to my goals object identification, detection of interesting objects, and steering intentionally towards such objects to explore them further. To implement this use-case, I did object identification, motion or direction planning, and object following, in addition to collision avoidance.
- I had intended to teach the rover not only collision avoidance, but also fall avoidance, such as over a cliff or over the edge of a crater. However, with only 1 camera in the Waveshare kit and no other sensors, the training data and the live images have no relative altitude information for points within the image. Monocular depth perception lacks such altitude information too as the amount of floor visible above the bottom edge of the image does not reflect the relative altitude of different parts of that floor. That amount of floor visible at the edge of a crater can appear to be sufficient to move forward if the soil looks the same outside and inside a crater. Note that the term depth in monocular depth perception does not equal altitude, rather it is about depth in perspective images. With edge detection and additional sensors including more cameras, such altitude perception could be improved. However, without additional sensors, I decided to set aside fall prevention for future work. 
- The Sparkfun 2.1 kit was a lot of work to assemble. The chassis in the kit has fastening holes that do not line up with the holes in the Nano. I have no intention of drilling holes into the Nano. Next, screws that came with the Sparkfun kit could not even go into the holes for the Nano. The Sparkfun’s OLED screen never turned on. Even if it had turned on, it could not provide an IP address because the WiFi module that came with the Sparkfun kit has device drivers for Windows and Mac, but not for Linux which runs on the Nano. To hack through any of these incompatibilities, I would need an HDMI connection to the monitor, but the HDMI was intermittent and unreliable. I abandoned plans of using the Sparkfun kit.

4.7 Troubleshooting
- Make sure that the Nvidia examples work with the Jetbot 
- Make sure that the SD card is at least 64GB and is rated U3 V30 or close to such read/write speeds to avoid storage bottlenecks
- The Jetbot must operate close enough to your WiFi router to have a good connection and avoid network bottlenecks.
- Use a wired connection to troubleshoot camera lag issues
- Reduce frames per second (fps) to 10 or lower if sufficient for your application, as higher fps overloads the Jetson Nano processor, memory and network stack
- Reduce motor speeds and turn gain to make sure the movement of the Jetbot is at a speed manageable for bottlenecks with the system, but set it high enough to generate sufficient torque for the given surface; I needed motors to run at least at 0.2 to generate sufficient torque on my carpeted surface
