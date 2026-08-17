
---

## L-2026-08-17z — „Übersprungen" sieht aus wie „grün", solange beide dasselbe melden

**Gemessen, Sprint 12 (ADR-008, SWR-128).** Die Organisation hatte **741 Python-Tests und
null JS-Tests** bei 1.524 Zeilen `app.js` — und SWR-098/099/100 verlangen Nachweise an
JavaScript. Fünf Sprints lang meldete jeder Abschluss „Tests grün", und die Aussage war
wahr: alle Tests, **die es gab**, waren grün.

> **Eine Teststrecke, die es nicht gibt, meldet dasselbe wie eine, die grün ist:
> nichts.**

Das ist die dritte Gestalt derselben Familie in drei Sprints — SWR-122: eine Prüfung ohne
Leser; SWR-125: eine Regel ohne Prüfung; hier: eine **Fläche ohne Prüfung**, an der
niemand das Fehlen bemerkte, weil Fehlen und Erfolg gleich klingen.

**Regel 1 — eine Prüfung hat drei Zustände, nicht zwei.** `ok`, `rot`, `übersprungen`.
Wer nur zwei kennt, verbucht das Nichtlaufen als Erfolg. `js_tests.lauf()` gibt deshalb
alle drei zurück, und der teuerste Test der ganzen Strecke ist der, der verlangt, dass
`übersprungen` **nicht** `ok` ist.

**Regel 2 — die Meldung nennt, was der Leser dagegen tun kann.** „JS-Teststrecke
übersprungen" allein ist eine Sackgasse. Die Zeile nennt deshalb `node` und den offenen
Decision Request `p12/T-0007`. Eine Meldung ohne Handlungsweg wird nach dreimal Lesen
Tapete.

**Regel 3 — was die Trennung tragen soll, braucht eine Prüfung, nicht nur ein ADR.**
ADR-008 sagt: Entscheidungen nach `regeln.js`, Darstellung in `app.js`. Sprint 11 hat
**dreimal** gemessen, was eine aufgeschriebene Regel ohne Prüfung wert ist. Also steht die
Prüfung im selben Lauf: `regeln.js` enthält im **Code** kein `document.`/`fetch(`, und
`app.js` liest die vier Regeln aus `Regeln` statt sie zu wiederholen.

⚠ **Nebenbefund am eigenen Test:** der erste Entwurf dieser Prüfung wurde rot an einer
**Kommentarzeile** — `regeln.js` erklärt in seinem Kopf, warum es `document.createElement`
nicht anfasst. Eine Prüfung, die die Begründung bestraft, erzieht zum Weglassen der
Begründung. Kommentare werden deshalb vor der Prüfung entfernt — mit einer Gegenprobe, dass
diese Nachsicht keinen echten Zugriff durchlässt.
