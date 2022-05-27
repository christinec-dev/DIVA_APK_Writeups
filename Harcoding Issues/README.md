# Harcoding Issues
If you've read my previous tutorials(or write-ups), you would know that I have started dabbling in the world of hacking, with my most recent write-ups focusing primarily on Android Pentesting. I've shown you how to install [Genymotion & Virtualbox](https://dev.to/christinecdev/how-to-install-genymotion-virtualbox-on-parrot-os-287p), [JADX-GUI & ADB](https://dev.to/christinecdev/android-how-to-install-adb-apks-and-jdx-gui-on-parrot-os-3m9c), and [Android Studio](https://tutorialforlinux.com/2021/04/05/step-by-step-android-studio-parrot-linux-installation/) on Parrot OS - but today we will go beyond just installing tools. Today we start hacking, _kind of_.

First things first, I need to make sure that we are on the same page. What is [Android Pentesting](https://www.getastra.com/blog/security-audit/android-penetration-testing/)? Simply put, it is a simulated cyber attack against a mobile application where we try to expose and find any vulnerabilities or security issues that are present in the application. In the following days to come, we will be Pentesting the [DIVA APK](https://www.payatu.com/wp-content/uploads/2016/01/diva-beta.tar.gz), which I explain and demonstrate how to install and set up on an emulated device in this [tutorial](https://dev.to/christinecdev/android-how-to-install-adb-apks-and-jdx-gui-on-parrot-os-3m9c).

**<u>Before we start make sure that you have the following:</u>**
- Android Studio Installed.
- Genymotion and Virtualbox installed, with an emulated device setup and running.
- ADB is installed with DIVA APK installed onto the device. 
- JADX-GUI is installed and open.

Okay, let's get hacking! When we open the DIVA application on our device, we can see that it consists of a menu or navigation that lists many sections for us to Pentest. 

![Diva Interface](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/00pka1yeuzi564jct4y1.png)

In this section, we will be testing the Hardcoding Issues (Part 1 & 2) sections of this application. 

## Hardcoding Issues Part One
When a programmer hardcodes a value, it means that they type it into the application, making it a static value. For example:
```
var password = "iamnotsecure";
```
This is not only bad from a programming perspective since the developer will have to manually change this value every time they want to update it, but from a security perspective, it allows attackers to easily exploit or hijack firmware, devices, systems, and software. 

When we open the Hardcoding Issues Part One section we are met with the following objective: find out what is hardcoded and where. So, we are expected to find what is being hardcoded and where. From the get-go, we can immediately identify what will be hardcoded: **the vendor key**.

![Hardcoded part 1](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/z4gjjju2b3y9w4weiukh.png)

On to the where. Let's head into JADX-GUI and open up the **HardcodeActivity** activity to see if we can find anything in the source code.

![Hardcoded part 1](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k6knfvvvh3a5xflvfuak.png)

Immediately we can see that the vendor key is hardcoded, thus we can copy the "**vendorsecretkey**" value and pop it into our app.

![Hardcoded part 1](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gkaoiswgdi7grbycc16v.png)

Voila, we have gained access to the system!🥳

![Hardcoded part 1](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/isuirxzpyayyvp6kmz4p.png)

**<u>So, in summary, we were able to find the following:</u>**
- **What**: Vendor Secret Key > vendorsecretkey
- **Where**: HardcodeActivity

## Hardcoding Issues Part Two
When we open the Hardcoding Issues Part Two section we are met with the following objective: find out what is hardcoded and where. So, we are expected to find what is being hardcoded and where. Just like previously, we can immediately identify what is probably hardcoded just by looking at the activity layout: **the vendor key**.

![Hardcoded part 2](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7z1lk1pzpar9avffmbdl.png)
 
Now on to the where. Let's head into our **Hardcode2Activity** in JADX-GUI and open it up. We can immediately identify the **DivaJni** class that is being referenced, as the rest of the code depends on this class to be able to validate the access key. 

![Hardcoded part 2](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tmg72jsu7gh70iwopxty.png)

![Hardcoded part 2](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/j37xc51bbmbd4v85s34l.png)

Without wasting further time, let's open the DivaJni class. When I first opened it, without properly assessing the code, I saw the string soName = "divajni", and initially I thought this was the hardcoded value. To my dismay, it was not. Silly me, maybe next time I should read the source code fully! 😂

![Hardcoded part 2](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vlkpn8uu5ow0ydjgjhfu.png)

![Hardcoded part 2](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5dqsk00kolz23vrdg0e2.png)

We can see upon further inspection that the static function loads the native library of **soName**, which is a library named **divajni**. Since we can assume that it calls upon a [Shared Library](https://tldp.org/HOWTO/Program-Library-HOWTO/shared-libraries.html), we can go looking for a file named "divajni.so". The .so extension identifies a Shared Object library that may be dynamically loaded during Android runtime. 

Mmh, so we are looking for a library containing "divajni.so" in our lib folder within our APK file. Go to where you installed your DIVA APK and extract the file (to do this easily, just replace .apk with .zip and extract like normal). Once extracted, open it up and go **lib > any one of the folders > libdivajni.so**.

---
**IMPORTANT NOTE:** You can also access this library file via the terminal without having to extract the file and navigate to it like this:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8k118x30vwe39iqzffki.png)

---

![Hardcoded part 2](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gxdq17lj7fa0t5jrg4j1.png)

You can copy this library file to your Desktop or anywhere with easy access. Open up your terminal using CTRL + ALT + T and **cd** into Destkop. Make sure your libdivajni.so file is there by entering the **ls** command. 

![Hardcoded part 2](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/48v1wm5sud7pqamjlcvn.png)

To be able to view the text inside a binary or data file such as our library file, we need to use the [strings <filename>](https://www.howtogeek.com/427805/how-to-use-the-strings-command-on-linux/) command. Enter it as such: **strings libdivajni.so**. We can see a list of words popping up, mainly packages and extended libraries. If you look at it, most of them have a similar format: .x, _x, __x, x.so, etc. There is one string that stood out to me, because why would it have a semi-colon? Why? None of the other strings have semi-colons! 
 
![Hardcoded part 2](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/nmv2novoirbor3l7zfjo.png)

Let's try entering it as our vendor key. Please note that this process is trial-and-error. Sometimes we might not be so lucky as to identify a clear outlier in a string list as in this scenario. If we enter it into our app, we get a popup saying we have gained access, and success!
 
![Hardcoded part 2](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2fnwcg510pce8vwqouuq.png)

**<u>So, in summary, we were able to find the following:</u>**
- **What**: Vendor Secret Key
- **Where**: libdivajni.so > olsdfgad;lh

## Conclusion
Congrats, we have finished the first two sections of the DIVA APK! I hope this was easy enough to follow/understand. I'll see you next time with Section 3: Insecure Data Storage.😊

If you have recommendations on any cool tools, techniques, or tutorials that I too can follow feel free to leave them below and I'll check it out!

![More](https://media1.giphy.com/media/l2JHQzrwFCOw98rMA/giphy.gif)

