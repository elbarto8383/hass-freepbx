# Passo 3: rotta in uscita

La rotta in uscita decide quali numeri digitati dagli interni vanno sul trunk Iliad. Senza una regola che li riconosca, FreePBX risponde che il numero non è raggiungibile.

Vai in *Connectivity → Outbound Routes → Add Outbound Route*. Nome: `iliad`, Trunk Sequence: `iliad`. Nei Dial Patterns inserisci tre righe, lasciando vuote le colonne indicate:

| prepend | prefix | match pattern | Serve per |
| --- | --- | --- | --- |
| vuoto | vuoto | `0.` | fissi nazionali (es. 081...) |
| vuoto | vuoto | `3XXXXXXXXX` | cellulari |
| vuoto | `+39` | `X.` | numeri digitati con il prefisso +39 |

La colonna *prefix* contiene la parte che FreePBX **toglie** dal numero. Se ci scrivi il pattern al posto del prefisso, il numero arriva vuoto a Iliad, che risponde `484 Address Incomplete` e tu senti "numero non in uso".

Salva con *Submit* e *Apply Config*. Poi prova una chiamata dall'interno e controlla i log con `tail -f /var/log/asterisk/full`: l'INVITE verso Iliad deve contenere il numero chiamato.

Indietro: [Passo 2](02-trunk-freepbx.md) · Avanti: [Passo 4, la rotta in entrata](04-rotta-in-entrata.md)
