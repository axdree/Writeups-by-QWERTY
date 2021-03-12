# Romeo Shane Arthur
## Description
Remeo, Shane and arthur are such talented students in my cryptography classes hope they would be able to make something that would improve the world of technology in the future

submit the flag in the LNC{secret} format

nc challenge1.lagncrash.com 16872
## Solution
Looking at the question name, if we take the first letter of each words we will get RSA. This means that the question is related to RSA. When we netcat into the server, we are presented with two values "e" and "n". The server then tell us to find the value "d".
![Netcat into the server](https://github.com/lightcoxa/Writeups-by-QWERTY/blob/main/Lag%27N%27Crash%202021/Crypto/Romeo%20Shane%20Arthur/Resource/NC.PNG)
After a quick google search, I found a Rsactftool which can calculate "e" and "n" to get "d". 
![Google search](https://github.com/lightcoxa/Writeups-by-QWERTY/blob/main/Lag%27N%27Crash%202021/Crypto/Romeo%20Shane%20Arthur/Resource/Google%20search.PNG)
I git clone the tool into my kali and run the tool with argument "python3 Rsactftool.py -e (value) -n (value) --uncipher --dumpkeys" as seen in the second picture.
![Rsactftool help](https://github.com/lightcoxa/Writeups-by-QWERTY/blob/main/Lag%27N%27Crash%202021/Crypto/Romeo%20Shane%20Arthur/Resource/RSActftool%20help.PNG)
![Rsactftool execution](https://github.com/lightcoxa/Writeups-by-QWERTY/blob/main/Lag%27N%27Crash%202021/Crypto/Romeo%20Shane%20Arthur/Resource/RSActftool%20output.PNG)
Afterwards, I copy the "d" value and submit the answer to get the flag.
![flag](https://github.com/lightcoxa/Writeups-by-QWERTY/blob/main/Lag%27N%27Crash%202021/Crypto/Romeo%20Shane%20Arthur/Resource/Flag.PNG)
