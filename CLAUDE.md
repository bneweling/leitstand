# CLAUDE.md -- Projektkontext Leitstand

## Projektbeschreibung

Single-File HTML-Anwendung (PM-Cockpit) nach dem HiCheck-Designsystem.
Hauptdatei: `leitstand.html` -- eine ~3800-Zeilen standalone SPA ohne Build-Step.
Laeuft direkt per Doppelklick/Bookmark auf Firmenlaptops.

## Architektur

### Namespaces (Objekt-Literale)

| Objekt | Zweck |
|--------|-------|
| `U` | Utilities: `esc()`, `escAttr()`, `save()`, `load()`, `debounce()`, `checkStorageQuota()`, `obfuscate()`/`deobfuscate()` |
| `Store` | Datenhaltung + Persistenz (localStorage). Entities: projekte, arbeitspakete, personen, meilensteine, notizen, termine, risiken, tagesplan |
| `AS` | Application State (transient): currentView, modalType, tagDatum, tableSort, tableSearch, apFilter |
| `Toast` | Notifications: `Toast.show(msg, type)` |
| `Validate` | Input-Validierung |
| `Undo` | 1-Schritt-Undo fuer Loeschungen |
| `AuditLog` | Logging kritischer Aktionen |
| `RateLimit` | Client-seitiges Rate-Limiting fuer KI |
| `FileDB` | File System Access API + Download-Fallback |
| `OPFS` | Origin Private File System -- stille Browser-Sicherung |
| `Anonymize` | Auto-Anonymisierung fuer KI-Requests |
| `App` | Lifecycle: getSaveData(), loadData() |

### Store-Prefix

Alle localStorage-Keys haben Prefix `ls_`.

### Kritische Regeln

1. **KEIN `'use strict'`** -- Funktionsdeklarationen stehen innerhalb von Bloecken, strict-mode wuerde sie block-scoped machen
2. **CSS: Nur kanonische Variablen** -- `--ci-color-brand-blue`, nicht `--ci-brand-blue`
3. **Single-File-Architektur ist gewollt** -- keine Aufteilung in mehrere Dateien
4. **Anonymisierung** -- Alle Daten die an die KI gehen muessen anonymisiert sein (Anonymize-Namespace)

### KI-Integration

- **Webhook**: `/webhook/leitstand-ki` auf n8ntest.germanywestcentral.cloudapp.azure.com
- **n8n Workflow**: `TEST_HTML_Leitstand` (ID: `Uvq3qiLwWenJBbr8`)
- **10 Actions**: decompose_task, suggest_delegation, prioritize, weekly_summary, parse_transcript, execute_commands, draft_email, draft_followup, prepare_meeting, plan_day
- **Routing**: 2 Switch-Nodes (Core: 6, Comm: 4) wegen n8n 9-Output-Limit

### Git-Workflow

- Immer auf `main` pushen
- Kein Commit ohne explizite Aufforderung
