# Forensics - 2 Voices in the head
## Description
We found a voice recording in one of the forensic images but we have no clue what's the voice recording about. Are you able to help?
<br>
Please view this [Document](https://github.com/lightcoxa/STF-Writeups/blob/main/Forensics/Forensics%20-%202%20Voices%20in%20the%20head/forensics-challenge-2.wav) for download instructions.
<br>
This challenge:
- Unlocks other challenge(s)
- Is eligible for Awesome Write-ups Award
- Prerequisite for Mastery Award - Forensicator
<br>
Hint:
Xiao wants to help. Will you let him help you?
## Solution
<br>
We tried using “turgen system” to extract out a hidden file but turns out it was the wrong program. Afterwards, we tried using “Sonic Visualiser” and found an image with a hash written on it when we added a spectrogram to all channels.
<br>
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Forensics/Forensics%20-%202%20Voices%20in%20the%20head/Resources/Spectrogram.png?raw=true)
<br>
We manually typed the base64 hash out ( aHR0cHM0_y9wYXN0ZWJpbi5jb20vakVUajJ1VWI= ).
<br>
Even though the hash we typed out wasn’t 100% the same we manage to make out a link to a pastebin.
<br>
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Forensics/Forensics%20-%202%20Voices%20in%20the%20head/Resources/Base64decode.png?raw=true)
<br>
We went to the pastebin link and found a programming language that has been obfuscated called brainfuck.
<br>
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Forensics/Forensics%20-%202%20Voices%20in%20the%20head/Resources/Brainfuck.png?raw=true)
<br>
Using https://www.dcode.fr/brainfuck-language we decode the encrypted text and gotten thisisnottheflag.
<br>
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Forensics/Forensics%20-%202%20Voices%20in%20the%20head/Resources/Brainfuck2.png?raw=true)
<br>
Using the hint from the question we did more research on xiao and added steganography when searching for answers. We found a software called xiao steganography
<br>
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Forensics/Forensics%20-%202%20Voices%20in%20the%20head/Resources/Xiao-stego.png?raw=true)
<br>
We proceeded to download the software and executed it.
<br>
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Forensics/Forensics%20-%202%20Voices%20in%20the%20head/Resources/Xiao-stego1.png?raw=true)
<br>
We clicked on the second option, Extract Files and import the wav file by click on “Load Source file”.
<br>
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Forensics/Forensics%20-%202%20Voices%20in%20the%20head/Resources/Xiao-stego2.png?raw=true)
<br>
Then we clicked “Next” and found that there was a zip file hidden in the wav file. However in order to extract it, we had to input a password. Using the plaintext we got from decoding brainfuck (thisisnottheflag) , we used it as the password to extract the file.
<br>
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Forensics/Forensics%20-%202%20Voices%20in%20the%20head/Resources/Xiao-stego3.png?raw=true)
<br>
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Forensics/Forensics%20-%202%20Voices%20in%20the%20head/Resources/Xiao-stego4.png?raw=true)
<br>
When opening the extracted zip file, we found that the zip file is password protected. In the zip file there is a word document. 
<br>
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Forensics/Forensics%20-%202%20Voices%20in%20the%20head/Resources/Zip+file.png?raw=true)
<br>
Initially, we thought we need to do some data carving on the zip file but after throwing the file into a hex editor we found the password written in it.
<br>
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Forensics/Forensics%20-%202%20Voices%20in%20the%20head/Resources/Hex.png?raw=true)
<br>
Using the password we gotten, we open the file and the flag was there.
<br>
![](https://github.com/lightcoxa/STF-Writeups/blob/main/Forensics/Forensics%20-%202%20Voices%20in%20the%20head/Resources/flag.png?raw=true)
<br>