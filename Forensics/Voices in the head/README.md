# Forensics
## Challenge
## Solution
We tried using “turgen system” to extract out a hidden file but turns out it was the wrong program. Afterwards, we tried using “Sonic Visualiser” and found an image with a hash written on it when we added a spectrogram to all channels.
![]()
We manually typed the base64 hash out ( aHR0cHM0_y9wYXN0ZWJpbi5jb20vakVUajJ1VWI= ).
![]()
Even though the hash we typed out wasn’t 100% the same we manage to make out a link to a pastebin.
![]()
We went to the pastebin link and found a programming language that has been obfuscated called brainfuck.
![]()
Using https://www.dcode.fr/brainfuck-language we decode the encrypted text and gotten thisisnottheflag.
![]()
Using the hint from the question we did more research on xiao and added steganography when searching for answers. We found a software called xiao steganography
![]()
We proceeded to download the software and executed it.
![]()
We clicked on the second option, Extract Files and import the wav file by click on “Load Source file”.
![]()
Then we clicked “Next” and found that there was a zip file hidden in the wav file. However in order to extract it, we had to input a password. Using the plaintext we got from decoding brainfuck (thisisnottheflag) , we used it as the password to extract the file.
![]()
When opening the extracted zip file, we found that the zip file is password protected. In the zip file there is a word document. 
![]()
Initially, we thought we need to do some data carving on the zip file but after throwing the file into a hex editor we found the password written in it.
![]()
Using the password we gotten, we open the file and the flag was there.
