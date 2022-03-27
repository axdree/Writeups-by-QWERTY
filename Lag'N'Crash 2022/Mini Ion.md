# Mini Ion

Contents:
1. [Description](#1-description)
2. [Folder Analysis](#2-folder-analysis)
3. [Getting Zip Password](#3-getting-zip-password)
4. [GIF processing](#4-gif-processing)
5. [Fixing Corrupt File](#5-fixing-corrupt-file)

## 1.  Description

The mischievous minions were caught in the jail and you are tasked to find the message to release them! 
[Download File](https://github.com/lightcoxa/Writeups-by-QWERTY/tree/main/Lag'N'Crash%202022/resources/challenge.zip)

![](https://i.imgur.com/1ywGwhD.png) 
## 2. Folder Analysis

> Inside challenge.zip, we are presented with 2 folders and a file.
```
challenge.zip/Mini Ion -LX/distrib
 ┣ Images
 ┃ ┣ 1.bob.jpg
 ┃ ┣ 2.darwin.jpg
 ┃ ┣ 3.dave.jpg
 ┃ ┣ 4.jerry.jpg
 ┃ ┣ 5.john.jpg
 ┃ ┣ 6.kevin.jpg
 ┃ ┣ 7.larry.jpg
 ┃ ┗ 8.mark.jpg
 ┣ Text
 ┃ ┣ 1
 ┃ ┃ ┣ 1.txt
 ┃ ┃ ┣ ...
 ┃ ┃ ┣ 30.txt
 ┃ ┣ ...
 ┃ ┃ ┣ 1.txt
 ┃ ┃ ┣ ...
 ┃ ┃ ┣ 30.txt
 ┃ ┣ 40
 ┃ ┃ ┣ 1.txt
 ┃ ┃ ┣ ...
 ┃ ┃ ┣ 30.txt
 ┃ ┗ script.py
 ┗ .config.7z
```

> Firstly, I took a look at the text files in the 'Text' folder and noticed that in each file, there is a base64 encoded string. I then decoded the string and found out its lorem ipsum.

![](https://i.imgur.com/DupYOOh.png)


## 3. Getting Zip Password

> Next, I wrote a simple python script to parse through all the files and decode all the strings, and print the result if the length is below 50.

```
import base64, glob, os

files = glob.glob((os.path.dirname(os.path.abspath(__file__)) + "/*/*"), recursive=True)

for file in files:
    try:
        with open(file, "r+") as f:
            data = f.read()
            out = base64.b64decode(data + "==").decode()
            if len(out) < 50:
                print(out)
    except:pass
```

![](https://i.imgur.com/hPum4xC.png)

> I then used the password from the script to unzip .config.7z to find the following files in it.

```
.config
 ┣ hello
 ┣ PASSWORD.GIF
```

## 4. GIF Processing

> Taking a look at PASSSWORD.GIF, we can see lines flickering on and off at different locations, which means that it would probably show some information when combined. 



![](https://i.imgur.com/y4aL6ua.gif)

> I then removed the white background with this simple command 

```
convert PASSWORD.gif -transparent white out.gif

```

![](https://i.imgur.com/DW6CrMs.gif)

![](https://i.imgur.com/4tsAonw.png)

> Seeing that we got a password and '.7z' it would be safe to assume that the file 'hello' is a 7z file that was encrypted with the password.

> However, if we try to add the '.7z' extension to the file and open it, we would get an error.

![](https://i.imgur.com/rIL3XiO.png)

## 5. Fixing Corrupt File

> After a simple google search, I found [this article](https://www.7-zip.org/recover.html) by 7zip on how to fix corrupt .7z files. Looking at the article, we can find an example of headers for a .7z file.

![](https://i.imgur.com/ulm9THC.png)

> If we reference 'hello' to the example, we can see some similarities. 

```
Start of archive:
0x00 0x04 (Format version) in offset 6-7

End of archive:
0x17 0x06 (End Header contains the link to Metadata Block) in offset BE-BF
```

![](https://i.imgur.com/n4wDxlY.png)

> With that, we can see that the Signature bytes are wrong. After a simple fix, we were able to extract 'hello.7z' to find the flag.

![](https://i.imgur.com/S7iz0Z6.png)

![](https://i.imgur.com/aNrPuZ4.png)

![](https://i.imgur.com/XrDkeSB.png)

> `LNC2022{Th3_de5P!ca6Le_y0U}`
