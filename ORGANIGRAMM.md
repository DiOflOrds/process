# Organigramm der Gesamtorganisation

*Generiert durch `platform/scripts/organigramm.py` — nicht von Hand pflegen. Je Einheit liegt ein eigenes `ORGANIGRAMM.md` im Repo.*

```mermaid
graph TB
  MENSCH["Mensch<br/>Auftraggeber / Gates (Klasse A)"]
  PM["PM-Team<br/>Intake · Staffing · PL-Koordination"]
  MENSCH --> PM
  p11["p11<br/>entwicklung · 3 Rollen"]
  PM --> p11
  p12["p12<br/>entwicklung · 2 Rollen"]
  PM --> p12
  p9["p9<br/>entwicklung · 0 Rollen"]
  PM --> p9
  platform["platform<br/>entwicklung · 10 Rollen"]
  PM --> platform
  promt_team["promt-team<br/>wiederkehrend · 1 Rollen"]
  PM --> promt_team
  team_dashboard["team-dashboard<br/>wiederkehrend · 1 Rollen"]
  PM --> team_dashboard
  team_mail["team-mail<br/>wiederkehrend · 1 Rollen"]
  PM --> team_mail
```

| Einheit | Typ | Profil | Status | Datenklasse | Besetzungen |
|---|---|---|---|---|---|
| p0 | projekt | entwicklung | abgeschlossen | intern | — |
| p1 | projekt | entwicklung | ohne Status | intern | — |
| p10 | projekt | entwicklung | abgeschlossen | intern | — |
| p11 | projekt | entwicklung | aktiv | intern | DEV@p11, PL@p11, TEST@p11 |
| p12 | projekt | entwicklung | aktiv | intern | DEV@p12, PL@p12 |
| p2 | projekt | entwicklung | ohne Status | intern | — |
| p3 | projekt | entwicklung | ohne Status | intern | — |
| p4 | projekt | entwicklung | ohne Status | intern | — |
| p5 | projekt | entwicklung | ohne Status | intern | — |
| p7 | projekt | entwicklung | ohne Status | intern | — |
| p8 | projekt | entwicklung | ohne Status | intern | — |
| p9 | projekt | entwicklung | aktiv | intern | — |
| platform | aspice | entwicklung | aktiv | intern | ARCH@aspice, CHG@aspice, CM@aspice, COACH@aspice, DEV@aspice, PL@aspice, PROB@aspice, QM@aspice, RM@aspice, TEST@aspice |
| pm | pm | wiederkehrend | aktiv | intern | COACH@pm, PL@pm, QM@pm |
| promt-team | projekt | wiederkehrend | aktiv | sensibel | PROMPT-OPT@promt-team |
| team-dashboard | projekt | wiederkehrend | aktiv | intern | DASH-RED@team-dashboard |
| team-mail | projekt | wiederkehrend | aktiv | sensibel | MAIL-RED@team-mail |
