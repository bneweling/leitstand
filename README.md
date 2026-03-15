# Leitstand

**Persoenliches PM-Cockpit fuer IT-Sicherheitsberater**

Single-File HTML-Anwendung zur Steuerung von 10-20 parallelen Projekten im Bereich BSI IT-Grundschutz, ISMS und IT-Audits. Laeuft direkt im Browser per Doppelklick -- kein Server, kein Build, kein Admin-Recht.

---

## Schnellstart

1. `leitstand.html` im Browser oeffnen (Chrome/Edge empfohlen)
2. Einstellungen oeffnen (Zahnrad unten links)
3. Unter "Mein Profil" den eigenen Namen eintragen
4. Optional: Demo-Daten laden (Einstellungen > Datei laden > `leitstand-demo.json`)
5. Optional: KI-Endpunkt konfigurieren (siehe [KI-Setup](#ki-setup))

---

## Features

### Steuerung

| Feature | Beschreibung |
|---------|-------------|
| **Mein Tag** | Taegliche Todo-Liste mit KI-Tagesplanung. AP hinzufuegen, Stunden buchen, Notizen erfassen. Sync zurueck in Projekte beim Abhaken. |
| **Cockpit** | Dashboard mit Alarm-Leiste, 4 Stat-Cards, Prioritaetsliste "Was brennt?", Meilenstein-Timeline, Follow-ups, Projekt-Karten mit Ampeln. |
| **Board** | Kanban-Board ueber alle Projekte. 5 Spalten (Offen/In Arbeit/Delegiert/Review/Erledigt). Drag-and-Drop zum Status-Wechsel. |
| **Kalender** | Wochen-gruppierte Ansicht mit Meilensteinen, AP-Deadlines und Outlook-Terminen. ICS-Import mit UID-basiertem Diff-Merge. |

### Daten

| Feature | Beschreibung |
|---------|-------------|
| **Projekte** | Mit Ampel (auto-berechnet), Rolle (Leiter/Stellv./Mitglied), Prioritaet, BSI-Vorgehensweise. |
| **Arbeitspakete** | Status, Delegation mit 5 Leveln, Soll/Ist-Stunden, 8/80-Regel-Warnung, Wiedervorlage, Vollstaendigkeitsindikator. |
| **Personen** | Team + Kundenkontakte + Externe. Verfuegbarkeit, Schwerpunkte, Anrede fuer E-Mail-Generierung. |
| **Meilensteine** | Harte Termine mit Status (offen/erreicht/gefaehrdet). |
| **Risiko-Register** | Wahrscheinlichkeit x Auswirkung Matrix, Massnahmen, Verantwortlicher. |
| **Notizen** | Entscheidungen, Risiken, allgemeine Notizen pro Projekt. |
| **Termine** | Outlook-ICS-Import mit UID-Matching. Projekt-Zuordnung per Dropdown. |

### KI-Copilot (n8n + Anthropic Claude)

| Aktion | Beschreibung |
|--------|-------------|
| **Aufgabe zerlegen** | WBS-Zerlegung nach 100%-Regel und 8/80-Regel. BSI-Baustein-Kenntnisse. |
| **Delegation vorschlagen** | Personen-Matching nach Schwerpunkten. Copy-Paste-Delegationstexte. |
| **Priorisierung** | Eisenhower-Matrix + kritischer Pfad. |
| **Wochenbericht** | Zusammenfassung ueber alle Projekte. |
| **Tag planen** | KI schlaegt Top-Aufgaben fuer heute vor (max 6-8h). |
| **Transkript auswerten** | Meeting-Protokoll rein, AP + Entscheidungen + Risiken raus. |
| **E-Mail entwerfen** | Use-Case-basiert (Status, Eskalation, Nachfrage...) mit Kontext-Editor. |
| **Nachfass-Text** | Follow-up fuer ueberfaellige Delegationen, Ton passt zum Level. |
| **Meeting vorbereiten** | Agenda, Talking Points, offene Fragen, Risiken. |
| **NL-Kommandozeile** | Freitext-Eingabe wird zu Projekt-Aenderungen. Per Sprache oder Text. |

### Weitere Features

- **Gantt-Chart** (SVG) im Projektdetail mit Abhaengigkeiten und Heute-Marker
- **Baseline-Vergleich** -- "Was hat sich seit letzter Woche geaendert?"
- **Globale Suche** -- Findet alles ueber alle Entity-Typen
- **Statusbericht-Generator** -- Copy-Paste-fertiger Projektbericht
- **BSI-Vorlagen** -- 6 Templates (ISMS-Aufbau, Standard-Absicherung, Kern-Absicherung, Risikoanalyse, Baustein-Umsetzung, Audit), editierbar in Einstellungen
- **BlueAnt CSV-Import** -- Projekte, AP, Personen, Meilensteine
- **Spracheingabe** -- Web Speech API (Chrome/Edge, Deutsch)
- **Autosave** -- localStorage + FileDB + OPFS Recovery

---

## Use Cases

### UC1: Morgens den Tag starten (5 Minuten)

1. App oeffnen -- startet in **Mein Tag**
2. **"KI: Tag planen"** klicken -- KI schlaegt 5-7 Aufgaben vor basierend auf Deadlines, Prio und Terminen
3. Vorschlaege durchgehen, passende uebernehmen, ggf. anpassen
4. Optional: Eigene Todos ergaenzen (AP waehlen oder freie Todos)
5. Im Laufe des Tages: Abhaken, Stunden und Notizen eintragen

**Ergebnis:** Strukturierter Arbeitstag, gebuchte Stunden fliessen automatisch in die Projekte zurueck.

### UC2: Neues ISMS-Projekt aufsetzen

1. Projekt anlegen (Name, Kunde, Rolle: Leiter, Vorgehensweise: Standard)
2. Projektdetail oeffnen -- **BSI-Vorlage "ISMS-Aufbau"** anwenden
3. 12 Arbeitspakete werden automatisch angelegt (Scope bis Management Review)
4. Meilensteine ergaenzen (Zertifizierungstermin, Audittermine)
5. Personen zuweisen ueber KI: **"Delegation vorschlagen"**
6. Kundenkontakt als Person anlegen (Rolle: Kundenansprechpartner, Anrede: Herr/Frau)

**Ergebnis:** Projekt vollstaendig aufgesetzt mit AP-Struktur nach BSI 200-1/2, Verantwortlichkeiten, Meilensteinen.

### UC3: Wochentliches Kunden-Statusupdate

1. Projektdetail oeffnen -- **"Statusbericht"** klicken
2. Bericht wird generiert: Ampel, Fortschritt, Soll/Ist, erledigte AP, offene Risiken, naechste Schritte
3. Text kopieren und in E-Mail einfuegen, oder:
4. **"E-Mail entwerfen"** klicken -- Use Case "Status-Update" waehlen, Empfaenger waehlen, Tonalitaet einstellen
5. KI generiert E-Mail mit Projektkontext -- Text bearbeiten, kopieren, ab in Outlook

**Ergebnis:** Professionelles Statusupdate in 2 Minuten statt 20.

### UC4: Delegation und Nachverfolgung

1. AP oeffnen -- Verantwortlichen zuweisen
2. **Delegationslevel** waehlen (z.B. "Beraten -- Mach das, weil...")
3. Wiedervorlage-Datum setzen (z.B. in 5 Tagen)
4. Optional: KI generiert passenden Delegationstext zum Kopieren
5. Nach Ablauf der Wiedervorlage: Follow-up erscheint automatisch im Cockpit und Projektdetail
6. **"Nachfass-Text"** klicken -- KI generiert freundliche Erinnerung passend zum Delegationslevel

**Ergebnis:** Nichts faellt durch, Delegation mit System statt Bauchgefuehl.

### UC5: Meeting auswerten

1. Nach dem Meeting: Projektdetail oeffnen -- **"Transkript auswerten"**
2. Meeting-Protokoll oder Transkript in das Textfeld einfuegen
3. KI extrahiert automatisch: Arbeitspakete, Entscheidungen, Risiken, Follow-ups
4. Jeden Vorschlag einzeln pruefen und per Klick uebernehmen
5. AP landen im Projekt, Entscheidungen als Notizen, Risiken im Register

**Ergebnis:** Kein "was hatten wir nochmal besprochen?" -- alles sofort strukturiert erfasst.

### UC6: Per Sprache oder Freitext steuern

1. Projektdetail -- In das Textfeld tippen oder Mikrofon klicken
2. Sprechen: *"Die Carolin soll bis Freitag die ORP.1 Richtlinie fertig haben, hoch priorisiert. Ausserdem brauchen wir einen Meilenstein am 30. April fuer den Audit."*
3. KI interpretiert und zeigt strukturierte Kommandos als Vorschau: update_ap + create_milestone
4. Pruefen, ggf. einzelne abwaehlen, "Alle anwenden"

**Ergebnis:** Natuerliche Sprache wird zu Projektaenderungen -- keine Formulare noetig.

### UC7: Outlook-Kalender synchronisieren

1. In Outlook: Kalender als .ics exportieren
2. In Leitstand: Kalender-View -- **"Outlook aktualisieren"** klicken, .ics waehlen
3. Termine werden importiert -- neue hinzugefuegt, geaenderte aktualisiert, entfallene markiert
4. Bei jedem Termin: Projekt per Dropdown zuordnen (bleibt bei Re-Import erhalten)
5. Naechste Woche: Neue .ics laden -- nur Aenderungen werden uebernommen

**Ergebnis:** Outlook-Termine und Leitstand-Daten auf einer Zeitachse, ohne doppelte Pflege.

### UC8: BSI-Baustein umsetzen

1. Projektdetail -- BSI-Vorlage **"Baustein-Umsetzung"** anwenden
2. 7 AP werden angelegt: Richtlinie, Ist-Aufnahme, Gap-Analyse, Massnahmen, Schulung, Nachweise, Review
3. Oder: **"KI: Zerlegen"** klicken und Baustein-Nummer nennen -- KI erzeugt spezifische AP
4. Soll-Stunden schaetzen, Verantwortliche zuweisen
5. Im Verlauf: Ist-Stunden buchen (ueber "Mein Tag" oder direkt im AP)
6. Fortschritt und Soll/Ist-Abweichung im Projektdetail sichtbar

**Ergebnis:** Strukturierte Baustein-Umsetzung mit Nachvollziehbarkeit.

### UC9: Risiken managen

1. Projektdetail -- **"+ Risiko"** oder KI erkennt Risiko im Transkript
2. Wahrscheinlichkeit und Auswirkung bewerten (niedrig/mittel/hoch)
3. Massnahme und Verantwortlichen zuweisen
4. Risikomatrix im Projektdetail zeigt alle Risiken visuell
5. Im Statusbericht werden offene Risiken automatisch aufgelistet

**Ergebnis:** Strukturiertes Risikomanagement nach BSI 200-3 statt Bauchgefuehl.

### UC10: Daten aus BlueAnt importieren

1. In BlueAnt: Projekte/AP/Personen als CSV exportieren
2. In Leitstand: Einstellungen -- **BlueAnt Import** -- Typ waehlen (Projekte/AP/Personen/MS)
3. Spalten werden automatisch erkannt (Name, Deadline, Aufwand, etc.)
4. Import-Reihenfolge: Personen, dann Projekte, dann AP/MS (damit Zuordnungen greifen)
5. Duplikate werden per Name erkannt und uebersprungen

**Ergebnis:** Bestehende BlueAnt-Daten in 5 Minuten im Leitstand, ohne Abtippen.

---

## KI-Funktionen im Detail

### Vorhandene KI-Aktionen

#### Analyse & Planung

| Aktion | Wo | Input | Output | Anonymisiert |
|--------|-----|-------|--------|:---:|
| **decompose_task** | Projektdetail: "KI: Zerlegen" | Projekt + vorhandene AP | AP-Vorschlaege mit Aufwand, Reihenfolge, Personenempfehlung | Ja |
| **suggest_delegation** | Projektdetail: "KI: Delegation" | Offene AP + Personen mit Schwerpunkten | Zuweisungsvorschlaege mit Delegationslevel + Copy-Paste-Text | Ja |
| **prioritize** | Cockpit: "KI: Priorisierung" | Alle offenen AP + Meilensteine | Eisenhower-Einordnung pro AP mit Begruendung | Ja |
| **plan_day** | Mein Tag: "KI: Tag planen" | Offene AP + Meilensteine + Termine | Top 5-7 Aufgaben fuer heute mit Stundenbudget | Ja |
| **weekly_summary** | Cockpit: "KI: Wochenbericht" | Alle Projekte, AP-Status, Meilensteine | Zusammenfassung, Risiken, Empfehlungen naechste Woche | Ja |

#### Kommunikation

| Aktion | Wo | Input | Output | Anonymisiert |
|--------|-----|-------|--------|:---:|
| **draft_email** | Projektdetail: "E-Mail entwerfen" | Use Case + Empfaenger + Ton + Projektkontext (editierbar) | Betreff + E-Mail-Text (Copy-Paste-fertig, bearbeitbar) | Ja |
| **draft_followup** | Projektdetail: "Nachfass-Text" | Delegiertes AP + Person + Tonalitaet | Follow-up-Text passend zum Delegationslevel | Ja |
| **prepare_meeting** | Projektdetail: "Meeting vorbereiten" | Projektkontext (AP, MS, Risiken) | Agenda + Talking Points + offene Fragen + Risiken | Ja |

#### Datenextraktion

| Aktion | Wo | Input | Output | Anonymisiert |
|--------|-----|-------|--------|:---:|
| **parse_transcript** | Projektdetail: "Transkript auswerten" | Meeting-Transkript (Freitext) | AP, Entscheidungen, Risiken, Follow-ups -- einzeln uebernehmbar | Nein (lokal) |
| **execute_commands** | Projektdetail: NL-Kommandozeile | Freitext oder Spracheingabe | Strukturierte Kommandos: create/update AP, Person, MS, Notiz, Risiko | Ja |

### Moegliche zukuenftige KI-Aktionen

#### Fachlich (BSI-spezifisch)

| Aktion | Beschreibung | Nutzen |
|--------|-------------|--------|
| **suggest_bausteine** | Anhand der Zielobjekte eines Projekts passende BSI-Bausteine vorschlagen | Keine Bausteine vergessen, schnellere Modellierung |
| **draft_document** | Gliederung/Entwurf fuer BSI-Dokumente generieren (Leitlinie, Notfallhandbuch, Richtlinie) | Schneller Start bei Dokumenten statt leere Seite |
| **assess_risk** | Risikobeschreibung rein, W/A/Massnahme raus basierend auf BSI 200-3 Gefaehrdungskatalog | Konsistente Risikobewertung |
| **gap_analysis** | GS-Check-Ergebnisse analysieren und priorisierte Massnahmen vorschlagen | Schnellere Realisierungsplanung |

#### Qualitaet & Reflexion

| Aktion | Beschreibung | Nutzen |
|--------|-------------|--------|
| **review_project** | Pruefen: AP ergebnisorientiert? 8/80 eingehalten? Abhaengigkeiten sinnvoll? Scope-Luecken? | Qualitaetssicherung des Projektplans |
| **lessons_learned** | Soll/Ist-Analyse bei Projektabschluss: Was lief gut, wo Ueberschreitungen, Learnings | Verbesserung fuer naechste Projekte |
| **estimate_effort** | Aufwandsschaetzung fuer neue AP basierend auf Erfahrungswerten aehnlicher AP | Realistischere Planung |

#### Kommunikation (erweitert)

| Aktion | Beschreibung | Nutzen |
|--------|-------------|--------|
| **draft_angebot** | Angebotstext fuer Folgebeauftragung oder Change Request basierend auf Projektdaten | Schnellere Angebotserstellung |
| **prepare_presentation** | Praesentationsstruktur fuer Management-Vorstellung (Ergebnisse, Risiken, naechste Schritte) | Vorbereitung auf Steuerungskreise |
| **summarize_for_stakeholder** | Zusammenfassung angepasst an Zielgruppe (IT-Leitung vs. Geschaeftsfuehrung vs. Fachbereich) | Zielgruppengerechte Kommunikation |

#### Automatisierung

| Aktion | Beschreibung | Nutzen |
|--------|-------------|--------|
| **auto_status_update** | Automatisch generierter Statusbericht der woechentlich per n8n versendet wird | Regelmassige Updates ohne manuellen Aufwand |
| **smart_reminders** | KI analysiert taglich alle Projekte und pusht kritische Erinnerungen | Proaktive Steuerung statt reaktives Feuerlöschen |
| **detect_scope_creep** | Baseline-Vergleich durch KI interpretieren lassen, Scope-Veraenderungen bewerten | Fruehwarnsystem fuer Projektrisiken |

---

## Dateien

| Datei | Beschreibung |
|-------|-------------|
| `leitstand.html` | Die Anwendung. Einzige Datei die benoetigt wird. |
| `n8n-workflow-leitstand.json` | n8n Workflow fuer den KI-Copilot. In n8n importieren. |
| `leitstand-demo.json` | Demo-Daten mit 5 Projekten, 15 AP, 5 Personen, 8 MS, 4 Risiken, 6 Termine. |

---

## KI-Setup

### Voraussetzungen

- n8n Instanz (Self-Hosted oder Cloud)
- Anthropic API Key (Claude Haiku)

### Installation

1. In n8n: Workflow importieren (`n8n-workflow-leitstand.json`)
2. Am Node "Anthropic Claude Haiku": Anthropic API Credential zuweisen
3. Workflow aktivieren
4. In der Leitstand-App: Einstellungen > KI-Copilot > Webhook-URL eintragen

### Workflow-Architektur

```
Webhook (POST /webhook/leitstand-ki)
  |
  +-- IF Test? --> Respond "OK"
  |
  +-- Extract Context --> Comm or Core? (IF)
        |
        +-- Switch Core (6 Outputs)
        |     +-- Prompt: decompose_task    --+
        |     +-- Prompt: suggest_delegation --+
        |     +-- Prompt: prioritize         --+--> AI Agent --> Response Formatter --> Respond
        |     +-- Prompt: weekly_summary     --+
        |     +-- Prompt: parse_transcript   --+
        |     +-- Prompt: execute_commands   --+
        |
        +-- Switch Comm (4 Outputs)
              +-- Prompt: draft_email        --+
              +-- Prompt: draft_followup     --+--> AI Agent --> Response Formatter --> Respond
              +-- Prompt: prepare_meeting    --+
              +-- Prompt: plan_day           --+
```

21 Nodes, 10 dedizierte Prompt-Nodes (einzeln in n8n editierbar).

### Datenschutz / Anonymisierung

Alle Daten werden vor dem Senden an die KI automatisch anonymisiert:

- Projektnamen: "ISMS Stadtwerke Musterstadt" --> "Projekt A"
- Personennamen: "Carolin Mueller" --> "Person CM"
- Beschreibungen, E-Mails, Organisationen: entfernt
- KI-Antworten werden nach Empfang automatisch de-anonymisiert

Kein Kundendaten-Leak an die KI.

---

## Datenmodell

### Store-Entities (in localStorage, Prefix `ls_`)

```
projekte[]        -- id, name, kunde, rolle, status, prioritaet, deadline, vorgehensweise, tags
arbeitspakete[]   -- id, titel, projekt_id, verantwortlicher_id, status, aufwand_h, aufwand_ist_h,
                     deadline, prioritaet, delegationslevel, wiedervorlage, ergebnis, abhaengigkeiten
personen[]        -- id, name, kuerzel, anrede, rolle, organisation, schwerpunkte, email, verfuegbarkeit
meilensteine[]    -- id, projekt_id, titel, datum, status
notizen[]         -- id, projekt_id, typ (notiz/entscheidung/risiko), text
termine[]         -- id, ics_uid, titel, start, ende, projekt_id, quelle, status
risiken[]         -- id, projekt_id, titel, wahrscheinlichkeit, auswirkung, massnahme, verantwortlicher_id
tagesplan[]       -- id, datum, items[{id, ap_id, titel, erledigt, notiz, stunden}]
```

### Berechnete Werte (nicht gespeichert)

```
berechnePrio(ap)           -- Score aus Deadline, Prioritaet, Abhaengigkeiten, Projektrolle
getAmpel(projektId)        -- rot/gelb/gruen basierend auf ueberfaelligen AP und gefaehrdeten MS
getProjektFortschritt(id)  -- Prozent erledigter AP
getAPVollstaendigkeit(ap)  -- Prozent ausgefuellter Pflichtfelder
```

---

## Einstellungen

| Setting | Default | Beschreibung |
|---------|---------|-------------|
| Mein Name | -- | Wird bei "ich muss..." in KI-Kommandos zugeordnet |
| Ich bin (Person) | -- | Verknuepfung mit Personen-Eintrag |
| Start-View | Mein Tag | Welche Ansicht beim Oeffnen |
| Projekte-Badge | Alle | Alle oder nur aktive im Sidebar-Badge |
| Cockpit Projekte | Nur aktive | Was im Cockpit-Projektbereich angezeigt wird |
| Prio-Liste Anzahl | 10 | Wie viele AP in "Was brennt?" |
| MS-Horizont | 28 Tage | Meilenstein-Vorschau im Cockpit |
| Nachfass-Tage | 5 | Ab wann Follow-up fuer delegierte AP angezeigt wird |
| Deadline-Warnung | 3 Tage | Ab wann die Projekt-Ampel gelb wird |
| Autosave | 30 Sek | Intervall fuer FileDB + OPFS Sicherung |
| KI Webhook-URL | n8ntest...leitstand-ki | n8n Webhook-Endpunkt |
| KI API-Key | -- | Optional, Bearer Token |

### BSI-Projektvorlagen (editierbar)

6 Standard-Vorlagen basierend auf BSI 200-1/2/3:

1. **ISMS-Aufbau** (12 AP) -- PDCA-Zyklus, Sicherheitsleitlinie bis Management Review
2. **Standard-Absicherung** (11 AP) -- Zertifizierungsfaehig, MUSS+SOLLTE
3. **Kern-Absicherung** (7 AP) -- Kronjuwelen-Fokus
4. **Risikoanalyse** (6 AP) -- 47 elementare Gefaehrdungen, 4 Behandlungsoptionen
5. **Baustein-Umsetzung** (7 AP) -- Einzelnen BSI-Baustein umsetzen
6. **Audit** (7 AP) -- 4-Phasen Audit-Methodik

Alle anpassbar unter Einstellungen > BSI-Projektvorlagen > "Vorlagen bearbeiten".
Eigene Vorlagen koennen hinzugefuegt werden.

---

## Delegation

5 Level nach Management 3.0 (Jurgen Appelo):

| Level | Kurzform | Bedeutung |
|-------|----------|-----------|
| Anweisen | "Mach genau das" | Klare Vorgabe, kein Spielraum. Neue Kollegen, kritische Aufgaben. |
| Beraten | "Mach das, weil..." | Vorgabe mit Erklaerung. Person kennt sich teilweise aus. |
| Befragen | "Was schlaegst du vor?" | Input einholen, selbst entscheiden. Erfahrene Kollegen. |
| Einigen | "Wir entscheiden zusammen" | Konsens-Entscheidung. Gleichrangige, strategische Themen. |
| Delegieren | "Du entscheidest" | Volle Autonomie. Experten, unkritische Aufgaben. |

---

## Priorisierung

Der `berechnePrio(ap)` Score kombiniert:

1. **Ueberfaelligkeit**: +1000 + 10 pro ueberfaelligem Tag
2. **Prioritaet**: kritisch=500, hoch=300, mittel=100, niedrig=0
3. **Deadline-Naehe**: Bis 7 Tage: (7-Tage)*30
4. **Kritischer Pfad**: +80 pro abhaengigem AP
5. **Projektrolle**: +50 wenn Projektleiter

---

## Persistenz

3-Schicht-Speicherung:

1. **localStorage** -- Automatisch bei jeder Aenderung (500ms debounced). Prefix `ls_`.
2. **File System Access API** (FileDB) -- Wenn eine Datei geoeffnet wurde: alle 30 Sek silent save. Oder manuell ueber Einstellungen.
3. **Origin Private File System** (OPFS) -- Stille Browser-Sicherung. Recovery-Banner wenn localStorage leer aber OPFS-Backup vorhanden.

---

## Tastenkuerzel

| Kuerzel | Aktion |
|---------|--------|
| `Esc` | Modal / Einstellungen / KI-Panel schliessen |
| `Ctrl+Z` | Letzte Loeschung rueckgaengig (1 Schritt) |
| `Ctrl+S` | Manuell speichern |
| `Ctrl+Enter` | NL-Kommando senden (im Textfeld) |

---

## Technologie

- **Architektur**: Single-File HTML (~3800 Zeilen), kein Build, kein Framework
- **Design**: HiCheck Design System (CI-Variablen), DM Sans + IBM Plex Mono
- **KI**: n8n Webhook + Anthropic Claude Haiku, anonymisierte Requests
- **Spracheingabe**: Web Speech API (Browser-nativ, de-DE)
- **Kalender**: ICS-Parser mit UID-basiertem Diff-Merge
- **Import**: BlueAnt CSV mit Auto-Spaltenerkennung
- **Persistenz**: localStorage + File System Access API + OPFS

---

## Fachliches Wissen in den KI-Prompts

Die System-Prompts enthalten fundiertes Wissen aus:

- **BSI-Standard 200-1**: ISMS, PDCA, Sicherheitsleitlinie, Management-Verantwortung
- **BSI-Standard 200-2**: Basis/Standard/Kern-Absicherung, Strukturanalyse, Schutzbedarfsfeststellung (Maximumprinzip, Kumulationseffekt, Verteilungseffekt), Modellierung, IT-Grundschutz-Check
- **BSI-Standard 200-3**: 47 elementare Gefaehrdungen, 4 Risikobehandlungsoptionen, Restrisiko-Akzeptanz
- **PMBOK / ISO 21500**: WBS, 100%-Regel, 8/80-Regel, Rolling Wave Planning, Critical Path
- **Management 3.0**: 5 Delegationslevel (Appelo), RACI-Matrix
- **Audit-Methodik**: 4 Phasen, Tests of Design/Effectiveness, Finding-Klassifikation

---

## Browser-Kompatibilitaet

| Feature | Chrome/Edge | Firefox | Safari |
|---------|-------------|---------|--------|
| Kernfunktionen | Voll | Voll | Voll |
| File System Access API | Ja | Nein (Fallback: Download) | Nein (Fallback: Download) |
| OPFS Recovery | Ja | Ja | Eingeschraenkt |
| Spracheingabe | Ja | Nein | Nein |
| Drag-and-Drop (Board) | Ja | Ja | Ja |

Empfehlung: **Chrome oder Edge** fuer volles Feature-Set.

---

## Lizenz

Proprietaer. Alle Rechte vorbehalten.
