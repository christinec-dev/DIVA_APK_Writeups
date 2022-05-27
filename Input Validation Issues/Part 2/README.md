# Input Validation Issues - Part Two

With another day gone, it is time for another Android Pen-test write-up. Today we are going to cover the fourth section of the DIVA APK, [Input Validation Issues](https://cwe.mitre.org/data/definitions/20.html). When we have an application that does not validate input properly, it makes it easier for an attacker to go ahead and creating input that is not expected by the rest of the application. This has dire consequences, ranging from altered data, arbitrary code execution, or unauthorized data access. Not good!

When you're ready, put on your favorite hoodie and grab your nearest drink, and let's get HACKING! 👾

![Image description](https://media.giphy.com/media/damUMYvgrroqw2hxSu/giphy.gif)

When we open the Input Validation Issues - Part 2 section on our device we are met with the following objective: try accessing any sensitive information apart from a web URL.

Let's take note of the important part in this objective, which is to **NOT** access information from a web URL - so don't go trying to hack Google or your favorite site! We need to access local data. 😂 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/e806eivncmvyz1q1bnjb.png)
 
Now, before we continue, I need to confess something. I made an oops and had to clean install all of my tools. This means that all those tmp files and shared_prefs we created in the pervious writeups are all gone. Not to worry, because I'm going to work around it. 🤠

Let's go into our APK and see what sensitive data we can exploit. If you aren't me, and you still have your tmp file, you can easily use that file. I will instead create a "secret" file that will contain some user data. We will then use this file to see if we can access it in the application. Open up your terminal and do the following:

```
adb shell
su
cd data/data/jakhar.aseem.diva/
echo "password:123; username:alex" > private.txt
cat private.txt
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/raw14l7z3x9ix8brph92.png)
 
with our file created, and our sensitive data stored locally on our device, we can now go back to our application and try to access our **private.txt** file via input. Let's navigate to that file via:

```
file:///data/data/jakhar.aseem.diva/private.txt
```
When we hit view, we can see that our data is revealed! 🙃

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ox64rf432bzu0tq6ajmg.png)

---

Congratulations, you have successfully completed the second part of the DIVA Input Validation Issues! 🥳 




 
 








  
 

 




