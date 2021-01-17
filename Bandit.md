# Bandit

## Level 0 

SSH - Connect to bandit.labs.overthewire.org : Port 2220 : Username bandit0 : Pass bandit0

ssh <username>@<remote> 
Add flag -p 0000 with port of desired port 
```
Ssh bandit0@bandit.labs.overthewire.org -p 2220 
-ls 
Cat readme 
```
**Output:** boJ9jbbUNNfktd78OOpsqOltutMc3MY1


Note: Copying text between a guest vm and to a host with virtual box 
	Devices —> Shared Clipboard

## Level 0 -> 1

Login to Bandit1

ctrl + d  - Logs out of ssh

```
ssh bandit1@bandit.labs.overthewire.org -p 2220

Pass: boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

## Level 1 -> 2 

The password for the next level is stored in a file called - located in the home directory

Need to learn how to open a "-" directory 

```
cat < ./-
```
**Output:** CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9

## Level 2 -> 3 

The password for the next level is stored in a file called spaces in this filename located in the home directory

See a file: 
*spaces in this filename*

to work with wrap in quotatations ' ' or use \ before each space 

```
cat spaces\ in\ this\ filename
```

**output:** UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK

## Level 3 -> 4 

The password for the next level is stored in a hidden file in the inhere directory.

```
cd inhere
ls -a 
cat .hidden
```

**Output:** pIwrPrtPN36QITSp3EQaw936yaFoFgAB


## Level 4 -> 5

The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.

found files -file00 through to -file09

Inside files contain some sort of unknown code

```
 cat ./-file00 ./-file01 ./-file02 ./-file03
```

Above code outputs some but want to concatenate all files without having to write out. 

cat * will outlist all files in current directory  

```
cat ./-file*
```

**Result:**
```
�/`2ғ�%��rL~5�g��� �������p,k�;��r*��   �.!��C��J       �dx,�e�)�#��5��
                                                                       ��p�?��r�l$�?h�9('���!y�e�#�x�O��=�ly���~��A�f����-E�{���m�����ܗMkoReBOKuIDDepwhWk7jZC0RTdopnAYKh
�T�?�i��j���îP�F�l�n��J����{��@�e�0$�in=��_b�5FA�P7sz��gN
```

These don't work. Something to do with the human readable format. Is there a way I can find that? 

Find & File Commands 

strings Command outputs human-readable text from files

```
strings ./-file0* 
```

**Output:**koReBOKuIDDepwhWk7jZC0RTdopnAYKh

## Level 5 -> 6

The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

human-readable
1033 bytes in size
not executable

ls finds a 20 differnet directories.
Find command? 
find -readable -size 1033 ! -executable 

Googling brought me to something similar:
```
find . -type f -readable -size 1033c ! -executable 
```

Although it looks like the flag -readable only refers to files that can be read by the account

-size 1033c uses c instead of b for the file size. Apparently b is for 512 byte blocks. c is for actual bytes 

```
find -size 1033c ! -executable
```

Found ./maybehere07/.file2

**Output:** DXjZPULLxYr17uwoI01bNLQbtFemEgo7

## Level 6 -> 7

The password for the next level is stored somewhere on the server and has all of the following properties:

owned by user bandit7
owned by group bandit6
33 bytes in size

Looks like it may be another find command 

```
find / -group bandit6 -user bandit7 -size 33c
```

printed out lists of lots of permission denied but can see one file /var/lib/dpkg/info/bandit7.password that looks interesting 

**Output:** HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs

## Level 7 -> 8

The password for the next level is stored in the file data.txt next to the word millionth

Lots of text in the file, going to need to sort through with some sort of regex or some other command

[Great regex article](https://www.howtogeek.com/661101/how-to-use-regular-expressions-regexes-on-linux/)

```
grep -e 'miilionth' data.txt
```

**Output:** cvX2JJa4CFALtqS87jk27qwqGhBM9plV

## Level 8 -> 9

The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

Thinking that I want to use sort command to get rid of duplicates.

used *sort -u data.txt* and was still left with too many results 

tried *sort -u -i data.txt* and still too many

```
cat data.txt | sort | uniq -u
```

**Output:** UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR

Note: uniq -u filters adjacent matching therefore you must sort the input before using it.

## Level 9 -> 10

The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

Used *strings data.txt* to get human-readable strings

```
strings data.txt | grep '=' 
```
**Output:** truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk

## Level 10 -> 11

The password for the next level is stored in the file data.txt, which contains base64 encoded data

Linux has the base64 command to encode/decode data nad print to standard output 

```
cat data.txt | base64 -d 
```

**Output:** IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR

## Level 11 -> 12

The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

Looks like a variatation of a Caesar cipher. 
Refered to as ROT13 can be deciphered by encrypting again.

```
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

**Output:** 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu

## Level 12 -> 13

The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

xxd can be use to hexdump and reverse with the flag -r 

```
strings data.txt | xxd -r | strings
```

```
Result:
	data2.bin
	BZh91AY&SY
	{RBp
	5(3A
	7{qP
	3A4$
	q       \)
	BB9<s 
```

While it doesn't look like a password flag is in there the data2.bin makes me sus

Had problem so looked for help. command file gives us some information about the file. *file data.txt* is a gzip conpressesd file. 
Convert it to .gz

Decompress with *gzip -d data.gz*

It is a bzip2 compressed data so change the file to .bz then use *bzip2 -d data.bz*

In gzip form again. Decrypt again
Says it is in POSIX tar archive
*tar -xf data.tar* 
Gives a data5.bin which is a tar archine 
Removing it from that gives a bzip2
Removing that gives a tar
Removing that gives data8.bin which is a gzip file
Removing that and checking the file states ASCII text 

**Output:** 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL

## Level 13 -> 14 

The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on

Have access to the file sshkey.private
If I go into /etc/bandit_pass I have permission denied if trying to read bandit14. Running file on it tells me it is a regular file, no read permission. 

checking out s_client command

```
s_client -connect localhost:2220 - key ~/sshkey.private
```

trying to use an get the message s_client command not found 

```
ssh -i sshkey.private bandit14@localhost 
```

After gaining access with this I can go find the password in /etc/bandit_pass/bandit14

**Output:** 4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e


## Level 14 -> 15

The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

Thinking first off that maybe I can submit the password to -port 30000 using nc.

```
nc bandit15@localhost -p 30000 < '4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e'
```

Doesn't like that it isn't a file or directory 

nmap localhost shows that 30000 is open with a service of ndmps

```
	echo '4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e' | nc localhost 30000
```

**Output:** BfMYroe26WYalil77FoDi9qh59eK5xNr

## Level 15 -> 16

The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.

Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…

nmap local host and couldnt see the port 30001
Thinking i might have to use the openssl command.

```
openssl s_client -connect localhost:30001
```

-ign_eof inhibits shutting down the connection when end of ile is reach in the input 

```
openssl s_client -connect localhost:30001 -ign_eof
```
Putting in this whole command gave out a Correct! and the password

**Output:** cluFn7wTiGryunymYOu4RcffSxQluehd

## Level 16 -> 17

The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

Thinking by looking at that first off I'll have to do a port search ranging from 31000 to 32000. 

```
nmap localhost -p31000-32000

Returned: 
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

	
```

So i'm guessing it has to be one of these, now to find out what services run on them.

added -sV scan to previous command and its taking awhile. 

```
nmap localhost -sV -p31000-32000

Returned:
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
```

Looking at this i feel that any service with echo will just echo back the password we give it so I will try starting with port 31790

```
echo 'cluFn7wTiGryunymYOu4RcffSxQluehd' | openssl s_client -connect localhost:31790 -ign_eof
```

The returned output gave me the correct but not a password but rather a RSA private key

```
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

```

So I need to save this as a file and use it to login to the next level. By echoing it into a file named bandit17.key in /tmp/mykey

I got a warning that the permissions on bandit17.key is too open 

```
	chmod 600 /tmp/mykey/bandit17.key
```

Succesfully logged in. cat /etc/bandit_pass/bandit17

**Output:** xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn

## Level 17 -> 18

There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19

First thing that came to mind was to pipe both files into the same one and sorting them then using uniq, however I think the diff command may do it quicker

```
diff passwords.new passwords.old

Returns:
42c42
< kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
---
> w0Yfolrc5bwjS4qw5mq1nnQi6mF03bii

```

Gets me two different potential passwords 

**Password:** kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
