# Tom's Descend
![](https://i.imgur.com/iaW88Jt.png)
> First step is trying ' on all the parameters

![](https://i.imgur.com/Cnk3Oij.png)
>You should see SQLite3 and the error indicates SQLI
>Just go here :
>https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/SQLite%20Injection.md

> However the reason for this write-up was because I found something interesting as well

![](https://i.imgur.com/CEKb6Wh.png)

> Bad permissions allowed users to download teams.db
> However if you just use a SQLite3 DB Browser, you won't notice the metadata from the DB

![](https://i.imgur.com/p9pCo4H.png)

## SQL playload
```
' UNION SELECT * from users-- -
' UNION SELECT * from teams-- -
```

## User Agent
![](https://i.imgur.com/j2zxPtT.png)
> This hinted at the user agent
> Changing your user agent to:
```Mozilla/4.0 (compatible; MSIE 5.5; Windows 95; BCD2000)```
> Shows the second part of the flag