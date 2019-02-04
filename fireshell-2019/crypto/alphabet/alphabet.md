# Alphabet

## Prologue

This challenge wasn't a hard one, found the solution on Google, just had to make some changes.

## Ciphertext

Ciphertext contained hashes like:

```
148de9c5a7a44d19e56cd9ae1a554bf67847afb0c58f6e12fa29ac7ddfca9940 e1671797c52e15f763380b45e841ec32
```

Quick google search returned rainbow tables on SHA-256 and MD5.

Quickly making a rainbow-table and feeding the file through it returns a long file, so we just grepped it and got the flag.

## Source code

```python
import hashlib
f = open('ct.txt', 'r')

md5s = {}
sha256s = {}
for i in range(256):
        md5s[hashlib.md5(chr(i)).hexdigest()] = chr(i)
        sha256s[hashlib.sha256(chr(i)).hexdigest()] = chr(i)

text = ""

a = f.read().split(" ")
for c in a:
        if c in md5s:
                text += md5s[c]
        if c in sha256s:
                text += sha256s[c]
print text
```

## Flag

```
Congratulations!_T#e_Flag_Is_F#{Y3aH_Y0u_kN0w_mD5_4Nd_Sh4256}
```



