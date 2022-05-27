# Part 1
If you've read my previous [writeup](https://dev.to/christinecdev/android-pentesting-writeup-for-the-diva-insecure-logging-and-hardcoding-issues-for-parrot-os-1mo1) where I covered Logging and Hardcoding Issues (1 & 2) on the DIVA APK, then get ready for the next one. I hope you have your tools open, because today we are going to find all the Insecure Data Storage vulnerabilities in the DIVA APK! 😁

So the DIVA APK has four sections to test for Insecure Data Storage. [Insecure Data Storage](https://owasp.org/www-project-mobile-top-10/2016-risks/m2-insecure-data-storage) vulnerabilities occur when programmers assume that users, malware, or attackers will not have access to a mobile device’s filesystems and sensitive information that is stored on the device or APK. The problem arises when filesystems are easily accessible. Attackers, or us in this case, can root or jailbreak mobile devices or APK's and view application data - ultimately, sensitive application data.

Without any further lollygagging, let's jump into it!

![Image description](https://media.giphy.com/media/0DYipdNqJ5n4GYATKL/giphy.gif)

## Insecure Data Storage - Part 1

When we open the Insecure Data Storage - Part 1 section we are met with the following objective: find out where/how the credentials are being stored and the vulnerable code. So, we are expected to find where the credentials are being stored, how it is stored, and the vulnerable code.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ngz7juqa5m4lmzmnyt16.png)

Let's first start with inserting information into the form. We get a popup saying that our credentials has been saved,and that's about it. Not much to it, so let's see if we can find where it saved our credentials.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/w4d4h533odjs7bv527z9.png)

Let's go ahead and pen up JADX-GUI and navigate over to the **InsecureDataStorage1Activity**. From here we can see that our data is being stored in a [SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences) object. A SharedPreferences object points to a file that contains key-value pairs and provides simple methods to read and write them. Each SharedPreferences file is managed by the framework and can be private or shared. A simple example of using a SharedPreference object is saving the user's preferred background color for the application.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zeesmtqdmtmzzmaunwhr.png) 
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vwz46v2ya6c6jz819r32.png)

Usually, the SharedPreference file will be saved under **../data/data/name.company.com/shared_prefs/..** and to access or view this SharedPreference file I will show you two methods: how to access it in Android Studio, and how to access it via the terminal.

### Method One: Android Studio
Open up Android Studio and select the Device Manager bar on the right-side of your window. Then head into the File Explorer and navigate to the following file (**data > data > jakhar.aseem.diva > shared_prefs > jakhar.aseem.diva_preferences.xml**)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dwb8phcf9fgu26htu71y.png) 
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/a4dj2y9dkmh1yi2pu4gz.png)
 
Double click on **jakhar.aseem.diva_preferences.xml**, and voila, we have access to the data via Android Studio!

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/shj9nmb3ckt1xn5bvq3o.png)

### Method Two: Terminal
Open up your terminal via CTRL + ALT + T and start a adb shell via the **adb shell** command. ADB Shell will allow us to access the device's files and manipulate the device via the terminal. We need to get super-user access, which is ultimate Darth Vader access, by entering to **su** command. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2tywwk6wyvb7u88n6ndi.png)

Let's go ahead and list our files on the device via the ls command, and from here on we can navigate to the directory as we did in Android Studio:
```
cd data
cd data
cd jakhar.aseem.diva
cd shared_prefs
```
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0mqnj3uvu0z05elzfd5i.png) 
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7pgspz9w1auwtn8chhm2.png)
...
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8cqii9rys92j02rco8em.png) 
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/j11zlojo1zbwn03cknul.png)
  
Finally, we can read the data by entering the **cat jakhar.aseem.diva_preferences.xml** command. And we're done! 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qxiy2t0iezoxkgn6gc75.png)
 
**<u>So, in summary, we were able to find the following:</u>**
- **Where**: shared_prefs > jakhar.aseem.diva_preferences.xml
- **How**: SharedPreferences() object
   
Congrats, we have finished the first part of Insecure Data Storage of the DIVA APK! I hope this was easy enough to follow/understand. If you have recommendations on any cool tools, techniques, or tutorials that I too can follow feel free to leave them below and I'll check it out!
  






  





