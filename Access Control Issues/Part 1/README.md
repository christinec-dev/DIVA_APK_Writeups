# Part One
If you've seen any of my previous [write-ups](https://github.com/christinec-dev/DIVA_APK_Writeups) on the DIVA APK's, you would know that today we are going to cover the last and final section: Access Control Issues. Access Control Issues arise when we, as normal users, can gain access to data that we are not suppose to access either directly or via malicious methods. This is mostly due to poor data/access protection mechanisms put in place by developers. 🤠

Now, with this section there are three parts. Without any further lollygagging, let's jump into it!

![gif](https://media.giphy.com/media/GOQCP2d9LQYBfhO5p8/giphy.gif)

## Access Control Issues - Part One
When we open the Access Control Issues - Part 1 section on our device we are met with the following objective: try to access the API credentials from outside the app. This means that instead of just clicking the **View API Credentials** button, we should try and access the credentials on the activity using other methods, such as via the terminal.

![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gabmb2s4167y5ncnfkww.png)
 
For coverage sake, this is what happens when we do press the View API Credentials button directly. We can see that we get instant access to credentials that we shouldn't have access to!
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/o2oj5vgt4bi87gilb6gm.png)

Okay, let's start with the fun things. Let's see if we can see what happened in our **LogCat** after we opened the api credentials. LogCat is powerful since it can reveal useful information for us as the attacker, such as the activity that was opened, which we can use to exploit. Open up your terminal using CTRL + ALT + T and enter the following command.

```
adb shell logcat
```
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5o2d1fpqnpwehrxxtu1u.png)
 ![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1lxzav5m6z94722lfmgd.png)
 
We can see that it opens an activity called .**APICredsActivity**. Let's open **jadx.gui** and see if we can see where the activity pulls the credentials from.
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/74b8ydgsvimmazbybxw6.png)
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wx6dr5o2kcp0os99tvgh.png)
  
Okay, so the data is hardcoded. Now that we know which activity is used to store the hardocded api credentials, we can use the terminal to bypass the "View API Credentials" button and show us the credentials directly. In other words, we will start the activity's [Intent](https://developer.android.com/reference/android/content/Intent) directly from the terminal. 👾

```
adb shell am start -n jakhar.aseem.diva/.APICredsActivity
```

- `am`is used to manage the activity.

- `start` is used to start the activity.

- `-n` is used to indicate the name of the activity to open (.APICredsActivity).

![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/71a6axz1d86pe0gmy29o.png)

Hooray! When we go back to our application we can see that we have successfully opened the activity and revealed the credentials without pressing the button!
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3gvjh4clgiazk5fp5gcr.png)

---


 
 

 

