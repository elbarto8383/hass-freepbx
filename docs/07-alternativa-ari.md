# Alternativa: chiamare con l'ARI di Asterisk

Questo è un altro modo per far telefonare Home Assistant, senza l'integrazione hass-sip del [passo 6](06-home-assistant.md). Home Assistant manda un comando all'ARI (l'interfaccia REST di Asterisk) con un `rest_command`, e un piccolo dialplan riproduce un messaggio registrato. È più manuale: usalo se non vuoi installare l'integrazione.

## 1. Utente ARI e firewall

1. In FreePBX crea un utente ARI: *Settings → Asterisk REST Interface Users → Add*. Nome `hass`, password lunga, Read Only = No.
2. Controlla che il server HTTP di Asterisk sia attivo sulla porta 8088. Se usi il Firewall di FreePBX, aggiungi l'IP di Home Assistant tra le reti attendibili.
3. Prova da un terminale nella tua rete: `curl -u hass:PASSWORD http://IP_FREEPBX:8088/ari/asterisk/info`. Deve rispondere con un testo JSON.

## 2. Comando in Home Assistant

In `configuration.yaml` ([file pronto](../examples/alternativa-ari/rest_command.yaml), con la password in [secrets](../examples/alternativa-ari/secrets.yaml.example)), poi riavvia:

```yaml
rest_command:
  asterisk_chiama:
    url: "http://IP_FREEPBX:8088/ari/channels?endpoint=Local/{{ numero }}@from-internal&extension=s&context=chiamata-allarme&priority=1&callerId=Casa&timeout=40"
    method: post
    username: hass
    password: !secret ari_password
```

Nell'automazione usa `rest_command.asterisk_chiama` con `numero: "3331234567"`.

## 3. Il messaggio vocale

Quando qualcuno risponde, Asterisk riproduce un messaggio registrato.

1. Registra o genera l'audio e caricalo da *Admin → System Recordings → Add Recording* con il nome `allarme-casa`.
2. Aggiungi in fondo a `/etc/asterisk/extensions_custom.conf` ([file pronto](../examples/alternativa-ari/extensions_custom.conf)):

```
[chiamata-allarme]
exten => s,1,Answer()
 same => n,Wait(1)
 same => n,Playback(custom/allarme-casa)
 same => n,Wait(1)
 same => n,Playback(custom/allarme-casa)
 same => n,Hangup()
```

3. Ricarica con `fwconsole reload`.
4. Prova da SSH:

```
asterisk -rx "originate Local/3331234567@from-internal extension s@chiamata-allarme"
```

Se il telefono squilla e senti il messaggio, il resto è solo l'automazione.

Indietro: [Passo 6](06-home-assistant.md) · Vedi anche: [Problemi comuni](problemi-comuni.md)
