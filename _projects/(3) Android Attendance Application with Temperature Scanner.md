---
name: Android Attendance Application with Temperature Scanner
tools: [Kotlin, Android Studio]
image: https://sm.pcmag.com/t/pcmag_ap/news/a/a-wallpape/a-wallpaper-can-crash-some-android-10-phones_1mhp.1920.jpg
description: A Useful tool for attendace recording during COVID-19 pandemic.
---

# Android Attendance Application with Temperature Scanner

From where I live which is In Malaysia, There's this thing we do before entering premises as to follow the Standard Operational Procedure(SOP) for COVID-19 which is **scanning the provided QR code with the MySejahtera Application**. The aforementioned Android application is made by the government in order to track which premise there are in. There are also other features such as managing the users vaccine administration schedule, information on COVID-19, and other related functionality.

{% include elements/figure.html image="https://mysejahtera.malaysia.gov.my/img/gallery/gallery-en-11.png" caption="My Sejahtera Application" %}

But there's one problem with the application: The SOP requires body temperature reading to be taken before entering premises. You may ask, **"Why is it a problem? there's a temperature scanner on every premise now even my house have one!"**. Well, it just beeps and displays the temperature, but it doesn't record the body temperature reading. Wouldn't it be great if temperature readings can be taken? Surely we can get more insights from the data right?

## The Solution

I've design this prototype Android Application the can do the basics of recording the attendees information with an added feature: the ability to recieve temperature data from bluetooth enabled device. In this case, I'm using a NodeMCU ESP32 as a test device for temperature scanning.

### The Hardware

{% include elements/figure.html image="https://i1.wp.com/www.electroniclinic.com/wp-content/uploads/2021/04/ESP32.jpg" caption="NodeMCU ESP32" %}

As for the temperature reading, we'll be using the MLX90614 sensor since it's fairly cheap for it's capabiities. It offers a contactless temperature sensing using IR for both ambient and also object temperature, I've just checked on aliexpress.com and it's around $7.30. 

{% include elements/figure.html image="https://components101.com/sites/default/files/components/MLX90614.jpg" caption="MLX90614 Contacless IR temperature sensor" %}

The components are soldered on a perfboard as shown in the figure below with an added buzzer to inform the user that the temperature scanning process is complete.

{% include elements/figure.html image="https://raw.githubusercontent.com/ameerhaziq20/QRcodeScannerWithBodyTemp/main/Application%20Screenshots/hw_top_view.jpeg" caption="The temperature scanning device" %}

### The Android Application

For the Android Application, we will divide the functionality into three parts which are:

* Firebase Database
* QR scan/generate
* Temperature Scanning

#### Firebase Database

The project will be using the Firebase Real-Time Database for storing the attendees information. The setup was very easy and it is well documented. If you're using the database with Kotlin, the overall setup process for the database will be tremendously easy in contrast to Java. 


#### QR Scanning

The application will use a commonly used library for QR detection which is the Zxing library. The library can work as a QR code reader as well as a QR generator. In the development of this functionality of the app, There are 3 activities involved which are the ScanActivity, Capture Activity, and QRGenerateActivity.

##### ScanActivity

The ScanActivity is where the QR scanning funtionality happens. 
<script src="https://gist.github.com/ameerhaziq20/3b547bd1728a97c1c6aecc9dd8146659.js"></script>

<script src="https://gist.github.com/ameerhaziq20/12dd4dda762b9042153a1843894f203a.js"></script>


<script src="https://gist.github.com/ameerhaziq20/84925ff54da7bc0f0204b96d5f1394f7.js"></script>

