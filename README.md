# 🔥 **GESTIONE VALVOLE by LoTableT** 🔥

Benvenuto nella guida di questo package per la gestione delle nostre valvole termostatiche!

Se ti piace il mio lavoro e per i video dove spiego i miei packages, seguimi su Tiktok e Youtube! [LINK](https://linktr.ee/lotablet) 

Videov [TIKTOK](https://vm.tiktok.com/ZNdd7rjRY/)


Questo package è stato pensato e creato per una mia neccessità e condivisione con amici, e quindi ho deciso di pubblicarlo ( ciao a Roby e Alex <3 )




Partiamo con il package: [gestione_valvole_lotablet.yaml](https://github.com/lotablet/ha-card-gestione-valvole-by-lotablet/blob/main/gestione_valvole_lotablet.yaml) <------ questo va inserito nella cartella:

```
/config/packages/
```

I componenti da installare in HACS sono:

>   - Better Thermostat UI
>   - OPZIONALE: Better Thermostat (per usare un sensore non tado, che sincronizza l'offset sulla valvola)
>   - Scheduler Component
>   - Scheduler Card
>   - Timer Bar Card
>   - Button Card
>   - Card Mod


   SCHEDULER - LA PROGRAMMAZIONE ORARIA

   Una volta installato tutto, riavviare il server di HA, aggiungere l'integrazione Scheduler Component, poi creare una nuova dashboard o una nuova scheda dove va inserita la card di Scheduler.
   La prima cosa da fare è creare una programmazione oraria in Scheduler con gli orari e temperature che abbiamo nell'app di tado© e copiarle, per ogni valvola.
   
   ![Scheduler](https://github.com/lotablet/ha-card-gestione-valvole-by-lotablet/blob/main/images/scheduler.gif)
   
   Nell'app di tado© ho disabilitato la Programmazione Intelligente, andando in "Impostazioni - Programmazione Intelligente" e impostando la fascia oraria da 00:00 a 00:00 in ogni stanza su OFF e poi in "Stanze e dispositivi" 
   impostare il "Controllo manuale del dispositivo tado" su "Finchè l'utente non lo modifica" cliccando sulla stanza (non sulla valvola).
   Cosi facendo, abbiamo escluso la Programmazione Intelligente di tado cosi non interferirà con la programmazione di Scheduler (vedi screenshot in fondo alla pagina)
   
**Una volta creata la programmazione oraria con scheduler, verrano create delle entita "switch" che attivano o disattivano la programmazione oraria, queste entità andranno inserite in "default_schedule" (vedi "Lista Scheduler Switch Valvole" nel package)


   BETTER THERMOSTAT

   Per la parte "Better Thermostat" e la configurazione del sensore esterno "non tado", vi rimando alla guida ufficiale: https://github.com/KartoffelToby/better_thermostat/blob/master/docs/Configuration/configuration.md
   Io personalmente, ho inserito un sensore di temperatura SONOFF e ho configurato Better Thermostat come da immagini allegate qui sotto nella sezione **SCREENSHOTS**
   Una volta configurato "Better Thermostat", avremmo un entità climate che andrà inserita nella lista sotto "default_climate" (vedi "Lista Valvole Termostatiche" nel package)
   OPZIONALE: Potete anche inserire un delay di "distacco" e "riarmo" se si apre una finestra nelle impostazioni di Better Thermostat, io questa parte l'ho esclusa dal package perche l'ho automatizzata tramite Node-RED.

# **CARD**

In [card.yaml](https://github.com/lotablet/ha-card-gestione-valvole-by-lotablet/blob/main/card.yaml) trovate una card con 6 valvole e i 3 input boolean creati e un timer per la modalità BOOST.
Niente di speciale, è solo una card con 3 pulsanti e un timer, ma è un buon punto di partenza per sbizzarrirvi 😁

# **Descrizione Card e Opzioni Aggiuntive/Automazioni supplementari**
La card ha 3 pulsanti ed un timer, Modalità Boost che attiva un timer da 1 ora, Modalità Away e Modalità Home, e fanno esattamente quello che farebbe l'app di Tado.

**ATTENZIONE:** Per la modalità Away e Home, vi basta creare un automazione che quando non ce nessuno in casa, si attiva la modalità Away.

Non ho voluto di proposito includere questa automazione perche è opzionale, e molto semplice da realizzare, potete usare questo codice:

Basta fare un **binary_sensor** di presenza in casa:
```
binary_sensor:
  - platform: template
    sensors:
      qualcuno_casa:
        friendly_name: 'Qualcuno a Casa'
        device_class: occupancy
        value_template: >
          {{ is_state('person.UTENTE1', 'home') or
             is_state('person.UTENTE2', 'home') or
             is_state('person.UTENTE3', 'home') }}
        icon_template: >
          {% if is_state('binary_sensor.qualcuno_casa', 'on') %}
            mdi:home-account
          {% else %}
            mdi:home-outline
          {% endif %}
```

Sostituire **UTENTE1 , UTENTE2 , UTENTE3** con i nomi utente delle persone che hanno il profilo sul vostro server di Home Assistant, con ovviamente relativo device_tracker configurato per ogni profilo.

Ecco un automazione per la gestione della presenza e spegnimento/accensione delle valvole (incollo direttamente l'automazione da inserire in Automazioni e Scenari - Automazioni)
```
alias: Gestione Presenze Valvole
description: "Quando nessuno a casa, attiva la modalità AWAY. Al rientro attiva la modalità HOME"
mode: single
triggers:
  - trigger: state
    entity_id:
      - binary_sensor.qualcuno_casa
    to: "off"
    for:
      hours: 0
      minutes: 30
      seconds: 0
    id: uscita
  - trigger: state
    entity_id:
      - binary_sensor.qualcuno_casa
    to: "on"
    for:
      hours: 0
      minutes: 10
      seconds: 0
    id: entrata
conditions: []
actions:
  - choose:
      - conditions:
          - condition: trigger
            id:
              - uscita
        sequence:
          - action: input_boolean.turn_on
            metadata: {}
            data: {}
            target:
              entity_id: input_boolean.package_valvole_away
      - conditions:
          - condition: trigger
            id:
              - entrata
        sequence:
          - action: input_boolean.turn_on
            metadata: {}
            data: {}
            target:
              entity_id: input_boolean.package_valvole_home
```

## SCREENSHOTS

**Questa è la mia configurazione con delle valvole Tado V3+**

![Config 1](https://github.com/lotablet/ha-card-gestione-valvole-by-lotablet/blob/main/images/config-1.png)

![Config 2](https://github.com/lotablet/ha-card-gestione-valvole-by-lotablet/blob/main/images/config-2.png)

**La Card**


![Card](https://github.com/lotablet/ha-card-gestione-valvole-by-lotablet/blob/main/images/cardvalvole.gif)
