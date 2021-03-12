# Crypto / BruteForce?

Contents:
 1. [Description](#1-description)
 2. [Analysis](#2-analysis)
 3. [Decryption](#3-decryption)
 
## 1.  Description
![ ](./Resources/chall.jpg?raw=true)
<br>

>Do we really need to brute force a password out?

For this challenge, we were given a [Zip](./Resources/brute_force.zip) file.
<br>

![ ](./Resources/chall2.jpg?raw=true)
<br>

> The zip was encrypted with legacy encryption, can some sort of attacks break this?

We also unlocked a hint for this challenge which really helped us solve this! 
<br>
~~(this was the first time a ctf hint was useful :x)~~

## 2. Analysis
![ ](./Resources/file.jpg?raw=true)
<br>
Firstly, looking at the file provided, we unzipped the file and found 2 files, Desktop.zip and readme.txt
<br>
<br>
<br>
![ ](./Resources/readme1.jpg?raw=true)
<br>
Opening readme.txt, we are greeted with the string "pleasedontseethis!!!"
<br>
<br>
<br>
![ ](./Resources/desktopfile.jpg?raw=true)
<br>
We then tried to unzip Desktop.zip but the file was password protected
<br>
Taking a look at the hint, we did a simple google search on the Legacy Encryptions of Zip and found [this Writeup](https://medium.com/@alloygoh/neverlan-ctf-2018-writeups-935ce1fdb5bb).

## 3. Decryption
![ ](./Resources/pkcracksource.jpg?raw=true)
<br>
Following the writeup we found, we installed [PKCrack](https://github.com/keyunluo/pkcrack).
<br>
<br>
<br>
![ ](./Resources/pkcrackdocs.jpg?raw=true)
<br>
<br>
Refrencing the [Original Documentation](https://www.unix-ag.uni-kl.de/~conrad/krypto/pkcrack/pkcrack-readme.html) of PKCrack, we used
<br>
`./pkcrack -C /home/kali/Desktop/BruteForce/brute_force/Desktop.zip -c readme.txt -P /home/kali/Desktop/BruteForce/brute_force.zip -p readme.txt -d decrypted -a` 
<br>
<br>
<br>
![ ](./Resources/pkcrackresults.jpg?raw=true)
<br>
After PKCrack was done cracking the zip, we used cat to read flag.txt and got the flag
<br>
<br>
<br>
![ ](./Resources/catflag.jpg?raw=true)
<br>
<br>
> LNC{plain_t3xt_BRO0T_F0RCE}
