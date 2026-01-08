# Pet Gallery

Oppgave:

```
login@corax ~/2_Oppdrag/2.1_Pet_Gallery $ cat LESMEG.md 
Vi har informasjon om at visse dyr i Pet Gallery kan gi oss nøkkelinformasjon. Kan du hjelpe oss med å få tilgang til de nødvendige bildene?
https://pet-gallery.ctf.cybertalent.no
```

Dette er en side med masse kjæledyrbilder. Når vi undersøker koden ser vi at hvis bildenavnet inneholder "DeW", får du ikke åpnet bildet og får i stedet "Forbidden".
Vi skal prøve å få tilgang til "Neon Cyber Cat" -> Base64 -> "TmVvbiBDeWJlciBDYXQ=". Problemet er at denne Base64-strengen inneholder "DeW"
For å komme oss rundt dette kan vi endre litt på bildenavnet. I stedet for "Neon Cyber Cat" kan vi prøve "neon cyber cat" -> Base64 -> "bmVvbiBjeWJlciBjYXQ=" ≠ "DeW".

Mjau Mjau FLAGG FLAGG

[](https://github.com/Blixat/Etterretningstjenesten_CTF/blob/main/2_Oppdrag/2.1_Pet_Gallery/neon_cyber_cat.png)
