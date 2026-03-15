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
