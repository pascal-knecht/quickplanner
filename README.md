# Quickplanner

Ressourcenplaner als Single-File-App — kein Build-Schritt, keine Dependencies.

## Starten

`index.html` direkt im Browser öffnen (Chrome oder Edge empfohlen).

## Features

- **Wochenansicht** mit stündlicher Zeitachse (07:00–19:00)
- **Monatsansicht** die das Browserfenster vollständig ausfüllt
- **Drag & Drop** — Events erstellen, verschieben und in der Breite anpassen
- **Doppelklick** auf Event öffnet Bearbeitungsmodal
- **Löschen** per Klick + `Delete`-Taste
- **Detail-Leiste** zeigt Eventdaten bei Auswahl
- **localStorage** — Änderungen bleiben nach Reload erhalten
- **Export** als `planner-events.json`

## Architektur

Alles in einer einzigen `index.html` — kein Framework, kein Server, keine Dependencies.
