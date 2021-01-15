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
