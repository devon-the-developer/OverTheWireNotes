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

## Level 1
Login to Bandit1

ctrl + d  - Logs out of ssh

```
ssh bandit1@bandit.labs.overthewire.org -p 2220

Pass: boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

## Level 2 
The password for the next level is stored in a file called - located in the home directory

Need to learn how to open a "-" directory 

```
cat < ./-
```
**Output:** CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
