# Passo 7: il messaggio vocale

Quando qualcuno risponde alla chiamata, Asterisk riproduce un messaggio registrato, ripetuto due volte.

1. Prepara l'audio: scrivi il testo (per esempio "Attenzione, sta suonando l'allarme di casa") e generalo con un sintetizzatore vocale oppure registralo.
2. Caricalo in FreePBX da *Admin → System Recordings → Add Recording*, con il nome `allarme-casa`. FreePBX lo converte nel formato giusto.
3. Aggiungi in fondo a `/etc/asterisk/extensions_custom.conf` questo contesto ([file pronto](../examples/asterisk/extensions_custom.conf)):

```
[chiamata-allarme]
exten => s,1,Answer()
 same => n,Wait(1)
 same => n,Playback(custom/allarme-casa)
 same => n,Wait(1)
 same => n,Playback(custom/allarme-casa)
 same => n,Hangup()
```

4. Ricarica il dialplan con `fwconsole reload`.
5. Prova la chiamata senza Home Assistant, da SSH:

```
asterisk -rx "originate Local/3331234567@from-internal extension s@chiamata-allarme"
```

Se il telefono squilla e senti il messaggio, il resto è solo l'automazione di Home Assistant.

Indietro: [Passo 6](06-home-assistant.md) · Vedi anche: [Problemi comuni](problemi-comuni.md)
