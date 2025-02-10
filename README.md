# Autonomous Drone Development Challenge (ADDC) 2024

This repository contains the approach used by aeroKLE for SAEISS ADDC 2024, KCG College of Technology held from 16th July 2024 to 18th July 2024

# Setting up Raspberry Pi 4 Model B

1. Using 'Rasbperry Pi Imager', install Raspberry Pi OS compatible with the Raspberry Pi 4 (Recommended: Raspberry Pi OS (Debian Bullseye) Legacy 32-Bit Full with Desktop environment and recommended applications) onto the SD Card (Recommended: Class 10 32 GB Micro SD Card).

2. Access the Raspberry Pi through Wi-Fi via SSH
  
3. Set up serial connection and type the following in SSH:
```
sudo raspi-config
```
4. Change the folowing settings:

a) Go to interface settings

b) Enable Legacy camera

c) Enable SSH

d) Enable VNC

e) Go to serial

f) When prompted, select no to 'Would you like a login shell to be accessible over serial?'

g) When prompted, select yes to 'Would you like the serial port hardware to be enabled?'.

h) Reboot the Raspberry Pi using ```sudo reboot``` when you are done.

i) The Raspberry Pi’s serial port will now be usable on ```/dev/ttyAMA0```.

If you encounter ```Cannot currently show the Desktop```, go to ```sudo nano /boot/config.txt``` and type the following lines after ```#hdmi_safe=1```:
```
hdmi_force_hotplug=1
hdmi_group=2
hdmi_mode=9
```
Save the file and exit the text editor (in ```nano```, you do this by pressing CTRL + X, then Y, and Enter).

4. Run the following commands:
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python3-pip
sudo apt-get install python-dev
sudo pip install future
sudo apt-get install screen
sudo apt-get install python-wxgtk4.*
sudo apt-get install libxml2-dev
sudo apt-get install libxslt1-dev
pip install lxml
sudo pip3 install pyserial
sudo pip3 install dronekit
sudo pip3 install geopy
sudo pip3 install MAVProxy
```

6. Install OpenCV:
```
pip install opencv-python
```
NOTE: Refer to the troubleshooting section if any errors are raised.


# Integrate Raspberry Pi with PixHawk
1. Set following parameters in Mission Planner:


```SERIAL2_PROTOCOL = 2```

```SERIAL2_BAUD = 921```

```LOG_BACKEND_TYPE = 3```



2. Connect PixHawk and Raspberry Pi as shown in the figure:

![f837b6b1116ec02c3490e34035c2f09da5a62936](https://github.com/vvpai9/OpenCV-Detection/assets/162291797/f1a79c68-ce5e-46aa-9fdd-fc60bfb1db5b)

3. Power the Raspberry Pi using BEC module.

a) Check port
```
ls /dev/ttyAMA0
```


b) Add the following two lines at bottom of file ```sudo nano /boot/config.txt``` :
```
enable_uart=1
dtoverlay=disable-bt
```
Save the file and exit the text editor (in ```nano```, you do this by pressing CTRL + X, then Y, and Enter).

4. Now type the following to get the telemetry data of PixHawk:
```
mavproxy.py --master=/dev/ttyAMA0 --baudrate 921600
```

5. Type the following if you want telemetry data to be displayed in Mission Planner:
```
mavproxy.py --master=/dev/serial0 --baudrate 921600 --out udp:127.0.0.1:14552

/*Here,
 '127.0.0.1' Your PC's IP Adress, Obtained by typing 'ipconfig' in command prompt
 '14552' is the port to which you need to connect to Mission Planner using UDP
*/
```
# Executing the mission

To run Python code:
In Terminal 1, run:
```
python3 optimise.py
```

In Terminal 2, run:
```
python3 mission.py --connect /dev/ttyAMA0
```

# Troubleshooting
If the following error arises during the installation of OpenCV
```
Defaulting to user installation because normal site-packages is not writeable
Looking in indexes: https://pypi.org/simple, https://www.piwheels.org/simple
Collecting opencv-python
  Using cached opencv-python-4.9.0.80.tar.gz (92.9 MB)
  Installing build dependencies ... error
  error: subprocess-exited-with-error
  
  × pip subprocess to install build dependencies did not run successfully.
  │ exit code: 1
  ╰─> [84 lines of output]
      Looking in indexes: https://pypi.org/simple, https://www.piwheels.org/simple, https://www.piwheels.org/simple
      Ignoring numpy: markers 'python_version == "3.6" and platform_machine != "aarch64" and platform_machine != "arm64"' don't match your environment
      Ignoring numpy: markers 'python_version == "3.7" and platform_machine != "aarch64" and platform_machine != "arm64"' don't match your environment
      Ignoring numpy: markers 'python_version == "3.8" and platform_machine != "aarch64" and platform_machine != "arm64"' don't match your environment
      Ignoring numpy: markers 'python_version <= "3.9" and sys_platform == "darwin" and platform_machine == "arm64"' don't match your environment
      Ignoring numpy: markers 'python_version == "3.9" and platform_machine != "aarch64" and platform_machine != "arm64"' don't match your environment
      Ignoring numpy: markers 'python_version == "3.10" and platform_system != "Darwin"' don't match your environment
      Ignoring numpy: markers 'python_version == "3.10" and platform_system == "Darwin"' don't match your environment
      Ignoring numpy: markers 'python_version >= "3.11"' don't match your environment
      Collecting cmake>=3.1
        Using cached cmake-3.29.0.1.tar.gz (30 kB)
        Installing build dependencies: started
        Installing build dependencies: finished with status 'done'
        Getting requirements to build wheel: started
        Getting requirements to build wheel: finished with status 'done'
        Installing backend dependencies: started
        Installing backend dependencies: finished with status 'error'
        error: subprocess-exited-with-error
      
        × pip subprocess to install backend dependencies did not run successfully.
        │ exit code: 2
        ╰─> [52 lines of output]
            Looking in indexes: https://pypi.org/simple, https://www.piwheels.org/simple, https://www.piwheels.org/simple, https://www.piwheels.org/simple
            Collecting cmake>=3.15
              Using cached cmake-3.29.0.1.tar.gz (30 kB)
            ERROR: Exception:
            Traceback (most recent call last):
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/cli/base_command.py", line 180, in exc_logging_wrapper
                status = run_func(*args)
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/cli/req_command.py", line 245, in wrapper
                return func(self, options, args)
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/commands/install.py", line 377, in run
                requirement_set = resolver.resolve(
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/resolution/resolvelib/resolver.py", line 95, in resolve
                result = self._result = resolver.resolve(
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_vendor/resolvelib/resolvers.py", line 546, in resolve
                state = resolution.resolve(requirements, max_rounds=max_rounds)
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_vendor/resolvelib/resolvers.py", line 397, in resolve
                self._add_to_criteria(self.state.criteria, r, parent=None)
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_vendor/resolvelib/resolvers.py", line 173, in _add_to_criteria
                if not criterion.candidates:
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_vendor/resolvelib/structs.py", line 156, in __bool__
                return bool(self._sequence)
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/resolution/resolvelib/found_candidates.py", line 155, in __bool__
                return any(self)
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/resolution/resolvelib/found_candidates.py", line 143, in <genexpr>
                return (c for c in iterator if id(c) not in self._incompatible_ids)
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/resolution/resolvelib/found_candidates.py", line 47, in _iter_built
                candidate = func()
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/resolution/resolvelib/factory.py", line 182, in _make_candidate_from_link
                base: Optional[BaseCandidate] = self._make_base_candidate_from_link(
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/resolution/resolvelib/factory.py", line 228, in _make_base_candidate_from_link
                self._link_candidate_cache[link] = LinkCandidate(
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/resolution/resolvelib/candidates.py", line 290, in __init__
                super().__init__(
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/resolution/resolvelib/candidates.py", line 156, in __init__
                self.dist = self._prepare()
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/resolution/resolvelib/candidates.py", line 222, in _prepare
                dist = self._prepare_distribution()
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/resolution/resolvelib/candidates.py", line 301, in _prepare_distribution
                return preparer.prepare_linked_requirement(self._ireq, parallel_builds=True)
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/operations/prepare.py", line 525, in prepare_linked_requirement
                return self._prepare_linked_requirement(req, parallel_builds)
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/operations/prepare.py", line 640, in _prepare_linked_requirement
                dist = _get_prepared_distribution(
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/operations/prepare.py", line 70, in _get_prepared_distribution
                with build_tracker.track(req, tracker_id):
              File "/usr/lib/python3.9/contextlib.py", line 117, in __enter__
                return next(self.gen)
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/operations/build/build_tracker.py", line 137, in track
                self.add(req, tracker_id)
              File "/home/pi/.local/lib/python3.9/site-packages/pip/_internal/operations/build/build_tracker.py", line 103, in add
                raise LookupError(message)
            LookupError: https://files.pythonhosted.org/packages/63/10/8c5fe6c6f3ea61dff23b570693d06fe78bb9a65ee1a0a437512122061109/cmake-3.29.0.1.tar.gz (from https://pypi.org/simple/cmake/) (requires-python:>=3.7) is already being built: cmake>=3.1 from https://files.pythonhosted.org/packages/63/10/8c5fe6c6f3ea61dff23b570693d06fe78bb9a65ee1a0a437512122061109/cmake-3.29.0.1.tar.gz
            [end of output]
      
        note: This error originates from a subprocess, and is likely not a problem with pip.
      error: subprocess-exited-with-error
      
      × pip subprocess to install backend dependencies did not run successfully.
      │ exit code: 2
      ╰─> See above for output.
      
      note: This error originates from a subprocess, and is likely not a problem with pip.
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
error: subprocess-exited-with-error

× pip subprocess to install build dependencies did not run successfully.
│ exit code: 1
╰─> See above for output.

note: This error originates from a subprocess, and is likely not a problem with pip.
```
Method 1: 
1. Install OpenSSL Development Libraries: Ensure that the necessary OpenSSL libraries and headers are installed. You can do this by running:
```
sudo apt-get update
sudo apt-get install libssl-dev
```
2. Install CMake: If CMake is not already installed, or if you have an older version, install or upgrade it. You can install CMake via your package manager or download the latest version directly from the CMake website.
```
sudo apt-get install cmake
```
3. Ensure You Have pip and setuptools Updated: Sometimes, issues with building packages can be resolved by updating ```pip``` and ```setuptools```.
```
pip3 install --upgrade pip setuptools
```
4. Install Additional Dependencies: Some additional dependencies might be required for building OpenCV. Install them by running:
```
sudo apt-get install build-essential pkg-config
sudo apt-get install libjpeg-dev libtiff-dev libpng-dev
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install libgtk2.0-dev libcanberra-gtk*
sudo apt-get install libxvidcore-dev libx264-dev
sudo apt-get install libatlas-base-dev gfortran
```
5. Retry Installation: After ensuring all dependencies are installed, try installing ```opencv-python``` again.
```
pip3 install opencv-python
```

Method 2: 
If Method 1 fails, consider using the pre-built OpenCV packages from the Ubuntu repositories, which are easier to install and maintain:
```
sudo apt-get install python3-opencv
```
This command installs the OpenCV library for Python, and it should be sufficient for most use cases.



Method 3: Clone the OpenCV Library using ```git clone```

NOTE: Use this method only if the above two methods fail. Cloning and building the library takes a lot of time.

1. Run the following commands:
```
pip3 uninstall opencv-python
sudo apt update
sudo apt upgrade
```
2. Install the necessary dependencies for building OpenCV:
```
sudo apt install build-essential cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt install libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev
```
3. Clone the OpenCV repository from GitHub:
```
git clone https://github.com/opencv/opencv.git
```
4. Navigate to the directory where you cloned OpenCV and create a build directory:
```
cd opencv
mkdir build
cd build
```
5. Configure the build with cmake:
```
cmake ..
```
6. Build OpenCV: (This takes time)
```
make -j$(nproc)
```
7. Install OpenCV:
```
sudo make install
```
8. Install cv2:
```
pip3 install cv2
```

# Verify Installation
Type the following in terminal to verify the installation of OpenCV
```
python3
```
```
Python 3.12.3 (tags/v3.12.3:f6650f9, Apr  9 2024, 14:05:25) [MSC v.1938 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import cv2
>>>
```
The ```cv2 ``` module should be imported into Python without any errors. This indicates that OpenCV module is correctly installed.



