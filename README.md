# FreePBX + Iliad + Home Assistant

Guida per usare la linea fissa **Iliad Fibra** con **FreePBX** (Asterisk) e far partire telefonate da **Home Assistant**, usando il numero di casa anche come normale telefono.

La guida parte da una configurazione funzionante con FreePBX 16 (Asterisk 18) e Iliad Fibra.

## Cosa si ottiene

- Chiamate in uscita e in entrata dal numero fisso di casa, con un interno SIP (telefono o softphone).
- Telefonate automatiche da Home Assistant verso qualsiasi numero, con un messaggio vocale alla risposta (per esempio un allarme).

## Schema

```mermaid
flowchart LR
    HA["Home Assistant<br/>rest_command"] -->|ARI| PBX["FreePBX<br/>Asterisk 18"]
    PBX -->|SIP| ILIAD["Iliad Fibra<br/>voip.iliad.it"]
    ILIAD -->|voce| TEL["Chi risponde<br/>cellulare o fisso"]
    PBX <-->|SIP| INT["Interno 1001<br/>telefono SIP"]
```

Home Assistant manda il comando a FreePBX, che apre la chiamata verso Iliad. Gli interni di casa usano lo stesso FreePBX per telefonare e ricevere.

## Cosa serve

- Iliad Fibra con la linea fissa attiva e i suoi dati VoIP (numero, password, server).
- Un server FreePBX 16 nella stessa rete di Home Assistant (VM o mini PC), con accesso SSH.
- Home Assistant.
- Un telefono SIP o un softphone, per esempio Linphone, Zoiper o Telephone su Mac.
- Circa due ore di lavoro.

## Indice

1. [Recupera i dati SIP della linea Iliad](docs/01-dati-sip-iliad.md)
2. [Crea il trunk Iliad in FreePBX](docs/02-trunk-freepbx.md)
3. [Rotta in uscita](docs/03-rotta-in-uscita.md)
4. [Rotta in entrata](docs/04-rotta-in-entrata.md)
5. [Crea l'interno](docs/05-interno.md)
6. [Collega Home Assistant](docs/06-home-assistant.md)
7. [Il messaggio vocale](docs/07-messaggio-vocale.md)

Inoltre:

- [Problemi comuni](docs/problemi-comuni.md)
- [Idee per sviluppare il progetto](docs/idee.md)
- [Esempi di configurazione pronti da copiare](examples/)

## Avvertenze

- Il collegamento con Home Assistant è descritto con l'ARI di Asterisk e un `rest_command`. È uno dei modi possibili: verificalo sul tuo sistema prima di affidargli un allarme.
- I nomi delle voci nella GUI di FreePBX e nell'area personale Iliad possono cambiare con gli aggiornamenti.
- Non aprire mai la porta 5060 del router verso internet: un FreePBX con SIP esposto viene attaccato in pochi minuti.
- Non pubblicare mai numero, password o indirizzi IP reali: negli esempi sono tutti segnaposto.
