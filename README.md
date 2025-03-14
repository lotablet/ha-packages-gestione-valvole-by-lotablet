 I componenti da installare in HACS sono:

   - Better Thermostat UI
   - OPZIONALE: Better Thermostat (se si vuole usare un sensore non tado che sincronizza l'offset sulla valvola)
   - Scheduler Component
   - Scheduler Card
   - Timer Bar Card
   - Button Card
   - Gap Card
   - Card Mod


   SCHEDULER - LA PROGRAMMAZIONE ORARIA

   Riavviare il server di HA, aggiungere l'integrazione Scheduler Component, poi creare una nuova dashboard o una nuova scheda dove va inserita la card di Scheduler.
   La prima cosa da fare è creare una programmazione oraria in Scheduler con gli orari e temperature che abbiamo nell'app (io uso Tado) e copiarle, per ogni valvola.

   Nell'app di Tado ho disabilitato la Programmazione Intelligente, andando in "Impostazioni - Programmazione Intelligente" e impostando la fascia oraria da 00:00 a 00:00 in ogni stanza su OFF e poi in "Stanze e dispositivi" 
   impostare il "Controllo manuale del dispositivo tado" su "Finchè l'utente non lo modifica" cliccando sulla stanza (non sulla valvola).
   Cosi facendo, abbiamo escluso la Programmazione Intelligente di tado cosi non interferirà con la programmazione di Scheduler.
   Una volta creata la programmazione oraria con scheduler, verrano create delle entita "switch" che attivano o disattivano la programmazione oraria, queste entità vanno inserite in "default_schedule" qui sopra-


   BETTER THERMOSTAT

   Per la parte "Better Thermostat" e la configurazione del sensore esterno "non tado", vi rimando alla guida ufficiale: https://github.com/KartoffelToby/better_thermostat/blob/master/docs/Configuration/configuration.md
   Io personalmente, ho inserito un sensore di temperatura SONOFF e ho configurato Better Thermostat come da immagini allegate.
   Una volta configurato "Better Thermostat", avremmo un entità climate che andrà inserita nella lista sopra "default_climate"
   ATTENZIONE: Io ho avuto problemi a configurare Better Thermostat perche alcune entità che avevo inserito come l'outdoor temperature, non erano inserite nel recorder, quindi assicurarsi che tutte le entità (temperatura, umidità etc) siamo inserite nel registro di sistema.
   OPZIONALE: Potete anche inserire un delay di "distacco" e "riarmo" se si apre una finestra nelle impostazioni di Better Thermostat, io questa parte l'ho esclusa perche l'ho automatizzata tramite Node-RED.
