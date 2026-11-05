# Fermentations-Simulator

Interaktiver Simulator für die Fed-Batch-Fermentation von rekombinantem *E. coli* zur Produktion eines Modellproteins (GFP). Läuft als einzelne HTML-Datei im Browser, ohne Installation.

**→ [Simulator öffnen](https://DEIN-NUTZERNAME.github.io/fermentations-simulator/)**

Lernprojekt: Alle kinetischen Parameter sind Literaturschätzungen für *E. coli* auf Glukose, keine an einem Stamm gemessenen Werte.

## Was der Simulator abbildet

**Wachstum und Produktbildung**
- Monod-Kinetik mit Massenbilanzen für Biomasse, Glukose, Acetat, Produkt und Volumen
- Produktbildung nach Luedeking-Piret, eingeschaltet durch IPTG-Induktion

**Prozessführung**
- Batch, Fed-Batch mit konstantem oder exponentiellem Zulauf
- Umschaltung auf eine zweite Wachstumsrate bei der Induktion

**Sauerstoff und Wärme**
- Sauerstoffbedarf (OUR) gegen die maximale Übertragung (OTR = kLa · C\*)
- Abnahme des Sauerstoffgehalts im aufsteigenden Gas
- Wärmebilanz aus Atmung und Rührleistung gegen die Kühlleistung des Mantels

**Overflow-Metabolismus**
- Acetatbildung oberhalb einer kritischen Wachstumsrate und bei Sauerstoffmangel
- Wiederaufnahme von Acetat bei freier Atmungskapazität
- Hemmung von Aufnahme und Wachstum durch Acetat

**Scale-up**
- Maßstäbe vom Laborreaktor bis zum Produktionstank
- Berechnung von kLa (van ’t Riet), Mischzeit und Kühlleistung aus der Geometrie
- Zwei-Zonen-Modell für Konzentrationsgradienten: eine Zulaufzone tauscht Kultur mit dem Rest des Tanks aus

**Parameterschätzung**
- Eigene Messdaten einfügen (aus einer Tabellenkalkulation kopierbar)
- Anpassung der Parameter mit dem Nelder-Mead-Verfahren, Fehlerquadratsumme über alle Messgrößen
- Beispieldaten mit bekannten Parametern, um die Anpassung zu prüfen

**Softsensor**
- Schätzung der Biomasse aus dem simulierten Abgassignal
- Korrektur durch Offline-Proben mit einem Kalman-Filter, inklusive Unsicherheitsband
- Einstellbares Messrauschen, Kalibrierfehler und Probenintervall

## Numerik

Zeitschrittverfahren mit 0,002 h Schrittweite. Die Glukosebilanz wird halb-implizit gelöst, weil das System steif ist: Bei kleinem K_s und hoher Zelldichte reagiert die Substratkonzentration um Größenordnungen schneller als die Biomasse, und ein explizites Verfahren würde oszillieren oder negative Konzentrationen liefern. Im Zwei-Zonen-Modell werden beide Zonenbilanzen gemeinsam gelöst (Regula falsi mit Illinois-Korrektur).

Die Ergebnisse wurden über Massenbilanzen geprüft: Die zugeführte Glukose entspricht der gebildeten Biomasse, dem Acetat und dem Restsubstrat.

## Grenzen des Modells

Nicht abgebildet: pO₂-Regelung, Wachstumsbremse und Plasmidverlust nach der Induktion, Erhaltungsstoffwechsel, CO₂-Bilanz und -Hemmung, Temperatur- und pH-Effekte.

Beim Scale-up sind die Korrelationen für kLa, Mischzeit und Wärmeübergang Abschätzungen für einen idealisierten Rührkessel. Die kLa-Korrelation wird oberhalb weniger Kubikmeter extrapoliert. Das Zwei-Zonen-Modell zeigt Gradienten qualitativ, erklärt die in der Literatur berichteten Ausbeuteverluste im großen Maßstab aber nicht allein; Zellstress durch wechselnde Bedingungen fehlt.

Die Signale des Softsensors sind simuliert, nicht gemessen.

## Technisch

Eine HTML-Datei, kein Framework, keine Abhängigkeiten außer den Schriftarten (ohne Internet greift eine Ersatzschrift). Diagramme über Canvas, Oberfläche auf Deutsch, für Mobilgeräte ausgelegt, mit hellem und dunklem Modus.

Entwickelt mit KI-Unterstützung (Claude). Modellstruktur, Parameterwahl und Plausibilitätsprüfungen stammen von mir.

## Lizenz

MIT
