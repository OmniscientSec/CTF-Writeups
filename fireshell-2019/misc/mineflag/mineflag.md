# Mineflag

## Prologue

Really fun challenge, don't see a lot of challenges like this in CTFs

## Analyzing the map

When we enter the map we see:

![Iron door](https://i.imgur.com/agZTaaA.png)

Behind ourselves there is a massive wall with a lot of levers:

![Passcode wall](https://i.imgur.com/pSWmBC1.png)

The levers were turned off when I entered the map, this is the correct passcode.

The circuit analyzes the password basing on each line's redstone strength at the end

![Redstone circuit](https://i.imgur.com/0Y0PqBF.jpg)

All we had to do is count each line's redstone count and enter the corresponding character:

![Password analysis](https://i.imgur.com/LlIHixv.jpg)

The password was:

```
6D316E33
```

Decoding this from hex, we get: ```min3```

The book said to md5 this and enter it as ```F#{Flag_md5}```

## Flag

```
F#{e817a228276fc0afb778763c46aa302f}
```







