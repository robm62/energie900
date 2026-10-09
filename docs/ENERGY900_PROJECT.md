# Energy900 — centraal projectdocument

Document gestart: 2026-10-09. Status: ontwerp en verificatie; geen automatische promotie.

## Bron en bewijsregels
- Git: `robm62/energie900`, branch `main`. Baseline bij aanmaak: `690f43c5486947a715572676952994854933f6c9`.
- Onderscheid **doelontwerp**, **code aanwezig in Git**, **runtime bevestigd** en **niet geverifieerd**.
- Een Git-bestand bewijst geen geladen HA-configuratie of fysieke werking. Runtimebevindingen worden alleen met datum en bron genoteerd.
- Geen fictieve voortgang, geen automatische omschakeling, geen servicecalls, geen wijzigingen aan HA, writers of productie.

## Doelarchitectuur (ontwerp, niet als gerealiseerd beschouwen)
1. **HEMS ON**: afzonderlijk batterijsysteem met eigen writer.
2. **HEMS OFF**: afzonderlijk batterijsysteem met eigen writer.
3. **Centrale exclusieve vergrendeling**: nooit gelijktijdig actieve batterijwriters; handmatige moduskeuze, fail-safe bij ongeldige toestand.
4. **EV900**: één zelfstandige EV-module met precies één Easee-writer, met vast/variabel beleid en dynamische kwartierstrategie; handmatige moduskeuze.
5. **HEMS Dynamic** is géén derde batterijsysteem.
6. Bestaande **HEMS AUTO/Energy700** blijft operationeel tijdens shadowbouw; geen automatische promotie of productiemigratie.

## Migratiematrix (Git-baseline 2026-10-09)

| Component | Git-pad / bron | Git-bewijs | Runtime / fysieke validatie | Status |
|---|---|---|---|---|
| Core | `packages/900_energy900_core_shadow.yaml` | Aanwezig | Niet in deze controle getest | Shadow-code in Git |
| Grid | `packages/920_energy900_grid_shadow.yaml` | Aanwezig | Niet getest | Shadow-code in Git |
| PV | `packages/925_energy900_pv_shadow.yaml` | Aanwezig | Niet getest; HA-versiedrift eerder gerapporteerd, niet opnieuw bevestigd | Shadow-code in Git |
| Prijskwartieren | `packages/926_energy900_price_slots_shadow.yaml` | Aanwezig | Niet getest | Shadow-code in Git |
| Growatt | `packages/927_energy900_growatt_shadow.yaml` | Aanwezig | Niet getest | Shadow-code in Git |
| EV vast/variabel | `packages/930a_energy900_ev_fixed_shadow.yaml` | Aanwezig | Niet getest | Shadow-code in Git |
| EV dynamisch | `packages/930b_energy900_ev_dynamic_shadow.yaml` | Aanwezig | Niet getest | Shadow-code in Git |
| EV-compatibiliteit | `docs/930_ev_compatibiliteit.md` | Aanwezig, inventaris van 2026-09-20 | Referenties opnieuw te valideren | Bestaand document behouden |
| HEMS ON eigen writer | Nog niet geïdentificeerd in Git | Niet aangetoond | Niet getest | Ontwerp |
| HEMS OFF eigen writer | Nog niet geïdentificeerd in Git | Niet aangetoond | Niet getest | Ontwerp |
| Centrale batterijwriter-lock | Nog niet geïdentificeerd in Git | Niet aangetoond | Niet getest | Ontwerp |
| Zelfstandige EV900 met één Easee-writer | Niet aanwezig als afzonderlijk bestand in Git-baseline | Niet aangetoond in Git | Eerdere HA-controles rapporteerden EV-core/writer; niet opnieuw gevalideerd | Git/HA-drift te onderzoeken |
| Handmatige moduskeuze | Niet afzonderlijk geverifieerd | Niet bewezen | Niet getest | Verifiëren |
| HEMS AUTO/Energy700 continuïteit | Bestaande productie buiten deze Git-tree | Niet uit Git vast te stellen | Niet getest | Behoud is harde eis |

## Daglog

### 2026-10-09
- **Bevestigde wijzigingen:** Git-tree van `main` read-only gecontroleerd; zeven YAML-bestanden en één bestaand document aangetroffen. Dit centrale document is met expliciete toestemming nieuw aangemaakt.
- **Git-commit:** baseline vóór aanmaak `690f43c5486947a715572676952994854933f6c9`; commit van documentaanmaak: zie Git-geschiedenis (niet vooraf invullen).
- **Testbewijs:** volledige Git-tree-respons `truncated=false`; geen HA-runtime- of fysieke test in deze documentatieronde.
- **Blokkades:** HA/Git-drift, volledige writer-exclusiviteit, batterijarchitectuur, fysieke EV-validatie en actuele afhankelijkheden niet bewezen.
- **Veiligheidsstatus:** alleen Git-documentatieaanmaak; geen HA-configuratie, writer, servicecall, productie, promotie of moduswijziging.
- **Eerstvolgende actie:** read-only actuele HA-packages en writerpaden vergelijken met Git; daarna uitsluitend geverifieerde voortgang vastleggen.


### 2026-10-09 — aanvullende read-only HA/Git-audit
- **Git-baseline:** `main` bevat zeven Energy900 YAML-packages; de centrale documentatie bestaat sinds commit `e1835a3aaa690a439dddc9ac08f73f9144aa517b`.
- **HA-bestandsinventaris:** 15 actieve `packages/9xx_energy900_*.yaml` bestanden: 900, 920, 921, 925, 926, 927, 930, 930a, 930b, 931, 932, 934, 934a, 935 en 936. Git mist ten opzichte van deze inventaris 921, 930, 931, 932, 934, 934a, 935 en 936. Dit is een bestandsinventaris, geen bewezen semantische diff voor elk bestand.
- **Bevestigde HA-code:** `900` v0.2.0-standalone (read-only), `925` v0.1.4-soc-heartbeat-interlock (shadow), `930` v0.3.8-standalone-production-candidate (fysieke Easee-writer aanwezig), `930a` v0.2.0-standalone (shadow), `930b` v0.2.3-standalone (shadow-intent), `934` v0.3.2 (batterijguard met script voor Zendure-minimum-SOC). Aanwezigheid van een script betekent niet dat het tijdens deze audit is uitgevoerd.
- **Runtime bewijs (read-only entity-state, 2026-10-09):** `input_select.energy900_ev_policy_mode=Vast`; `input_boolean.energy900_ev_core_commissioned=on`; `input_boolean.energy900_ev_legacy_routes_isolated=on`; `sensor.energy900_ev_writer_status=READY_MATCH`, `requested_a=0`, `physical_phase_match=off`. `input_select.energy900_contract_mode=Vast/variabel`. `sensor.energy900_core_shadow_status=observing_hems_auto_soc_timestamp_old`, `battery_soc_age_s=37361.4`, `physical_write=false`, `battery_writer_authorized=false`.
- **Architectuur/migratiematrix:** HEMS ON eigen writer: niet bewezen; HEMS OFF eigen writer: niet bewezen; centrale batterijwriter-lock: niet bewezen. EV900 met fysieke writer: HA-code bevestigd, maar exclusiviteit over alle routes en volledige fysieke werking niet bewezen. HEMS Dynamic blijft géén apart batterijsysteem. Handmatige EV-moduskeuze bevestigd; geen automatische moduswisseling getest.
- **Blokkades:** Git/HA-versiedrift; volledigheid van EV-writer-isolatie; SOC-timestamp; `physical_phase_match=off` bij idle; ontbrekend testbewijs voor batterijwriter-exclusiviteit.
- **Veiligheidsstatus:** alleen HA-bestanden/states en Git-document gelezen. Geen servicecall, writeractie, HA-configuratie, promotie of omschakeling.
- **Eerstvolgende actie:** read-only resterende 921/931/932/934a/935/936-code en fysieke writerpaden inspecteren, daarna compatibiliteit en testbewijs aanvullen. Geen productiepromotie.

## Protocol volgende dagcontroles
Controleer Git en waar beschikbaar HA read-only. Leg datum, feitelijk gewijzigde bestanden, relevante commit-SHA, migratiematrix, testbewijs, blokkades, veiligheidsstatus en eerstvolgende actie vast. Update alleen dit bestaande document indien exacte locatie en schrijfrechten beschikbaar zijn. Label onbekende informatie als niet geverifieerd. Geen configuratiewijzigingen of automatische omschakeling.
