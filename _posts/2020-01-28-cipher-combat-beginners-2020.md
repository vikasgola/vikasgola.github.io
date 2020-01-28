---
title: Writeup for Cipher Combat Beginners 2020
tags: [CTF, Cyber Security, Hackerearth]
style: 
color: 
description: This is the writup for cipher combat ctf held on 22 january 2020. I would be discussing solutions to challenges which I was able to solve.
---

This post contains writeups for the problems of [Cipher Combat Beginners ctf](https://cybersec.hackerearth.com/). In the end, I was able to achieve rank #13 against 1598 who registered for the contest.

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

captured the flag!!!

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

captured the flag!!!

## Welcome (Steganography), QR-Code! (Steganography), and ReadMe (MISC)

In the Welcome challenge, we were given a png file. Opening the file in any image viewer shows the flag.
```
HE{Welcome_to_Cipher_Combat}
```

In the QR-Code! challenge, we were given a QR code. After decoding QR code using `zbarimg`, it gives the flag.
```bash
$ zbarimg ./find.png
QR-Code:HE{Ahhhh!_Y0u_@r3_0N_4!gH7_Tr@cK}
```
captured the flag!!!

A text file was given in ReadMe challenge that contains the flag.
```bash
$ cat readme.txt
Copy This Flag and Submit to get Points.

HE{YoU_are_Absolute_Beginner}

-------------------------------------
```
captured the flag!!!

## Ping (Android)

A apk named ping.apk was given. I didn't run it because of my laziness and don't know what this apk does.

However, I converted it to jar file using dex2jar and we get a ping-dex2jar.jar file.
After, opening it in jdgui and checking MainActivity file, we get this.

{% include elements/figure.html image="https://vikasgola.github.io/assets/2020-01-28-cipher-combat-begginers-PING-challenge.png" caption="jdgui Screenshot" %}

captured the flag!!!

## CrackME (MISC)

A zip file was given named Zipper.zip which asks for password at the extraction time.

After trying *password*  (I mean why not?) as password it gets accepted.
```bash
$ unzip Zipper.zip
Archive:  Zipper.zip
[Zipper.zip] SuperSecret password: 
 extracting: Zipper/SuperSecret
```

Let's see what is in the SuperSecret file.
```bash
$ cat SuperSecret
HE{You_@re_@_Zip_Cracker}
```
captured the flag!!!

## Play and Win (Reverse Engineering)

A ELF binary file was given named TicTacToe.

Runing strings on it.
```bash
$ strings TicTacToe | grep "HE" # grep for only printing the line which contains "HE" string
	HE{You_are_a_tic_tac_toe_Ch@mpion}
```

captured the flag!!!
