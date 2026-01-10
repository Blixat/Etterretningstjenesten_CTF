# Flagle

Oppgave:
```
login@corax ~/2_Oppdrag/2.8_Flagle $ cat LESMEG.md 

Bare gjett flagget.

'''
ssh play@flagle
'''
```

Med å logge inn som player blir vi møtt med følgene spill:

![](https://github.com/Blixat/Etterretningstjenesten_CTF/blob/main/2_Oppdrag/2.8_Flagle/Flagle.png)


Bare gjett flagget, ja… med 6 forsøk og treffe på 32 hex‑verdier? Nei takk. 
Etter å ha tittet litt rundt og testet diverse ting, oppdaget jeg noe interessant:
Hvis jeg åpner flere spill samtidig og skriver inn 000000... (32 nuller), får jeg samme treff i flere av spillene. BOOM – spillet bruker altså tid som seed!
 
Så hvis jeg åpner mange SSH‑tilkoblinger og synkroniserer terminalene, kan jeg sjekke at alle har samme seed ved å sende inn 0 og se at alle gir identisk resultat på første linje.

Når seed’en matcher er det bare å bruke all hex fra 1 til F i rekkefølge.
Og vips – du slipper å gjette.

![](https://github.com/Blixat/Etterretningstjenesten_CTF/blob/main/2_Oppdrag/2.8_Flagle/Flagle_x3.png)
