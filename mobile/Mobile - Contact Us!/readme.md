# Contact Us!
## Description
Looks like Korovax has exposed some development comment lines accidentally. Can you get one of the secrets through this mistake?</br>

The challenge gives this [apk](./mobile-challenge.apk) file, since an apk file is nothing more than zip file, unzipping it gives us the jar/smali/class files.</br>
![Alt text](./resources/unzip.jpg?raw=true "Unzip")

Use grep to find any flags that is in plaintext, 3 flags are shown.</br>
![Alt text](./resources/flag_3.jpg?raw=true "Unzip")

Not knowing which challenge the flags belong to, I started my android emulator(Ldplayer).</br>
If you are interested in mobile, I suggest using adb with android studio, but Ldplayer or BlueStacks works just fine.</br>
Got a little confused with the UI but navigated to the menu</br>
![Alt text](./resources/menu.jpg?raw=true "Unzip")

Viewing "contact us":
![Alt text](./resources/contact.jpg?raw=true "Unzip")
It looks like it's asking for some input in the name.

This time I used strings on the binary again and found this:
![Alt text](./resources/challenge_1.jpg?raw=true "Unzip")

Inputting "Give me the flag" to the name parameter:
![Alt text](./resources/flag_1.jpg?raw=true "Unzip")
