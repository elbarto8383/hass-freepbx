# Passo 2: crea il trunk Iliad in FreePBX

Il trunk è il collegamento tra FreePBX e Iliad: si registra con il tuo numero e resta attivo giorno e notte.

1. Vai in *Connectivity → Trunks → Add Trunk → Add SIP (chan_pjsip) Trunk*.
2. Nome del trunk: `iliad`. In *Outbound CallerID* inserisci il tuo numero fisso.
3. Nella scheda *pjsip Settings → General* imposta: Username = numero fisso, Secret = password Iliad, Authentication = Outbound, Registration = Send, SIP Server = `voip.iliad.it`, porta 5060.
4. Nella scheda *Codecs* lascia solo `alaw`: nelle chiamate osservate nei log Asterisk offriva a Iliad soltanto PCMA.
5. Nella scheda *Advanced* imposta Outbound Proxy = `sip:proxy-voip-2.iliad.it\;lr` ed Expiration = 90 secondi.
6. *Submit* e poi *Apply Config*.

Nella configurazione di partenza registrazione e chiamate viaggiano su IPv6. Verifica che il server FreePBX abbia un indirizzo IPv6 e che il transport UDP sia attivo anche su `[::]` (*Settings → Asterisk SIP Settings → Chan PJSIP Settings*), poi selezionalo nel trunk.

Controlla che la registrazione sia riuscita da SSH:

```
asterisk -rx "pjsip show registrations"
```

Lo stato deve essere `Registered`. Per le chiamate in entrata FreePBX di norma aggiunge da solo i parametri `line` ed `endpoint` alla registrazione, così Asterisk associa la chiamata al trunk giusto.

Indietro: [Passo 1](01-dati-sip-iliad.md) · Avanti: [Passo 3, la rotta in uscita](03-rotta-in-uscita.md)
