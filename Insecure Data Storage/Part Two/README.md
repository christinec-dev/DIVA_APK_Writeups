# Part 2
If you've read my previous [writeup](https://dev.to/christinecdev/android-pentesting-writeup-for-the-diva-insecure-logging-and-hardcoding-issues-for-parrot-os-1mo1) where I covered Logging and Hardcoding Issues (1 & 2) on the DIVA APK, then get ready for the next one. I hope you have your tools open, because today we are going to find all the Insecure Data Storage vulnerabilities in the DIVA APK! 😁

So the DIVA APK has four sections to test for Insecure Data Storage. [Insecure Data Storage](https://owasp.org/www-project-mobile-top-10/2016-risks/m2-insecure-data-storage) vulnerabilities occur when programmers assume that users, malware, or attackers will not have access to a mobile device’s filesystems and sensitive information that is stored on the device or APK. The problem arises when filesystems are easily accessible. Attackers, or us in this case, can root or jailbreak mobile devices or APK's and view application data - ultimately, sensitive application data.

Without any further lollygagging, let's jump into it!

![Image description](https://media.giphy.com/media/0DYipdNqJ5n4GYATKL/giphy.gif)

## Insecure Data Storage - Part 2
When we open the Insecure Data Storage - Part 2 section we are met with the following objective: find out where/how the credentials are being stored and the vulnerable code. So, we are expected to find where the credentials are being stored, how it is stored, and the vulnerable code.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tyncprj3sbz3u8m6thtc.png)

Let's first start with inserting information into the form. We get a popup saying that our credentials has been saved,and that's about it. Again, not much to it, so let's see if we can find where it saved our credentials.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/rtoh9oazeavhnwv62ggb.png)
 
Open up JADX-GUI and navigate over to the **InsecureDataStorage2Activity**. From here we can see that our data is being stored in a [SQLiteDatabase](https://developer.android.com/reference/android/database/sqlite/SQLiteDatabase) object - so we will be looking for a database file!

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2hy44l7fjthmdbd7v52h.png)
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6lzhnfmlkm9f4uk6mmys.png)

Usually, the SQLiteDatabase database files will be saved under **../data/data/name.company.com/databases/..** and to access or view this database file I will show you two methods: how to access it in Android Studio, and how to access it via the terminal.

### Method One: Android Studio
Open up Android Studio and select the Device Manager bar on the right-side of your window. Then head into the File Explorer and navigate to the following file (**data > data > jakhar.aseem.diva > databases**). From here we can see numerous database files, but just looking at the name we can deduce that "ids2" (id) will be more valuable to look at than "diva notes". So, let's have a look at **ids2**.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/u1qndzbv28qj2jgq4ib6.png)
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/viyy6buergwq9x2ttc94.png)
  
Right click and **save ids2** to a place on your desktop that you feel comfortable with. Once saved, open up your browser and head over to [SQLite Viewer](https://inloop.github.io/sqlite-viewer/) so that we can view the contents of our database. Pop in your ids2 file. We can see that there are two tables. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mg5xa6fx2ct56kuj7a0s.png)

Let's open the **myuser** table. We can now see our credentials! 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/e9nsph1k652ky80n0xp2.png)

### Method Two: Terminal
Open up your terminal via CTRL + ALT + T and start a adb shell via the **adb shell** command. From there on we can go:
```
su
cd data
cd data
cd jakhar.aseem.diva
cd databases 
```
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xlqxxtisks99etm0208e.png)
  
When we list the files in our databases directory, we can see that the **ids2** file is there. To read it, we need to first enter the **qlite3** command (_enter it by typing sql and hit TAB and enter_). 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wohf0lt8jky3lqo0c8gi.png)
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mh8s600rdxxzfjmebkxh.png)
 
From there on we need to open up the ids2 database file via the **.open ids2** command. If we list all the tables via the **.tables** command we can see that myuser is one of them. We can then "cd" into the myuser tables via the **.tables myuser** command.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2hozavwx9ca898gnv8ir.png)

To read the contents of the myuser table, we can simply say **.dump myuser**, and congrats, you've successfully cracked that bad boy open! 😎

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bgaypt0fqlho0vkb68yy.png)
 
**<u>So, in summary, we were able to find the following:</u>**
- **Where**: databases > ids2
- **How**: SQLiteDatabase mDB
   
Congrats, we have finished the second part of Insecure Data Storage of the DIVA APK! I hope this was easy enough to follow/understand. If you have recommendations on any cool tools, techniques, or tutorials that I too can follow feel free to leave them below and I'll check it out!

  






  





