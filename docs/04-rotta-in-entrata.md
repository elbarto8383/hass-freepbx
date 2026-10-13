# Passo 4: rotta in entrata

La rotta in entrata fa squillare un interno quando qualcuno chiama il tuo numero di casa. Vai in *Connectivity → Inbound Routes → Add Inbound Route* e imposta:

- Description: `Iliad casa`.
- DID Number: il numero fisso così come lo invia Iliad, con lo 0 iniziale.
- Set Destination: *Extensions* e l'interno scelto (per esempio 1001).

Se non sei sicuro del DID, fai una chiamata di prova e cerca nel log una riga come `Executing [0XXXXXXXXXX@from-trunk:1]`: il valore prima della chiocciola è il DID. Senza una rotta dedicata la chiamata arriva comunque tramite la regola "catch-all", ma il log segnala che conviene crearne una.

Indietro: [Passo 3](03-rotta-in-uscita.md) · Avanti: [Passo 5, l'interno](05-interno.md)
