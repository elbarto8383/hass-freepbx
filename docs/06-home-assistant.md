# Passo 6: collega Home Assistant

Home Assistant non chiama direttamente: manda un comando a FreePBX, che apre la chiamata passando dal trunk Iliad. Un modo semplice è l'ARI, l'interfaccia REST di Asterisk, usata con un `rest_command`.

1. In FreePBX crea un utente ARI: *Settings → Asterisk REST Interface Users → Add*. Nome `hass`, password lunga, Read Only = No.
2. Controlla che il server HTTP di Asterisk sia attivo sulla porta 8088. Se usi il Firewall di FreePBX, aggiungi l'IP di Home Assistant tra le reti attendibili (*Connectivity → Firewall → Networks*).
3. Prova l'accesso da un terminale nella tua rete: `curl -u hass:PASSWORD http://IP_FREEPBX:8088/ari/asterisk/info`. Deve rispondere con un testo JSON.
4. In `configuration.yaml` aggiungi il comando e riavvia Home Assistant ([file pronto](../examples/homeassistant/rest_command.yaml)):

```yaml
rest_command:
  asterisk_chiama:
    url: "http://IP_FREEPBX:8088/ari/channels?endpoint=Local/{{ numero }}@from-internal&extension=s&context=chiamata-allarme&priority=1&callerId=Casa&timeout=40"
    method: post
    username: hass
    password: !secret ari_password
```

5. Crea l'automazione che lo usa ([file pronto](../examples/homeassistant/automation.yaml)). Il numero va scritto come lo digiteresti da un interno:

```yaml
automation:
  - alias: Chiama se scatta l'allarme
    triggers:
      - trigger: state
        entity_id: alarm_control_panel.casa
        to: triggered
    actions:
      - action: rest_command.asterisk_chiama
        data:
          numero: "3331234567"
```

Il comando fa squillare il numero passando dalla rotta del [passo 3](03-rotta-in-uscita.md). Quando qualcuno risponde, la chiamata entra nel contesto `chiamata-allarme`, che crei nel [passo 7](07-messaggio-vocale.md).

Indietro: [Passo 5](05-interno.md) · Avanti: [Passo 7, il messaggio vocale](07-messaggio-vocale.md)
