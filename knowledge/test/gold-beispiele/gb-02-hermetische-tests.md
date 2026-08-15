# Gold-Beispiel TEST/DEV: Hermetische Tests (Env-Scrub) — aus p2/T-0002

*P2/T-0015, 2026-08-15. Anlass: Die HTTP-Test-Suite verschickte auf dem Team-Node ECHTE Mails, weil dort SMTP-Umgebungsvariablen gesetzt sind — auf Linux-Umgebungen (Sandbox/CI) unsichtbar.*

## Regel

Jeder Test, dessen Codepfad nach außen wirken KANN (Mail, Netz, Subprozesse mit Seiteneffekten), muss die relevante Umgebung in `setUp` entfernen und in `tearDown` wiederherstellen. Ein Test darf niemals davon abhängen, auf welcher Maschine er läuft — und niemals reale Nachrichten erzeugen.

## Muster (aus platform/tests/test_backend.py, HttpTest)

```python
def setUp(self):
    self._env_alt = {k: os.environ.pop(k) for k in
                     ("SMTP_HOST", "SMTP_PORT", "SMTP_USER", "SMTP_PASS", "MAIL_TO")
                     if k in os.environ}
    ...

def tearDown(self):
    ...
    os.environ.update(self._env_alt)
```

## Warum Gold

Doppelfehler in einem: umgebungsabhängiges Rot (blockierte jeden Push, p2/T-0002) UND Seiteneffekt nach außen (Test-Mails beim echten Empfänger). Der Scrub behebt beides mit vier Zeilen. Gegenprobe im Review: „Auf welcher Maschine wird dieser Test rot oder laut?" — die Antwort muss „auf keiner" sein.
