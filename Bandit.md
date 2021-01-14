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

