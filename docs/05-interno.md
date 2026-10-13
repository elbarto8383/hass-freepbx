# Passo 5: crea l'interno

L'interno è il telefono o il softphone che riceve e fa le chiamate dentro casa. Vai in *Applications → Extensions → Add Extension → Add New SIP [chan_pjsip] Extension*.

1. User Extension: `1001`. Display Name: il nome che preferisci.
2. Secret: una password lunga e casuale.
3. *Submit* e *Apply Config*.

Nel telefono o softphone inserisci come server l'indirizzo IP di FreePBX, come utente `1001` e la password appena creata. Porta 5060, UDP.

Non aprire mai la porta 5060 del router verso internet. Un FreePBX con SIP esposto viene attaccato in pochi minuti: lo SIP deve restare nella rete locale.

Indietro: [Passo 4](04-rotta-in-entrata.md) · Avanti: [Passo 6, Home Assistant](06-home-assistant.md)
