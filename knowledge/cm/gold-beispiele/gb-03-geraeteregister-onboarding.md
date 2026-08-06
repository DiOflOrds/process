# Gold-Beispiel CM: Geräte-Onboarding (SUP.8, Playbook Kap. 13)

**Zweck:** Referenz für Geräteregister-Einträge und den Onboarding-Ablauf.

## Input

User meldet: „Mein Windows-PC soll Ticks ausführen (Git + Python vorhanden, als DiOflOrds angemeldet)."

## Referenz-Output

1. Onboarding-Ticket mit Mensch-Gate (guardrails: device_onboarding: human_gate) → Freigabe ins Decision Log (hier: D007).
2. Registereintrag (`process/cm/geraeteregister.md`):

```
| Feld | Wert |
|---|---|
| Gerät | PC-User-01 (Windows 11) |
| Identität | GitHub-Login DiOflOrds via Credential Manager (kein Token im Repo!) |
| Fähigkeiten | git, python3, node; kein Docker |
| Erlaubte Rollen | alle Skript-Routen; KI-Rollen gemäß Registry |
| Rechteumfang | push auf feature/*; main nur Skript-Route-Ausnahmen |
| Verfügbarkeit | manuell gestartete Ticks (kein Scheduler) |
| Status | aktiv seit 2026-08-06 (D007) |
```

3. Secrets-Handling: ANTHROPIC_API_KEY nur als Umgebungsvariable auf dem Node bzw. GitHub-Actions-Secret — niemals in Dateien im Repo.

## Warum Gold

Mensch-Gate vor Eintrag; Token-Referenz statt Token; Rechteumfang begrenzt; Secrets-Regel explizit.
