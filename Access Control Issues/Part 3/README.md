# Part Three
If you've seen any of my previous [write-ups](https://github.com/christinec-dev/DIVA_APK_Writeups) on the DIVA APK's, you would know that today we are going to cover the last and final section: Access Control Issues. Access Control Issues arise when we, as normal users, can gain access to data that we are not suppose to access either directly or via malicious methods. This is mostly due to poor data/access protection mechanisms put in place by developers. 🤠

Now, with this section there are three parts. Without any further lollygagging, let's jump into it!

![gif](https://media.giphy.com/media/GOQCP2d9LQYBfhO5p8/giphy.gif)

## Access Control Issues - Part Three
When we open the Access Control Issues - Part Three section on our device we are met with the following objective: try to access the private notes from outside the app without knowing the pin. This means that we cannot go ahead and just create a pin to access the notes, but we should try and access the notes using other methods (not necessarily by launching the activity as prior), such as via the terminal (we know Christine, we know). 

![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/h4x84rghwdszfql52a1v.png)
 
---

For interest sake, this is what happens when we **register a pin** (I entered a basic 1234 pin):

![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/d9miq4xbwl7wnrz1e1ct.png)
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jzut2prpsqw91igxzbox.png)
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qkqs2ufoumfy8xrda3pa.png)
   

---

Let's open up our terminal and see what activity our **LogCat** reveals to us. 

```
adb shell logcat
```
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/r9tjjowlgp5mnj8q0dim.png)
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mqgek9punbyzbb79q7xb.png)

We can see that it logs two activities, ./**AccessControl3Activity** and ./**AccessControl3NotesActivity**. Let's open up our **JADX-GUI **and have a look at both.

![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8g6ooe8zyo5djp3kiswu.png)
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5ih3e7zt506i1edy7idj.png)

We can see our AccessControl3Activity stores our pin via a **SharedPreferences** object, which we covered way back when. When we enter the pin saved in shared_prefs, it launches the AccessControl3NotesActivity activity which validates this pin before showing the notes via a **query(NotesProvider.CONTENT_URI)** [content query](https://developer.android.com/guide/topics/providers/content-provider-basics). This content provider will dump all of the notes, and allow us to meet our objective.

We can _dump_ this [content provider](https://stackoverflow.com/questions/27988069/query-android-content-provider-from-command-line-adb-shell) via the following command in our terminal:
```
adb shell am content query --uri content://jakhar.aseem.diva.provider.notesprovider/notes/
```
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/aqg90dcn1sdol3acpu1d.png)
 
Thus we have accessed all the notes from outside of the application, without having to register for a pin or launch the activity as before.
![DIVA Access Control Issues](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l48uv1csj7a1jdr4mu7u.png)
 
---

## Conclusion
Congratulations, you have successfully completed all the sections of the DIVA APK! 🥳

![gif](https://media.giphy.com/media/vvz5AVSb96m0FY9XFb/giphy.gif)

I hope this was easy enough to follow/understand. If you have recommendations on any cool tools, techniques, or tutorials that I too can follow feel free to leave them below and I'll check it out! 😊


 
 

 

