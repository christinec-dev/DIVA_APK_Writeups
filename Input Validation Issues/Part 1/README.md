# Input Validation Issues - Part One

With another day gone, it is time for another Android Pen-test write-up. Today we are going to cover the fourth section of the DIVA APK, [Input Validation Issues](https://cwe.mitre.org/data/definitions/20.html). When we have an application that does not validate input properly, it makes it easier for an attacker to go ahead and creating input that is not expected by the rest of the application. This has dire consequences, ranging from altered data, arbitrary code execution, or unauthorized data access. Not good!

When you're ready, put on your favorite hoodie and grab your nearest drink, and let's get HACKING! 👾

![Image description](https://media.giphy.com/media/damUMYvgrroqw2hxSu/giphy.gif)

When we open the Input Validation Issues - Part 1 section on our device we are met with the following objective: try to access all user data without knowing any username. There are three users by default and your task is to output data of all three users with a single malicious search.

Let's take note of the key to this objective: **malicious search**. We also see a little hint, which tells us that there are three users in the database, where one is an admin.
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lk96qmecr1t3l0xq6w5x.png)

Since we are working with a database, we are most likely going to have to create a [SQL Injection](https://portswigger.net/web-security/sql-injection), but before we get to that, let's see what happens if we enter any username (without knowing the true values).

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/m5tcm0qs6fse9lbsrbdt.png)

When we enter an **random/guessed** value, we can see that no user gets returned. Yet, when we enter **admin**, we can see that it returns the details of the admin user! 👀

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dfazly696qyfomaamrte.png)
 
Now that we know that admin is most definitely a user, we can use this to construct our SQL Injection command. But, before we do this, I want to cheat a little bit and go snoop around in our database files to see if we can see the three users that we need to return with our command.

If you want to do this, open up your terminal via CTRL + ALT + T and enter the following commands:

```
adb shell
su
cd data/data/jakhar.aseem.diva/databases

> qlite3 (To enter this option start typing sql, press TAB + Enter)
> .open sqli
> .tables
> .table sqliuser
> .dump sqliuser
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/j54vs8eslj7deesot4h7.png)
 
We can see that we need to construct a command to return the users admin, diva, and john. Head back into your application, because we are about to write the most genius, original, most hackery SQL Injection command ever!

```
''admin' OR 1=1;--
```

- `'` indicates the start of our query
- `'admin'` we know this is already a user in the database.
- `OR 1=1;` since 1=1 is always true, the query will return all items. 
- `--` comments out the rest of our query. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/anjd2kldfy260wipjpr7.png)
 
DUHN DUHN DUUUUUHN, we've successfully dumped their database table! Wasn't that fun? 😀

---

Congratulations, you have successfully completed the first part of the DIVA Input Validation Issues! 🥳 




 
 








  
 

 




