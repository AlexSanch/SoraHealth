# SoraHealth

<img src="https://i.ibb.co/JycKWz8/Sorahealth.png" width="400">

Measuring blood sugar is not the only way in which a person can detect that it has this high level, there are times when a person detects symptoms in themselves that are not normal and these can trigger a more serious problem, such as a fainting or death.

If someone detects that the sugar level is high, the best thing to do is take medication to lower it, however this may be counterproductive in some cases, because these medications can lower the level of sugar so much that it could be mortal.

If only there was an easier, more natural way of tracking that even outside your home, patients sometimes have relapses and periods when they need help fast when outside and even at home and even more so if they are elders.

For this problem the answer is simple, when there is nothing else to do, asking for help quickly is the best solution to the problem, so that's why I decided to create SoraHealth, a device with the ability to send a notification of help quickly with just pressing a button.

# Table of contents
* [Introduction](#introduction)
* [Materials](#materials)
* [Setup Wio Module](#setup-wio-module)
* [Arduino IDE Setup](#arduino-ide-setup)
* [AWS IAM Setup](#aws-iam-setup)
* [AWS IoT Setup](#aws-iot-setup)
* [Soracom Console](#soracom-console)
* [AWS Setup](#aws-iam-setup)
* [The Final Product](#the-final-product)

## Materials:

Hardware: 

- WIO LTE - USA Version (Cortex-M4  microcontroller). 
- Soracom Air Global IoT SIM Card.
- Grove Button (http://wiki.seeedstudio.com/Grove-Button/).
- 2 x LTE antenna.
- Lipo Battery, 1050mAh, 3.7V.

Software: 

- Arduino IDE (https://www.arduino.cc/en/Main/Software). 
- Amazon Web Services (https://aws.amazon.com).
- Soracom Console (https://console.soracom.io).

## Setup Wio Module:

The module is so simple and easy to install, that we just have to connect everything to the WIO and that's it! Here's an image of how it looks when it's ready!

<img src="https://i.ibb.co/njvg20f/3.jpg" width="800">

## PC Configuration:
(All these steps are directly in the official guide http://wiki.seeedstudio.com/Wio_LTE_Cat.1/#getting-started)

- Install USB driver:

Windows Users: Most versions of Windows won't automatically load the built-in driver for USB com ports. You'll have to download ST's USB driver:

Non-Windows XP Users download version 1.4.0 drivers. Unzip the file, run the executable, and then go to C:\Program Files (x86)\STMicroelectronics\Software\Virtual comport driver in Windows Explorer and double-click either dpinst_amd64.exe for 64 bit systems, or dpinst_x86.exe for 32 bit.

Windows XP Users download version 1.3.1 drivers. Unzip the file, run VCP_V1.3.1_Setup.exe, and then go to C:\Program Files\STMicroelectronics\Software\Virtual comport driver in Windows Explorer and double-click the executable.

Linux users to ensure that you have the correct permissions to connect as a normal user you'll need to copy the file 45-espruino.rules to /etc/udev/rules.d, reload rules with udevadm control --reload-rules, and ensure your user is in the plugdev group (you can check by typing groups). You add it by typing sudo adduser $USER plugdev and then logging out and back in. Arch Linux users need to add their user to uucp and lock groups instead.

Mac OS X and Chromebook Users: The board will just plug in and work, without drivers!

- Change DFU driver:

For windows users:

Press and hold BOOT button and connect to computer you will see STM32 Device in DFU Mode at device manager as below.

<img src="https://github.com/SeeedDocument/Wio_LTE/raw/master/img/before_driver_installation.png" width="800">

- This says that you need to use zadig_xx.exe to change DFU driver from STTub30 to WinUSB as below. If we can't see any info on the Zadig, please click Options→ List All Devices, then select STM32 Virtual COM Ports.

<img src="https://github.com/SeeedDocument/Wio_LTE/raw/master/img/zadig.png" width="800">

- You will see the "STMicroelectronics Virtual COM Port" on device manager as below.

<img src="https://github.com/SeeedDocument/Wio_LTE/raw/master/img/after_driver_installation.png" width="800">

## Arduino IDE Setup:

Install Arduino IDE, recommand IDE version upon 1.8.0.

https://www.arduino.cc/en/Main/Software

Follow How to Add Seeed boards to Arduino IDE to add Wio_LTE into arduino board manager.

http://wiki.seeedstudio.com/Seeed_Arduino_Boards/

<img src="https://i.ibb.co/N9zzvh5/1.png" width="800">

Download the Wio_LTE Library from Github.

https://github.com/Seeed-Studio/Wio_LTE_Arduino_Library

<img src="https://i.ibb.co/CPLx8q5/2.png" width="800">

Refer How to install library to install library for Arduino.

http://wiki.seeedstudio.com/How_to_install_Arduino_Library/

## Arduino Code:

The Arduino code is in the Arduino Code folder, however the most important parts are the following.

This is the Pin that accepts interruptions.

    #define BUTTON_PIN  (WIOLTE_A4)
    
Volatile variables to control the interruption of the button.

    volatile bool StateChanged = false;
    volatile bool State = false;

We create an interruption by falling edge.

    attachInterrupt(BUTTON_PIN, change_state, FALLING);
    
Through this command we send our location in real time once we press the button, this information will serve us later in AWS to send it as a google maps link.

    sprintf(data, "{\"lat\":\"%d\",\"lon\":\"%d\"}",gnss.latitude,gnss.longitude);

## AWS IAM Setup:

Note: it is important to clarify that the data will be sent to the Soracom platform, however the AWS credentials are necessary to configure it correctly and the AWS IoT endpoint url, so first we will show how to configure the AWS IAM credentials, after Soracom Console and at the end the rest of AWS services.

- We create a new user:

<img src = "https://i.ibb.co/jhBJLCK/3.png" width = "800">

- We put whatever name it is and continue to the permissions tab

<img src="https://i.ibb.co/XFdNNJ8/4.png" width="800">

- We attach the "AWSIoTFullAccess" Policy.

<img src="https://i.ibb.co/WVPzdbW/5.png" width="800">

- Press Next.

<img src="https://i.ibb.co/RTKbDC4/6.png" width="800">

- Press "Create User"

<img src="https://i.ibb.co/f2bsd06/7.png" width="800">

- Save your "Access key ID" and "Secret access key" for the Soracom Console.

<img src="https://i.ibb.co/tsHcgxv/8.png" width="800">

## AWS IoT Setup:

- In AWS IoT Core, we go to the "Settings" tab.

<img src = "https://i.ibb.co/ZW1mTLj/9.png" width = "200">

- In this option we will see our endpoint, save it for the configuration of Soracom Console.

<img src="https://i.ibb.co/1My1Yq7/10.png" width="800">

## Soracom Console:

- Go to "Group" tab.

<img src="https://i.ibb.co/C9Nqxb8/11.png" width="800">

- Go to "SORACOM Funnel" and setup all your credentials, your Endpoint url and your topic inside the url.

<img src="https://i.ibb.co/P6Vrqk6/12.png" width="800">

## AWS Setup:

- In the "Act" tab we create a new rule.

<img src = "https://i.ibb.co/K2xC927/13.png" width = "800">

- We put any name and in the "Rule query statement" we put the following.

Note: this is SQL language.

    SELECT payloads.lat as lat, payloads.lon as lon FROM 'SH'

<img src="https://i.ibb.co/tPvXXXR/14.png" width="800">

- We select "Add action".

<img src = "https://i.ibb.co/4dkr0nY/15.png" width = "800">

- We select Lambda. This is because we need to process the data received by Aws IoT.

<img src = "https://i.ibb.co/X5xx3QL/16.png" width = "800">

- We select "Create a new Lambda function".

<img src = "https://i.ibb.co/MVwnMw7/17.png" width = "800">

- Set the lambda as shown in the next screen.

<img src = "https://i.ibb.co/wJ8wCMw/18.png" width = "800">

- The lambda should look like this, copy and paste the function below to your lambda. 

<img src = "https://i.ibb.co/W2NdP72/19.png" width = "800">

- We go on AWS to SNS, to create the notification system.

<img src = "https://i.ibb.co/zJGTJJg/20.png" width = "800">

- Once our notification system is created we will obtain our RNA for our Lambda function.

<img src = "https://i.ibb.co/nDLKBr5/21.png" width = "800">

- We select the option "Create subscription" and select one of the following notification options.

<img src = "https://i.ibb.co/jbYt5DT/22.png" width = "800">


### Lambda Function:
    console.log('Loading function');
    // Load the AWS SDK
    var AWS = require("aws-sdk");

    // Set up the code to call when the Lambda function is invoked
    exports.handler = (event, context, callback) => {
    // Load the message passed into the Lambda function into a JSON object 
    var eventText = JSON.parse(JSON.stringify(event, null, 2));


    // Log a message to the console; you can view this text in the Monitoring tab in the Lambda console or in the CloudWatch Logs console

    // Create a string, extracting the click type and serial number from the message sent by the AWS IoT button

    // Write the string to the console


    var latitude=parseInt(eventText.lat)
    var longitude=parseInt(eventText.lon)


    // Create an SNS object
    var sns = new AWS.SNS();

    console.log("Received event:"+"https://www.google.com.mx/maps/@"+ eventText.lat +","+eventText.lon);

    var params = {
    Message: ("https://www.google.com.mx/maps/@"+ eventText.lat +","+eventText.lon+",18.25z"),
    TopicArn: "arn:aws:sns:XXXXXXXXXXXXXX:XXXXXXXXXXXXXX:SHNotification"
    };
    sns.publish(params, context.done);

    };


## The Final Product:

- Now we assembled everything in a final cas, for this product, I decided to use an old cassette's box.

<img src = "https://i.ibb.co/8cYnrtw/IMG-20190513-013911.jpg" width = "800">

- Assemble inside the case, the antennas and the button.

<img src = "https://i.ibb.co/X89vnxJ/IMG-20190513-013942.jpg" width = "800">

- We put the Grove cable.

<img src = "https://i.ibb.co/brb0FYp/IMG-20190513-014029.jpg" width = "800">

-Located the Lithium Battery.

<img src = "https://i.ibb.co/JvZ7gfr/IMG-20190513-014045.jpg" width = "800">

- To ensure that the box could press the button, place the back of a pen inside the case, melting it a bit so that it fits perfectly.

<img src = "https://i.ibb.co/KDQRS2j/IMG-20190513-013208.jpg" width = "400"> <img src = "https://i.ibb.co/28XmxML/IMG- 20190513-013214.jpg "width =" 400 ">

- A hole was made on the side so that the charging cable enters correctly.

<img src = "https://i.ibb.co/94B1XCj/IMG-20190513-013252.jpg" width = "800">

- This is how the final product looks with the cable so that it can be easily loaded.

<img src = "https://i.ibb.co/6XTcq32/IMG-20190513-013324.jpg" width = "800">

### Demo Time:

(Insertar Video XP)
