# Passo 6: collega Home Assistant (metodo consigliato)

Per far telefonare Home Assistant si usa l'integrazione [hass-sip](https://github.com/eigger/hass-sip), che si registra su FreePBX come un normale **interno**. Quando un'automazione la chiama, FreePBX apre la telefonata verso Iliad e, appena qualcuno risponde, Home Assistant legge un messaggio con il TTS. Non serve registrare l'audio né scrivere il dialplan.

Con questo metodo la guida è stata provata con una chiamata di prova verso un cellulare: la chiamata è arrivata e il messaggio si è sentito.

## 1. Crea un interno per Home Assistant

In FreePBX vai in *Applications → Extensions → Add Extension → chan_pjsip*:

- User Extension: un numero libero, per esempio `1002`. Display Name: `Home Assistant`.
- Secret: una password lunga e casuale.
- Scheda *Advanced*: Direct Media = `No`.
- Scheda *Codecs*: lascia attivi `alaw`, `ulaw` e, se presente, `g722`.
- DTMF: `RFC 4733` (può comparire come RFC 2833). Serve solo se vuoi usare i tasti del telefono.

Poi *Submit* e *Apply Config*. Queste impostazioni sono di buon senso per far passare bene l'audio: il README di hass-sip non le indica.

## 2. Installa l'integrazione

1. In HACS apri il menu in alto a destra → *Repository personalizzati* e aggiungi `https://github.com/eigger/hass-sip` come tipo *Integrazione*.
2. Cerca "SIP" in HACS, scarica l'integrazione e riavvia Home Assistant.
3. Vai in *Impostazioni → Dispositivi e servizi → Aggiungi integrazione* e cerca **SIP Client**.
4. Compila: Server = IP di FreePBX, Porta = `5060`, Username = `1002`, Password = quella dell'interno. Dominio e Caller ID sono facoltativi. Porta RTP: lascia `7078`.

Se usi il Firewall di FreePBX, aggiungi l'IP di Home Assistant tra le reti attendibili, altrimenti la registrazione può fallire.

Home Assistant e FreePBX devono stare nella stessa rete (o in una VPN). Non aprire mai la porta 5060, né la porta RTP, verso internet.

## 3. Controlla la registrazione

Da SSH sul server FreePBX:

```
asterisk -rx "pjsip show contacts"
```

L'interno `1002` deve risultare registrato. In Home Assistant, in *Strumenti per sviluppatori → Stati*, compare un'entità `media_player.sip_...` e dei sensori: tra questi c'è quello del percorso audio (`none`, `no_rx`, `no_tx`, `bidirectional`), utile se manca l'audio.

## 4. Prima chiamata di prova

In *Strumenti per sviluppatori → Azioni*, passa alla modalità YAML e usa un tuo numero:

```yaml
action: sip.dial
data:
  number: "3331234567"
  message: Ciao, questa è una prova
  tts_engine: tts.google_translate
target:
  entity_id: media_player.IL_TUO_ID
```

Al posto di `tts.google_translate` usa il motore TTS che hai in Home Assistant. Il campo `target` può essere anche un `device_id`, come fa l'editor visuale. Altri campi di `sip.dial`: `ring_timeout`, `language`, `tts_options`, `menu`.

## 5. Usala in un'automazione

[File pronto](../examples/homeassistant/automation.yaml):

```yaml
automation:
  - alias: Chiama se scatta l'allarme
    triggers:
      - trigger: state
        entity_id: alarm_control_panel.casa
        to: triggered
    actions:
      - action: sip.dial
        data:
          number: "3331234567"
          message: Attenzione, sta suonando l'allarme di casa
          tts_engine: tts.google_translate
        target:
          entity_id: media_player.IL_TUO_ID
```

Il numero va scritto come lo digiteresti da un interno: la chiamata esce dalla [rotta del passo 3](03-rotta-in-uscita.md).

Indietro: [Passo 5](05-interno.md) · Alternativa senza integrazione: [metodo ARI](07-alternativa-ari.md)
