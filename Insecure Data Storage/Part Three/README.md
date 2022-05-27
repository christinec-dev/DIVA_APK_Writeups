# Part 3
If you've read my previous [writeup](https://dev.to/christinecdev/android-pentesting-writeup-for-the-diva-insecure-logging-and-hardcoding-issues-for-parrot-os-1mo1) where I covered Logging and Hardcoding Issues (1 & 2) on the DIVA APK, then get ready for the next one. I hope you have your tools open, because today we are going to find all the Insecure Data Storage vulnerabilities in the DIVA APK!😁

So the DIVA APK has four sections to test for Insecure Data Storage. [Insecure Data Storage](https://owasp.org/www-project-mobile-top-10/2016-risks/m2-insecure-data-storage) vulnerabilities occur when programmers assume that users, malware, or attackers will not have access to a mobile device’s filesystems and sensitive information that is stored on the device or APK. The problem arises when filesystems are easily accessible. Attackers, or us in this case, can root or jailbreak mobile devices or APK's and view application data - ultimately, sensitive application data.

Without any further lollygagging, let's jump into it!

![Image description](https://media.giphy.com/media/0DYipdNqJ5n4GYATKL/giphy.gif)

## Insecure Data Storage - Part 3
When we open the Insecure Data Storage - Part 3 section we are met with the following objective: find out where/how the credentials are being stored and the vulnerable code. So, we are expected to find where the credentials are being stored, how it is stored, and the vulnerable code.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/h7yyzy35xcbyoqdtng1i.png)

Let's first start with inserting information into the form. We get a popup saying that our credentials has been saved,and that's about it. Again, not much to it, so let's see if we can find where it saved our credentials.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/otxqqiiqud04fhpve5v8.png)
 
Open up JADX-GUI and navigate over to the **InsecureDataStorage3Activity**. From here we can see that our data is being stored in a [FileWriter](https://developer.android.com/reference/java/io/FileWriter) object that is being stored as **uinfotmp** - so we are looking for a file with a (tmp) ending.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ohkssj9b606x5xxd4e98.png)
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/uk3svkscotl72iv4uzv8.png)

The FileWriter tmp file will be saved under **../data/data/name.company.com/..** and to access or view this uinfo file I will show you two methods: how to access it in Android Studio, and how to access it via the terminal.

### Method One: Android Studio
Open up Android Studio and select the Device Manager bar on the right-side of your window. Then head into the File Explorer and navigate to the following file (**data > data > jakhar.aseem.diva**. From here we can see that our file is there, labeled as something similar to uinfoxxxxxxxxxxxxtmp.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b4avl90hvwh828phd6jm.png)
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/llihopuprd3xwis8oqlu.png)  
  
Double click your **uinfotmp** file. You can now read the data!

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5d8fyeffmeapvey849y6.png)

### Method Two: Terminal
Open up your terminal via CTRL + ALT + T and start a adb shell via the **adb shell** command. From there on we can go:
```
su
cd data
cd data
cd jakhar.aseem.diva
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/g6xrkpyyja35ywu91gcr.png)
 
When we list the files we can see our uinfotmp file. To read the contents of the uinfotmp file, we can simply say **cat uinfoxxxxxxxxxxxxtmp** (replace uinfoxxxxxxxxxxxxtmp with your filename), and you are set! 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pd7105c3pxwm1xjl3k5l.png)
 
**<u>So, in summary, we were able to find the following:</u>**
- **Where**: jakhar.aseem.diva > uinfotmp
- **How**: FileWriter(File)

Congrats, we have finished the third part of Insecure Data Storage of the DIVA APK! I hope this was easy enough to follow/understand. If you have recommendations on any cool tools, techniques, or tutorials that I too can follow feel free to leave them below and I'll check it out!


  






  





