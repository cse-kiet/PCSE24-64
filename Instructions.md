#    People Counting in Real-Time using live video stream/IP camera in OpenCV.

> NOTE: This is an improvement/modification to https://www.pyimagesearch.com/2018/08/13/opencv-people-counter/

<div align="center">
<img src=https://imgur.com/SaF1kk3.gif" width=550>
<p>Live demo</p>
</div>

- The primary aim is to use the project as a business perspective, ready to scale.
- Use case: counting the number of people in the stores/buildings/shopping malls etc., in real-time.
- Sending an alert to the staff if the people are way over the limit.
- Automating features and optimising the real-time stream for better performance (with threading).
- Acts as a measure towards footfall analysis and in a way to tackle COVID-19 scenarios.

--- 

## Running Inference

### Install the dependencies

First up, install all the required Python dependencies by running: ```
pip install -r requirements.txt ```

> NOTE: Supported Python version is 3.11.3 (there can always be version conflicts between the dependencies, OS, hardware etc.).

### Test video file

To run inference on a test video file, head into the root directory and run the command: 

```
python people_counter.py --prototxt detector/MobileNetSSD_deploy.prototxt --model detector/MobileNetSSD_deploy.caffemodel --input utils/data/tests/test_1.mp4
```

### Webcam

To run on a webcam, set ```"url": 0``` in ```utils/config.json``` and run the command:

```
python people_counter.py --prototxt detector/MobileNetSSD_deploy.prototxt --model detector/MobileNetSSD_deploy.caffemodel
```

### IP camera

To run on an IP camera, setup your camera url in ```utils/config.json```, e.g., ```"url": 'http://191.138.0.100:8040/video'```.

Then run the command:
```
python people_counter.py --prototxt detector/MobileNetSSD_deploy.prototxt --model detector/MobileNetSSD_deploy.caffemodel
```

---

## Features

The following features can be easily enabled/disabled in ```utils/config.json```:

```json
{
    "Email_Send": "",
    "Email_Receive": "",
    "Email_Password": "",
    "url": "",
    "ALERT": false,
    "Threshold": 10,
    "Thread": false,
    "Log": false,
    "Scheduler": false,
    "Timer": false
}
```

### Real-Time alert

If selected, we send an email alert in real-time. Example use case: If the total number of people (say 10 or 30) are exceeded in a store/building, we simply alert the staff. 

- You can set the max. people limit in config, e.g., ```"Threshold": 10```.
- This is quite useful considering scenarios similar to COVID-19. Below is an example:
<img src="https://imgur.com/35Yf1SR.png" width=350>

> ***1. Setup your emails:***

In the config, setup your sender email ```"Email_Send": ""``` to send the alerts and your receiver email ```"Email_Receive": ""``` to receive the alerts.

> ***2. Setup your password:***

Similarly, setup the sender email password ```"Email_Password": ""```.

Note that the password varies if you have secured 2 step verification turned on, so refer the links below and create an application specific password:

- Google mail has a guide here: https://myaccount.google.com/lesssecureapps
- For 2 step verified accounts: https://support.google.com/accounts/answer/185833

### Threading

- Multi-Threading is implemented in ```utils/thread.py```. If you ever see a lag/delay in your real-time stream, consider using it.
- Threading removes ```OpenCV's internal buffer``` (which basically stores the new frames yet to be processed until your system processes the old frames) and thus reduces the lag/increases fps.
- If your system is not capable of simultaneously processing and outputting the result, you might see a delay in the stream. This is where threading comes into action.
- It is most suitable to get solid performance on complex real-time applications. To use threading: set ```"Thread": true,``` in config.

### Scheduler

- Automatic scheduler to start the software. Configure to run at every second, minute, day, or workdays e.g., Monday to Friday.
- This is extremely useful in a business scenario, for instance, you could run the people counter only at your desired time (maybe 9-5?).
- Variables and any cache/memory would be reset, thus, less load on your machine.

```python
# runs at every day (09:00 am)
schedule.every().day.at("9:00").do(run)
```

### Timer

- Configure stopping the software execution after a certain time, e.g., 30 min or 8 hours (currently set) from now.
- All you have to do is set your desired time and run the script.

```python
# automatic timer to stop the live stream (set to 8 hours/28800s)
end_time = time.time()
num_seconds = (end_time - start_time)
if num_seconds > 28800:
    break
```
