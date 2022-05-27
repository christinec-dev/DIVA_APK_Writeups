# Input Validation Issues - Part Three

With another day gone, it is time for another Android Pen-test write-up. Today we are going to cover the fourth section of the DIVA APK, [Input Validation Issues](https://cwe.mitre.org/data/definitions/20.html). When we have an application that does not validate input properly, it makes it easier for an attacker to go ahead and creating input that is not expected by the rest of the application. This has dire consequences, ranging from altered data, arbitrary code execution, or unauthorized data access. Not good!

When you're ready, put on your favorite hoodie and grab your nearest drink, and let's get HACKING! 👾

![Image description](https://media.giphy.com/media/damUMYvgrroqw2hxSu/giphy.gif)

When we open the Input Validation Issues - Part 1 section on our device we are met with the following objective: ...DOS the damn thing! Do not find the code, just crash the app (and then find the root cause of the crash).

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zh9un9th9vetc7bnddxm.png)
 
Firsts things first, let's go over what a [DOS](https://www.paloaltonetworks.com/cyberpedia/what-is-a-denial-of-service-attack-dos) attack is. A Denial-of-Service (DOS) attack is an attack that has the intention of shutting down a system, which in turn makes it inaccessible or slow. We perform DOS attacks by flooding the target with traffic or large volumes of information that causes the system to crash. 

Now, we can go about this in various ways, but for this writeup let's do it the most basic way: by entering a large amount of data into the input and pushing the red button!

To make the app crash, I simply just spammed my keyboard with 0 until the input no longer accepted my string length, and voila, it worked!

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8eunk5cyjfmge1t1adas.png)
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lpi5omcl4wmb8xhhj7z1.png)

Okay, so we successfully completed the first part of the objective, which was to crash the app via a DOS attack. Let's head into Android Studio, or alternatively you can use the **adb logcat** command in your terminal (but I like the pretty AS colors 😆), to see what our log returned.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lp4vfv2nir5ith7c5s0j.png)
 
Now, there's a lot going on here, and it's easy to get overwhelmed, but let's focus on our error code SIGSEGV. I highlighted the [SIGSEGV](https://www.geeksforgeeks.org/segmentation-fault-sigsegv-vs-bus-error-sigbus/) code because it is is important since it indicates a segmentation fault in Linux containers. Simply put, we get this code since our application tries to read/write outside of the memory allocated for it or when writing memory which can only be read.  

Let's open up our JDX-GUI (jadx-gui) and see what our source code says. When we open up our **InputValidation3Activity**, we recognize a class (Divajni) that we had to use way back when in our hardcoding issues writ-eups. We can see that it uses this value to initiate our launch sequence. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5rqeewt3zenb5fe76lwl.png)

Let's open up **Divajni**. We get greeted again by soName, which we know has something to do with our **libdivajni.so** file.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/395514nktubrkkd3fswa.png)

![](https://media.giphy.com/media/fQorEj8vN8eqkNcy6T/giphy-downsized-large.gif)

Okay, now from here on we can open up our terminal and see if we can find something in our libdivajni.so file that is odd - or related to our error code. We won't have to scroll to far before we identify the culprit: **strcpy**.
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yoosf52mm3q5mr1t96m2.png)

Though we cannot access it to see how it is used, [strcpy](https://www.tutorialspoint.com/c_standard_library/c_function_strcpy.htm) is a common culprit when it comes to segmentation faults. This is because the strcpy() code is suitable handling for small inputs, but not for large ones such as the input we used for our DOS attack.

---

Congratulations, you have successfully completed the third part of the DIVA Input Validation Issues! 🥳 




 
 








  
 

 




