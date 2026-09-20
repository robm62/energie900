# Energy900 EV-compatibiliteit (read-only inventaris, 20-09-2026)

## Bestaande bediening behouden

| Functie | Bestaande bron / contract | Energie900-opbouw |
|---|---|---|
| Voertuig-SOC en vertrekdoel | `sensor.ev_actuele_voertuig_soc`, `input_number.ev_doel_soc`, route-/vertreksensoren op EV-dashboard | 930a/930b lezen bestaande invoer; geen dashboardwijziging in shadow |
| Override (ook geplande start en duur) | `510_ev_override_manager.yaml`, `timer.ev_laad_override`, `input_datetime.ev_override_geplande_start`, bestaande scripts | Bestaande bediening blijft werken; shadow observeert |
| PV- en directe verzoeken | `635_ev_request_contract.yaml` (prioriteit safety 100, override 90, direct 70, PV 40) | Vergelijk nieuwe adviezen met bestaande verzoeken |
| Fase-/stroomcap | `sensor.ev_netbalancer_safe_current`, 640-gateway, 920-faseobservatie | Nieuwe cap eerst onafhankelijk vergelijken |
| Dynamisch goedkoop kwartierplan | `199bc_ev_exact_quarter_plan.yaml` en prijsbeleid | 930b apart; meerdere goedkope blokken en vertrekhaalbaarheid toetsen |
| Easee-opdracht | `660_ev_writer.yaml` en fasepad `639b_ev_phase_writer_candidate.yaml` | Geen fysieke opdracht in 930a/930b; later een writer |
| EV-batterijinterlock | `639d_ev_battery_hard_interlock.yaml` en UI-automation | Ontladen tijdens werkelijk EV-laden in beide modi blokkeren; laden bij vast/variabel blokkeren, bij dynamisch Zendure laten beslissen. Fysieke werking nog te testen |

## Dashboard-afhankelijkheden

Actieve dashboards `ev-charge` en `kia-ev6` gebruiken bestaande EV-entiteiten en override-scripts. Bestaande entiteiten blijven tijdens shadow intact. Een nieuwe naam vervangt pas een oude na een expliciet geteste dashboardmigratie.

## Openstaande verificaties

- Volledige live-writerinventaris, inclusief UI-automations en fasewissel.
- Exacte effectieve vertrekcontext en overrideprioriteit uit de huidige dashboards en packages.
- Contractinstelling in Zendure-app is niet bewezen uitleesbaar; de Energy900-rekenmodus schakelt geen fysieke policy.
- Onder werkelijk EV-laden ontladen/laden, Easee-readback, fasebelasting en herstel na sessie meten.
- Voor dynamische planning horizon, all-in prijs, bruikbare laadcapaciteit en meerdere blokken afzonderlijk bewijzen.

Geen HA-bestanden of fysieke output worden door dit document gewijzigd.
