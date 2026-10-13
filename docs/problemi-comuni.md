# Problemi comuni

Per quasi tutti i problemi il log in tempo reale mostra la causa: `tail -f /var/log/asterisk/full`. Per vedere anche i messaggi SIP scambiati con Iliad usa `asterisk -rx "pjsip set logger on"` e spegnilo a fine prova con `off`.

| Sintomo | Causa probabile | Cosa fare |
| --- | --- | --- |
| Chiamando senti "numero non in uso" | Pattern nella colonna *prefix* della rotta: il numero arriva vuoto e Iliad risponde `484 Address Incomplete` | Metti i pattern solo in *match pattern* ([passo 3](03-rotta-in-uscita.md)) |
| Con `+39` la chiamata non parte ("extension not found in context from-internal") | Manca la riga con prefix `+39` e match `X.` | Aggiungi la riga ([passo 3](03-rotta-in-uscita.md)) |
| Il trunk non risulta `Registered` | Credenziali errate, transport o IPv6 non attivi | Ricontrolla utente, password e transport ([passo 2](02-trunk-freepbx.md)) con `pjsip show registrations` |
| Le chiamate in entrata non arrivano all'interno | Rotta in entrata assente o DID diverso da quello inviato da Iliad | Leggi il DID nel log e correggi la rotta ([passo 4](04-rotta-in-entrata.md)) |
| Home Assistant non fa partire la chiamata | Interno `1002` non registrato, o firewall di FreePBX che blocca Home Assistant | Controlla `pjsip show contacts` e le reti attendibili ([passo 6](06-home-assistant.md)). Con il metodo ARI: ripeti il `curl` dell'[alternativa](07-alternativa-ari.md) |
| Audio assente o a senso unico | Problema di RTP, NAT o firewall IPv6 | Controlla il firewall del router e del server, poi rifai la prova |
| Chi chiama non sente il "tuu tuu" mentre l'interno squilla | Nei log FreePBX invia il segnale di squillo, quindi dipende dalla rete di chi chiama o da Iliad | La chiamata funziona comunque; prova da un altro operatore |
