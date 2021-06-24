---
layout: post
title:  "New Caesar | PicoCTF 2021"
date:   2021-03-20 13:30:00 +0800
categories: CTF PicoCTF Cryptography
---

## New Caesar
#### Category: Cryptography

Code Given: `ihjghbjgjhfbhbfcfjflfjiifdfgffihfeigidfligigffihfjfhfhfhigfjfffjfeihihfdieieih`

Looked through the python file to try and understand briefly first. There seems to be a conversion to base 16 and then a shift.

Looking further into the key I noticed it has to be a single character.
To try and guess the key. I made flag equal `abcdefghijklmnopqrstuvwxyz0123456789_`. And then started guessing the key character by brute force.

Notice that each character converts into a two character counterpart with the first character representing like a 10's and the second being a unit.

Looking at the code given from these 2 characters there only appears 4 starting characters. `f, j, i, h`. Rotatating the brute force found `c` to give these 4 characters.

Then made a python file to reverse the encryption process
``` py
import string

key = "c"
cipher = "ihjghbjgjhfbhbfcfjflfjiifdfgffihfeigidfligigffihfjfhfhfhigfjfffjfeihihfdieieih"
LOWERCASE_OFFSET = ord("a")
ALPHABET = string.ascii_lowercase
ALPHABET_16 = string.ascii_lowercase[:16]

def un_shift(c, k):
    t1 = ord(c) - LOWERCASE_OFFSET
    t2 = ord(k) - LOWERCASE_OFFSET
    return ALPHABET_16[(t1-t2) % len(ALPHABET_16)]

def base16_decode(cipher):
    enc = ""
    for i in range(0, int(len(cipher)/2)):
        t1 = ord(cipher[i*2]) - LOWERCASE_OFFSET
        t2 = ord(cipher[i*2+1]) - LOWERCASE_OFFSET
        binary = "{0:04b}".format(t1) + "{0:04b}".format(t2)
        enc += chr(int(binary, 2))
    return enc

enc = ""
for i, c in enumerate(cipher):
    enc += un_shift(c, key[i % len(key)])

print(base16_decode(enc))
```

Running this gives the following: `et_tu?_0797f143e2da9dd3e7555d7372ee1bbe`
So flag is `picoCTF{et_tu?_0797f143e2da9dd3e7555d7372ee1bbe}`
