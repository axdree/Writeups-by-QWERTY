# Cryptography / 1- Can COViD steal Bob's idea?

Contents:
 1. Description
 2. Analysis
 3. Extraction
 4. Solving the Diffie-Hellman Key Exchange

## 1.  Description
Bob wants Alice to help him design the stream cipher's keystream generator base on his rough idea. Can COViD steal Bob's "protected" idea?

For this challenge, we were given a [pcapng](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob%27s%20idea/crypto-challenge-1.pcapng) file

## 2. Analysis
First off, lets take a look at the pcapng file provided to us.

![ ](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources/pcapimg1.JPG?raw=true)

Taking a quick look at the file using wireshark, we can see multiple types of traffic. Namely: ARP, TCP, DNS, UDP, ICMPV6.
<br>
<br>
<br>
Based on experience from prior CTF's, I personally find it is always good to look at and follow TCP & UDP streams first to as they normally contain the data you need. So let's take a look and follow the first TCP packet.

This can be done by right clicking the packet and selecting Follow > TCP.

![ ](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources/pcapimg2.jpg?raw=true)
![ ](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources/pcapimg3.jpg?raw=true)

Taking a look at the first TCP stream, we can see a message talking about a keystream generator inside a file., and the password to the file is the shared Diffie-Hellamn key between the two users. 
<br>
<br>
<br>
Immediately after reading this, We knew we were looking for a file. In order to find said file, let's take a look at the other TCP streams.
This can be done by increasing the stream number on the bottom right of the window.

![ ](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources/pcapimg4-2.jpg?raw=true)


![ ](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources/pcapimg5.jpg?raw=true)
![ ](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources/pcapimg6.jpg?raw=true)

From the next 2 streams, we can see that the file is downloaded through FTP. 
We can tell the file is a zip file by looking at the FTP traffic and the file header in the stream 2.

## 3. Extraction

There are multiple ways to extract files from cap/pcap/pcapng files. These are the 3 ways we tried.

**1.** Using the built in Export Objects in Wireshark. Unfortunately, this didn't work.
![ ](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources/pcapimg7.jpg?raw=true)
<br>
<br>
<br>
**2.** Using binwalk. This worked but for some odd reason our zip file did not contain anything.
![ ](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources/extractimg1.jpg?raw=true)
<br>
<br>
<br>
**3.** Manually extracting from Wireshark. This worked wonders!
![ ](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources/extractimg2-2.jpg?raw=true)
<br>
<br>
<br>
Firstly we went to stream 2 on wireshark and changed "Show and save data as" to raw. Followed by clicking "save as" and saving the file as a zip file on our computer
<br>
<br>
![ ](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources/extractimg3.jpg?raw=true)
<br>
<br>
<br>
## 
When we tried to open the file, it was password protected. 
<br>
From what we gathered earlier, the password would be the shared Diffie-Hellman key!
<br>
<br>
![ ](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources/extractimg4.jpg?raw=true)

## 4. Solving the Diffie-Hellman Key Exchange
We started off by finding out more about the diffie-hellman key exchange and found loads of infromation from [Diffie-Hellman Key Exchange (Wikipedia)](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange#:~:text=The%20Diffie%E2%80%93Hellman%20key%20exchange%20method%20allows%20two%20parties%20that,using%20a%20symmetric%20key%20cipher.)

![ ](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources//wikipost.JPG?raw=true)

From here, we had a basic understanding of how diffie-hellman worked and proceded to read writeups on past ctf's that were related to Diffie-Hellman.

Starting our calculations, we were given 3 values:

 - p = 298161833288328455288826827978944092433
 - g = 216590906870332474191827756801961881648
 - g^a = 181553548982634226931709548695881171814
 - g^b = 64889049934231151703132324484506000958

We were initally confused as g was bigger than g^a, but after clarifications from the organizers, we confirmed that g^a is equivalent to g^a mod p and vice versa for g^b.

After more research, we found out that the secret keys in Diffie-Hellman could be cracked by using the [Pohlig-Hellman algorithm](https://en.wikipedia.org/wiki/Pohlig%E2%80%93Hellman_algorithm)

![ ](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources//wikipost2.JPG?raw=true)

We can use the Pohlig-Hellman algorithm to solve for x in h = g^x mod p
<br>If we apply this with our values:
g = g^b mod p

Afterwards, we used [Sage Math](https://www.sagemath.org/) to find the shared key with the following [code](https://github.com/aadityapurani/My-CTF-Solutions/blob/master/ritsec-2018/DarkpearAI/dh-solve.sage):

    p=298161833288328455288826827978944092433
    g=216590906870332474191827756801961881648
    ga = 181553548982634226931709548695881171814
    gb = 64889049934231151703132324484506000958
    M = IntegerModRing(p)
    secreta = discrete_log(M(ga), M(g))
    secretb = discrete_log(M(gb), M(g))
    print str(IntegerModRing(p)(g)**(secreta*secretb))
    
Or we can also use this [Discrete Logarithm Calculator](https://www.alpertron.com.ar/DILOG.HTM) to manually calculate the values.
<br>
<br>
Running this code, this was the result we got!
![_](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources/sagemath.JPG?raw=true)

<br>
<br>
<br>

We tested the shared key on the zip file and it was unzipped!

![_](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources/final1.JPG?raw=true)
<br>
<br>
![_](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Cryptography%20-%201%20Can%20COViD%20steal%20Bob's%20idea/Resources/final2.JPG?raw=true)


## Flag:

    govtech-csg{246544130863363089867058587807471986686}

