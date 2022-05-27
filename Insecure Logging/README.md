# Insecure Logging
If you've read my previous tutorials(or write-ups), you would know that I have started dabbling in the world of hacking, with my most recent write-ups focusing primarily on Android Pentesting. I've shown you how to install [Genymotion & Virtualbox](https://dev.to/christinecdev/how-to-install-genymotion-virtualbox-on-parrot-os-287p), [JADX-GUI & ADB](https://dev.to/christinecdev/android-how-to-install-adb-apks-and-jdx-gui-on-parrot-os-3m9c), and [Android Studio](https://tutorialforlinux.com/2021/04/05/step-by-step-android-studio-parrot-linux-installation/) on Parrot OS - but today we will go beyond just installing tools. Today we start hacking, _kind of_.

First things first, I need to make sure that we are on the same page. What is [Android Pentesting](https://www.getastra.com/blog/security-audit/android-penetration-testing/)? Simply put, it is a simulated cyber attack against a mobile application where we try to expose and find any vulnerabilities or security issues that are present in the application. In the following days to come, we will be Pentesting the [DIVA APK](https://www.payatu.com/wp-content/uploads/2016/01/diva-beta.tar.gz), which I explain and demonstrate how to install and set up on an emulated device in this [tutorial](https://dev.to/christinecdev/android-how-to-install-adb-apks-and-jdx-gui-on-parrot-os-3m9c).

**<u>Before we start make sure that you have the following:</u>**
- Android Studio Installed.
- Genymotion and Virtualbox installed, with an emulated device setup and running.
- ADB is installed with DIVA APK installed onto the device. 
- JADX-GUI is installed and open.

Okay, let's get hacking! When we open the DIVA application on our device, we can see that it consists of a menu or navigation that lists many sections for us to Pentest. 

![Diva Interface](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/00pka1yeuzi564jct4y1.png)

In this section, we will be testing the Insecure Logging section of this application. 

## Insecure Logging
When we open the Insecure Logging section we are met with the following objective: find out what is being logged where/how and the vulnerable code. So, we are expected to find what is being logged, how it is logged, and the vulnerable code.

![Insecure Logging](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/589ypsx72k2o0pz4e6f7.png)

Let's try and answer these objectives one by one. From first impressions, just by looking at the activity layout (which is the page opened on our screen), we can see that it asks us for our credit card numbers. Thus, our credit card number is probably the data that is being logged (what). Let's test this by entering a number!

![Insecure Logging](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/sip5hhr5gmfvtraajz2k.png)

With our number entered, we get an error popup. Let's see if we can see how it is being logged, ie. where did our data go? The first culprit that we can look at is **LogCat in Android Studio**. Open up Android Studio and navigate over to the LogCat Output. We can see that it logs everything, which is bad because say we have a user that downloads a malicious APK that secretly monitors their log when they use the app - the attacker can then use this monitored log to read sensitive data, and exploit them in this way. 

When we scroll down, we see that our credit card number has been logged. BAD LOGCAT!  Now we know how it is being logged, via the LogCat Output in Android Studio underneath a log labeled diva-log.

![Insecure Logging](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/oty1b2bixc3ji1h9v653.png)

Finally, we need to find the vulnerable code. Easy peasy, just load up your **DIVA APK into JADX-GUI** and open up the **LogActivity** activity. 

![Insecure Logging](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jutzsahjj3b5k8aztjhf.png)![Insecure Logging](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/h6uenjjpifimy9mf7ed5.png)

When we look at the code, we can identify the section where it does the actual logging, thus confirming that it is logging our credit card number. Maybe logging sensitive information is not the best way to capture this information? 

![Insecure Logging](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k49f3417y916ybrj1nx3.png)
 
**<u>So, in summary, we were able to find the following:</u>**
- **What**: Credit Card Number
- **Where**: LogCat > diva-log
- **How**: Log.e()

## Conclusion
Congrats, we have finished the first two sections of the DIVA APK! I hope this was easy enough to follow/understand. I'll see you next time with Section 2: Hardcoding Issues.😊

If you have recommendations on any cool tools, techniques, or tutorials that I too can follow feel free to leave them below and I'll check it out!

![More](https://media1.giphy.com/media/l2JHQzrwFCOw98rMA/giphy.gif)
