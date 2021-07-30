# [Linuxcmd 101](https://cybertalents.com/challenges/forensics/linuxcmd-101) Writeup

---

## Challenge prerequisites

* linux basics commands basics
* some important tools like John the Ripper Or fcrackzip

---

### Step 1 : [download file](https://hubchallenges.s3-eu-west-1.amazonaws.com/Forensics/linux-chal.tar.gz)

![Linuxcmd 101](..\images\Linuxcmd_101_1.png "Linuxcmd 101")

we get a .tar.gz file it's used for distribution of software/media or backup purposes.

go to the downloaded directory and open terminal there

---

### Step 2 : 1st extraction

we will extract the data from ".tar.gz" using command :

```bash
7z x linux-chal.tar.gz
```

```bash
7z x linux-chal.tar
````

![Linuxcmd 101](..\images\Linuxcmd_101_2.png "Linuxcmd 101")

---

### Step 3 : 2nd extraction

when we get into the extracted folder we will get file with name **exec.zip**
when we try to extract the data from it we will asked for password

so we opened terminal in this folder and type

```bash
ls -a
```

![Linuxcmd 101](..\images\Linuxcmd_101_3.png "Linuxcmd 101")

to show all files in this directory we will get a hidden file called **pass.txt** we used this pass for unzip the file so we get the pass using ```cat``` command

```bash
cat .pass.txt
```

we get the extraction passcode
> qwerty1245

to extract it we apply this command ```-p``` follow this parameter with extraction passcode

```bash
7z x exec.zip -p"qwerty1245"
```

---

### Step 4 : 3rd extraction

change your directory into **exec** folder we will find a zip file and an executable one

run the executable

```bash
 ./- 
 ```

we get the extraction passcode
```998877665544332211```

```bash
7z x ascii.zip -p"998877665544332211"
```

here we got ascii open

![Linuxcmd 101](..\images\Linuxcmd_101_4.png "Linuxcmd 101")

---

### Step 5 : 4th extraction

change your directory into **ascii** folder we will find a zip file and a bunch of files when we tried to read one of them that give us unreadable strings so we can try them one by one or just find the file which contain readable strings using command ```file``` to recognize the file type

we found that **f6** is an ascii file and contain a pass code

>rryuiytqpyuiqyofdkhsjhfewojnhfdss

```bash
7z x size37.zip -p"rryuiytqpyuiqyofdkhsjhfewojnhfdss"
```

here we got **size37** open

![Linuxcmd 101](..\images\Linuxcmd_101_5.png "Linuxcmd 101")

---

### Step 6 : 5th extraction

change your directory into **size37** folder we will find a zip file and a bunch of files when we tried to read them we got a a readable strings seem like passwords of unzipping the file

![Linuxcmd 101](..\images\Linuxcmd_101_6.png "Linuxcmd 101")

* so we can try them one by one until **next.zip** got open by command

```bash
7z x size37.zip -p"**passcodes one by one**"
```

* or just crack it using any cracking tool like

  * John the Ripper
    1. generate wordlist from passcodes

        ```bash
        cat test* > list.txt
        ```

    2. generate the zip file hash

        ```bash
        zip2john next.zip > hash.txt
        ```

    3. crack the file

        ```bash
        john --wordlist=list.txt hash.txt

        ```

    4. we got the passcode for unzipping **next.zip**
        > 62094n902m2x-28mx4t9xy282y49ty9824yt

  * fcrackzip or any other tool

* by use folder name as a hint we can use simple command to find the size of each file

```bash
ls -al
````

![Linuxcmd 101](..\images\Linuxcmd_101_7.png "Linuxcmd 101")

here we got file with size 37 the same folder name when we read it we find the passcode and unzip **next.zip**

```bash
7z x next.zip -p"62094n902m2x-28mx4t9xy282y49ty9824yt"
```

### Step 7 : 6th extraction

change your directory into **next** folder we will find a zip file and a big wordlist we can try it by using cracking tools like the Previous extraction but after try we can't crack it by that way !!!

and if we read the wordlist file name it called **"nexttocybertalents"** so we need to find all strings after that name using the command

```bash
cat nexttocybertalents | grep cybertalent    
```

we will got that

> cybertalentsorderby1337

so the pass code is string after **"cybertalents"** the passcode is

>orderby1337

```bash
7z x NumberOne.zip -p"orderby1337"             
```

and we got **NumberOne** opened

### Step 8 : 7th extraction

change your directory into **NumberOne** folder we will find a zip file and a big wordlist called **One** so we use the same cracking method that we used in the 5th extraction so we cracked it and the passcode is

>infrastructure

![Linuxcmd 101](..\images\Linuxcmd_101_8.png "Linuxcmd 101")

```bash
7z x decodeme1.zip -p"infrastructure"
```

and we got **decodeme1** opened

### Step 9 : 8th extraction

change your directory into **decodeme1** folder we will find a zip file and a pass file contain a string look like a passcode after we try it we got error

![Linuxcmd 101](..\images\Linuxcmd_101_9.png "Linuxcmd 101")

then we try to identify if it encoded and what is encoding type using [CyberChef](https://gchq.github.io/CyberChef/ "CyberChef") with magic block we got what we expected it's base 64 encoding !!

and got the decoded pass
> usemeaspassword

![Linuxcmd 101](..\images\Linuxcmd_101_10.png "Linuxcmd 101")

```bash
7z x decodeme2.zip -p"usemeaspassword"
```

and we got **decodeme2** opened

### Step 10 : Getting the flag

change your directory into **decodeme2** folder we got the flag.txt but when we try to submit it we got wrong submit

with closer look on the flag we can notice that flag was rotated (Caesar Cipher) so we used [dcode](https://www.dcode.fr/caesar-cipher"dcode") site to decode it and finally we got the correct flag  

![Linuxcmd 101](..\images\Linuxcmd_101_11.png "Linuxcmd 101")

> flag{s1mple_linux_101}
