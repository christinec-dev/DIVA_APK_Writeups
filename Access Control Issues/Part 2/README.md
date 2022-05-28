# Part Two
If you've seen any of my previous [write-ups](https://github.com/christinec-dev/DIVA_APK_Writeups) on the DIVA APK's, you would know that today we are going to cover the last and final section: Access Control Issues. Access Control Issues arise when we, as normal users, can gain access to data that we are not suppose to access either directly or via malicious methods. This is mostly due to poor data/access protection mechanisms put in place by developers. 🤠

Now, with this section there are three parts. Without any further lollygagging, let's jump into it!

![gif](https://media.giphy.com/media/GOQCP2d9LQYBfhO5p8/giphy.gif)

## Access Control Issues - Part Two
When we open the Access Control Issues - Part Two section on our device we are met with the following objective: try to access the API credentials from outside the app without knowing the pin. This means that we cannot go ahead and just view the registered credentials via the interface, nor can we register new credentials, but we should try and access the credentials on the activity using other methods, such as via the terminal (just like before).
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dqkuenw4mt5xsakgymmy.png)

---

For interest sake, this is what happens when we click the **already registered** button:

![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/62xia3y3brfoawc1d8qk.png)
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k808e06296o8lhqlmck5.png)
 
We can see that we get immediate access to the API credentials. When we click the register now button, we are prompted to enter a pin after registering. We cannot register, as this feature does not exist! 😆
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3m6l43dlvbez7ee46jdt.png)
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2zj4hqtrnvwxcruuqt2f.png)
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tc222y5pfoe4s41kijkg.png)

---

Okay, as we learnt previously **LogCat** will show us everything we need to know. Let's open up our terminal and run the same command as before.
```
adb shell logcat
```
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jhld6yr9xu3d5jtj0j31.png)

We can see that it returns the .**APICreds2Activity** activity that opened the API credentials layout when we launched the section layout.Let's head into **JADX-GUI** and open up this activity to see what we can find in the source code.
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jui1do21oiycim143q9n.png)

As we can see, this value was also hardcoded - but this time there is a difference. We can see that we need a pin to access the credentials, whereas previously we had direct access. We will need to bypass this pin check.
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/nlr7pikj7t8sp0nk6qgf.png)

Okay, from here on we need to disable our authentication checks so that we can just view our ./APICreds2Activity activity without having to do a pin check. I recommend having a look at this [ADB Command List](https://gist.github.com/Pulimet/5013acf2cd5b28e55036c82c91bd56d8) before you continue. Go back to your terminal and enter the following command:
```
adb shell am start -n jakhar.aseem.diva/.APICredsActivity -a jakhar.aseem.diva.action.VIEW_CRED2 --ez check_pin false
```

- `am`is used to manage the activity.

- `start` is used to start the activity.

- `-n` is used to indicate the name of the activity to open (.APICredsActivity).

- `-a` is used to view our credentials. It's syntax elaborates better: -a <ACTION>. 

- `--ez check_pin false` is used to bypass the checks made by the application at the receiving side so we don't need a pin.

![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lruwaekc0p2jcv4mohrc.png)

And as easy as 123, when we head back to our application we can see that our activity launched. We have completed our activity objective!
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5lrftea5d07bnbz23gus.png)

---


 
 

 

