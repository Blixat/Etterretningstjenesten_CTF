# Chiffermelding

Oppgave:
```
login@corax ~/1_grunnleggende/1.5_Chiffermelding $ nc chiffermelding 1337 
Her har du en hemmelig melding:
o mt f id o gi r ig a nø a gm r em f ne o hr r ae a reaenrldrsjekedeg
- Signert av Roscher Lund

Finn ut hva den opprinnelige meldingen var, og fortell meg hvem som skrev den

>
```

Det er veldig typisk at "Chiffermelding"–oppgaver i den grunnleggende kategorien er en type substitusjonssiffer, veldig ofte ROT13, så det var det første jeg tenkte på å prøve.
Men det var det ikke – det var faktisk et Skip Cipher, noe jeg måtte lete litt fram til.

Når man prøver seg fram, ser man at når man gjør 3-stegs hopp, kommer teksten "Oforaarforaar...(https://www.google.com/search?q=Oforaarforaar&sca_esv=512b3b0f1298b71e&sxsrf=AE3TifNJB5tM-AI2TtmfYGH2VPV111Qz4g%3A1767824636651&source=hp&ei=_NxeaeGUJtvKwPAPjvHg8AY&iflsig=AOw8s4IAAAAAaV7rDCebwDYTScAczeP2wHM1972XUaH3&ved=0ahUKEwjhgvf7u_qRAxVbJRAIHY44GG4Q4dUDCB4&uact=5&oq=Oforaarforaar&gs_lp=Egdnd3Mtd2l6Ig1PZm9yYWFyZm9yYWFyMgcQIxiwAhgnMggQABiiBBiJBTIIEAAYgAQYogQyBRAAGO8FMggQABiABBiiBDIFEAAY7wVIz54CUABYlZ0CcAF4AJABAJgBRaABhwGqAQEyuAEDyAEA-AEC-AEBmAICoAJRqAIKwgIHECMYJxjqApgDCPEF6tIlCde-Md-SBwEyoAeABrIHATG4B0nCBwMyLTLIBw2ACAA&sclient=gws-wiz)"  => "O Foraar! Foraar!" => Henrik Wergelands dikt. 

Med å prøve fra seg oppover, ser man atnår man gjør 3 stegs hopp kommer det fram "Oforaarforaar...." => "O foraar! foraar!" => Henrik Wergelands 

```
> Henrik Wergeland
> FLAGG
```
