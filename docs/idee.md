# Idee per sviluppare il progetto

Una volta che il numero di casa è dentro FreePBX, le automazioni di Home Assistant possono usare il telefono come canale di avviso e come interfaccia della casa.

- Chiamata di allarme (intrusione, fumo, allagamento) verso uno o più numeri in sequenza.
- Avvisi vocali per eventi importanti: porta aperta, batteria scarica, temperatura fuori soglia.
- Più interni sullo stesso numero di casa: telefoni, softphone su PC e su smartphone.
- Segreteria telefonica e inoltro delle chiamate quando non sei in casa.
- Blocco dei numeri indesiderati con la blacklist di FreePBX.
- Citofono o campanello SIP che chiama gli interni.

Ogni idea riusa lo stesso schema: un'automazione chiama `sip.dial` e, alla risposta, Home Assistant legge il messaggio o apre un menu a tasti.
