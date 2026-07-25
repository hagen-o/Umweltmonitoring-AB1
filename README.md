# Umweltmonitoring – Abgabe 1

Dieses Projekt entstand im Rahmen der Lehrveranstaltung **Umweltmonitoring** bei
Hennhöfer. Es untersucht Messdaten des Deutschen Wetterdienstes (DWD) mit zwei
unterschiedlichen Ansätzen:

1. räumliche Interpolation der mittleren Lufttemperatur über Deutschland und
2. Analyse der langfristigen Temperaturentwicklung an der Station Nürnberg.

Die vollständige Verarbeitung, Auswertung und Visualisierung befindet sich im
Jupyter Notebook [`Abgabe1(obha1011).ipynb`](Abgabe1%28obha1011%29.ipynb).

## Inhalt der Analyse

### 1. Räumliche Temperaturinterpolation

Für die DWD-Wetterstationen werden Tageswerte der mittleren Lufttemperatur
(`temperature_air_mean_2m`) geladen. Bei einem mehrtägigen Zeitraum wird je
Station ein Mittelwert gebildet.

Der weitere Ablauf umfasst:

- Zusammenführen der Messwerte mit den Metadaten der Wetterstationen
- Projektion der Stationskoordinaten von WGS 84 (`EPSG:4326`) nach
  ETRS89/LAEA Europe (`EPSG:3035`)
- Erzeugen eines regelmäßigen Rasters über Deutschland
- Ordinary Kriging mit einem sphärischen Variogrammmodell
- Maskierung des Ergebnisses auf die deutsche Landesfläche einschließlich der
  Inseln
- Darstellung der interpolierten Temperaturfläche und der zugrunde liegenden
  Stationsmesswerte

Das Ergebnis ist eine Karte, die räumliche Temperaturunterschiede zwischen den
Regionen Deutschlands sichtbar macht.

### 2. Langfristige Zeitreihenanalyse

Für die DWD-Station **Nürnberg** wird die historische Reihe der mittleren
Lufttemperatur bis Ende 2025 ausgewertet. Die Tageswerte werden zu
Monatsmittelwerten aggregiert und anschließend mit einer robusten
STL-Zerlegung in folgende Komponenten aufgeteilt:

- langfristiger Trend
- jährliche Saisonalität
- Restkomponente

So lassen sich der wiederkehrende Jahresgang und die langfristige
Temperaturentwicklung getrennt betrachten.

## Projektstruktur

| Datei | Beschreibung |
| --- | --- |
| `Abgabe1(obha1011).ipynb` | Jupyter Notebook mit Datenabruf, Verarbeitung, Analysen und Visualisierungen |
| `de.json` | Geodaten der deutschen Landesgrenzen für Rastermaskierung und Kartendarstellung |
| `stations_data_2026-03-17_to_2026-04-17.csv` | Lokal gespeicherte Stationsdaten für den untersuchten Zeitraum |
| `stations_data_2026-04-17.csv` | Lokal gespeicherte Stationsdaten für den einzelnen Stichtag |
| `README.md` | Projektübersicht und Anleitung |

Die CSV-Dateien dienen als lokaler Zwischenspeicher. Dadurch bleibt die
räumliche Analyse ausführbar, ohne bei jedem Notebook-Lauf erneut alle Daten
vom DWD abzurufen.

## Voraussetzungen

Empfohlen werden Python 3.10 oder neuer sowie JupyterLab oder Jupyter Notebook.
Benötigt werden die folgenden Python-Pakete:

```text
pandas
numpy
matplotlib
geopandas
pyproj
pykrige
statsmodels
wetterdienst
jupyter
```

Die Pakete können beispielsweise mit `pip` installiert werden:

```bash
python -m pip install pandas numpy matplotlib geopandas pyproj pykrige statsmodels wetterdienst jupyter
```

## Ausführung

1. Repository klonen oder als Verzeichnis herunterladen.
2. Eine Python-Umgebung erstellen und die oben genannten Abhängigkeiten
   installieren.
3. Im Projektverzeichnis Jupyter starten:

   ```bash
   jupyter lab
   ```

4. `Abgabe1(obha1011).ipynb` öffnen und die Zellen der Reihe nach ausführen.

Für den bereits gespeicherten Zeitraum verwendet das Notebook automatisch die
passende CSV-Datei. Wird im Notebook ein anderer Zeitraum eingestellt und ist
noch keine entsprechende CSV vorhanden, werden die Daten über das Paket
`wetterdienst` vom DWD geladen. Dafür ist eine Internetverbindung erforderlich.
Auch die historische Zeitreihe der Station Nürnberg wird beim Ausführen direkt
abgerufen.

## Verwendete Methoden und Daten

- **Datenquelle:** DWD-Beobachtungsdaten, abgerufen über `wetterdienst`
- **Messgröße:** mittlere Lufttemperatur in 2 m Höhe
- **Räumliche Methode:** Ordinary Kriging
- **Variogrammmodell:** sphärisch
- **Kartenprojektion:** EPSG:3035
- **Zeitreihenmethode:** robuste STL-Zerlegung mit einer Periode von 12 Monaten

## Hinweise zur Reproduzierbarkeit

- Die Parameter `start_date` und `end_date` am Anfang des ersten
  Analyseabschnitts steuern den Zeitraum der räumlichen Auswertung.
- Der Dateiname der CSV wird aus diesem Zeitraum erzeugt. Bereits vorhandene
  Daten werden bevorzugt lokal geladen.
- Verfügbarkeit und nachträgliche Korrekturen der DWD-Daten können Ergebnisse
  eines erneuten Downloads leicht verändern.
- Rasterauflösung, Variogrammmodell und ausgewählte Station sind direkt im
  Notebook definiert und können dort angepasst werden.
