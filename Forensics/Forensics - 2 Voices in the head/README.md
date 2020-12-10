# Forensics - 2 Voices in the head
## Description
We found a voice recording in one of the forensic images but we have no clue what's the voice recording about. Are you able to help?

Please view this [Document](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Forensics+-+2+Voices+in+the+head/forensics-challenge-2.wav) for download instructions.

This challenge:
- Unlocks other challenge(s)
- Is eligible for Awesome Write-ups Award
- Prerequisite for Mastery Award - Forensicator

Hint:
Xiao wants to help. Will you let him help you?
## Solution
We tried using “turgen system” to extract out a hidden file but turns out it was the wrong program. Afterwards, we tried using “Sonic Visualiser” and found an image with a hash written on it when we added a spectrogram to all channels.
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Forensics+-+2+Voices+in+the+head/Resources/Spectrogram.png?raw=true)
We manually typed the base64 hash out ( aHR0cHM0_y9wYXN0ZWJpbi5jb20vakVUajJ1VWI= ).

Even though the hash we typed out wasn’t 100% the same we manage to make out a link to a pastebin.
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Forensics+-+2+Voices+in+the+head/Resources/Base64decode.png?raw=true)
We went to the pastebin link and found a programming language that has been obfuscated called brainfuck.
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Forensics+-+2+Voices+in+the+head/Resources/Brainfuck.png?raw=true)
Using https://www.dcode.fr/brainfuck-language we decode the encrypted text and gotten thisisnottheflag.
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Forensics+-+2+Voices+in+the+head/Resources/Brainfuck2.png?raw=true)
Using the hint from the question we did more research on xiao and added steganography when searching for answers. We found a software called xiao steganography
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Forensics+-+2+Voices+in+the+head/Resources/Xiao-stego.png?raw=true)
We proceeded to download the software and executed it.
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Forensics+-+2+Voices+in+the+head/Resources/Xiao-stego1.png?raw=true)
We clicked on the second option, Extract Files and import the wav file by click on “Load Source file”.
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Forensics+-+2+Voices+in+the+head/Resources/Xiao-stego2.png?raw=true)
Then we clicked “Next” and found that there was a zip file hidden in the wav file. However in order to extract it, we had to input a password. Using the plaintext we got from decoding brainfuck (thisisnottheflag) , we used it as the password to extract the file.
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Forensics+-+2+Voices+in+the+head/Resources/Xiao-stego3.png?raw=true)
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Forensics+-+2+Voices+in+the+head/Resources/Xiao-stego4.png?raw=true)
When opening the extracted zip file, we found that the zip file is password protected. In the zip file there is a word document. 
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Forensics+-+2+Voices+in+the+head/Resources/Zip+file.png?raw=true)
Initially, we thought we need to do some data carving on the zip file but after throwing the file into a hex editor we found the password written in it.
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Forensics+-+2+Voices+in+the+head/Resources/Hex.png?raw=true)
Using the password we gotten, we open the file and the flag was there.
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Cryptography/Forensics+-+2+Voices+in+the+head/Resources/flag.png?raw=true)
