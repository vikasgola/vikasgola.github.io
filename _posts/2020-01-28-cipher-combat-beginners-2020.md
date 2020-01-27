---
title: Writeup for Cipher Combat Beginners 2020
tags: [CTF, Cyber Security, Hackerearth]
style: 
color: 
description: This is the writup for cipher combat ctf held on 22 january 2020. I would be discussing solutions to challenges which I was able to solve.
---

This post contains writeups for the problems of [Cipher Combat Beginners ctf](https://cybersec.hackerearth.com/). In the end I was able to achieve rank #13 against 1598 who registered for the contest. I would be showing how I solved the problems to get that rank.

## Rotate (Cryptography)

We were given a text file named rotate.txt with content given below.
```
synt = UR{Abj_Lbh_Xabj_Ubj_Gb_Ebgngr}
Abj Fhozvg Guvf.
```

It was [Rot13](https://en.wikipedia.org/wiki/ROT13) which is a letter substitution cipher that replaces a letter with the 13th letter after it, in the alphabet.

Decoding it in the bash.
```bash
$ cat rotate.txt | rot13
flag = HE{Now_You_Know_How_To_Rotate}
Now Submit This.
```

captured the flag.

## Reverse Password (Reverse Engineering)

We were given a 64 bit ELF binary file named rev which was confirmed by running `file` command on it
```bash
$ file ./rev
rev: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 3.2.0, BuildID[sha1]=4e91e84b5558a16dd5e2acee4801f62ee6819e62, not stripped
```

After executing it in the shell, it asks for the password.
```bash
$ ./rev
Please Enter The Password : aaaaaaaaaaa
Wrong Password! Try Harder..
```

Running it with `ltrace` reveals the password which is being compared and checked with the *strcmp* function.
```bash
$ ltrace ./rev
printf("Please Enter The Password : ")                                                      = 28
__isoc99_scanf(0x55a687c9f915, 0x7fffbc416c41, 0, 0Please Enter The Password : a
)                                        = 1
strcmp("a", "hackerearth")                                                                  = -7
printf("Wrong Password! Try Harder..")                                                      = 28
Wrong Password! Try Harder..+++ exited (status 0) +++
```

Let's check the password *hackerearth* if it works or not.
```bash
$ ltrace -s 1000 ./rev # -s for increasing the maximum string size to print
printf("Please Enter The Password : ")                                                      = 28
__isoc99_scanf(0x564c98b8a915, 0x7ffe255a71a1, 0, 0Please Enter The Password : hackerearth
)                                        = 1
strcmp("hackerearth", "hackerearth")                                                        = 0
printf("\n Accepted"
)                                                                       = 10
printf("\n Flag is: HE{W0rK_H@rD_pl@y_Hard3R}" Accepted
)                                             = 36
__stack_chk_fail(0x564c9a36d283, 125, 0x32bc70261, 0*** stack smashing detected ***: <unknown> terminated
 <no return ...>
--- SIGABRT (Aborted) ---
+++ killed by SIGABRT +++
```

captured the flag.

## Welcome and QR-Code! (Steganography)

In the Welcome challenge, we were given a png file. Opening the file in any image viewer shows the flag.
```
HE{Welcome_to_Cipher_Combat}
```

In the QR-Code! challenge, we were given a QR code. After decoding QR code using `zbarimg`, it gives the flag.
```bash
$ zbarimg ./find.png
QR-Code:HE{Ahhhh!_Y0u_@r3_0N_4!gH7_Tr@cK}
```

captured the flag. 