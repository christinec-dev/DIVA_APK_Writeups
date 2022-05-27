# Part 4
If you've read my previous [writeup](https://dev.to/christinecdev/android-pentesting-writeup-for-the-diva-insecure-logging-and-hardcoding-issues-for-parrot-os-1mo1) where I covered Logging and Hardcoding Issues (1 & 2) on the DIVA APK, then get ready for the next one. I hope you have your tools open, because today we are going to find all the Insecure Data Storage vulnerabilities in the DIVA APK!😁

So the DIVA APK has four sections to test for Insecure Data Storage. [Insecure Data Storage](https://owasp.org/www-project-mobile-top-10/2016-risks/m2-insecure-data-storage) vulnerabilities occur when programmers assume that users, malware, or attackers will not have access to a mobile device’s filesystems and sensitive information that is stored on the device or APK. The problem arises when filesystems are easily accessible. Attackers, or us in this case, can root or jailbreak mobile devices or APK's and view application data - ultimately, sensitive application data.

Without any further lollygagging, let's jump into it!

![Image description](https://media.giphy.com/media/0DYipdNqJ5n4GYATKL/giphy.gif)

## Insecure Data Storage - Part 4
When we open the Insecure Data Storage - Part 4 section we are met with the following objective: find out where/how the credentials are being stored and the vulnerable code. So, we are expected to find where the credentials are being stored, how it is stored, and the vulnerable code.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8h8f06ufjmfy9fzjwjph.png)

Let's first start with inserting information into the form. We get a popup saying that our credentials has been saved,and that's about it. Again, not much to it, so let's see if we can find where it saved our credentials.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eri6pxldfvbg87xtlwdt.png)
 
Open up JADX-GUI and navigate over to the **InsecureDataStorage4Activity**. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/315najz6o96dwiwtfjad.png)

From here we can see that our data is being stored in something called [Environment.getExternalStorageDirectory()](https://developer.android.com/reference/android/os/Environment). No worries, this only means that it is saving our file as **.uinfo.txt** to our devices' SDCard. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/t9ox9bz94gsmhnfi4n3q.png)

The SDCard will be saved under our root directory. I will show you how to access this file I via two methods: how to access it in Android Studio, and how to access it via the terminal.

### Method One: Android Studio
Open up Android Studio and select the Device Manager bar on the right-side of your window. Then head into the File Explorer and navigate to the following directory: **sdcard**. From here we can see that our file is there, labeled as **.uinfo.txt**.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tok51p2d77sql9h2d8bc.png)
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0l86fx6s53tiuu45xij2.png)
  
Double click your .**uinfo.txt** file. You can now read the data!

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hpc5i4bzowu8wqnr0ddb.png)
 
### Method Two: Terminal
Open up your terminal via CTRL + ALT + T and start a adb shell via the **adb shell** command. From there on we can go:
```
su
cd sdcard
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/m5rhezug0vdlqruhpdhc.png)
 
When we list the files we can see our .uinfo.txt file. To read the contents of the .uinfo.txt file, we can simply say **cat .uinfo.txt**. Now we know all it's dirty little secrets!

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/o32a5mhbc1b1b27jiy3v.png)

**<u>So, in summary, we were able to find the following:</u>**
- **Where**: sdcard > .uinfo.txt
- **How**: FileWriter(File)

Congrats, we have finished the fourth part of Insecure Data Storage of the DIVA APK! I hope this was easy enough to follow/understand. If you have recommendations on any cool tools, techniques, or tutorials that I too can follow feel free to leave them below and I'll check it out!


  






  





