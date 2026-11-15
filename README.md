# Reply Logo · three.js Demo

Eine interaktive 3D-Demo des Reply-Logos mit [three.js](https://threejs.org/). Die gesamte Demo steckt in einer einzigen Datei, `index.html`, und braucht keinen Build-Schritt.

## Was die Demo zeigt

- **Logo in 3D:** Die Vektorpfade des Reply-Logos sind in `index.html` eingebettet und werden zur Laufzeit extrudiert. Die Figur erscheint im Reply-Grün `#02d34a`, der Schriftzug in Chrom, damit er auf dem dunklen Hintergrund sichtbar ist.
- **Intro:** 70.000 Partikel (28.000 auf schmalen Bildschirmen) wirbeln als Galaxie um die Szene und setzen sich zum Logo zusammen. Danach blendet das massive Logo mit einem Aufblitzen ein.
- **Szene:** spiegelnder Rasterboden mit Schockwelle, zwei kreisende Lichter, Bloom und Staub im Hintergrund.

## Starten

Voraussetzungen: Python 3 (oder ein beliebiger anderer statischer Webserver), ein aktueller Browser mit WebGL 2 und eine Internetverbindung, weil three.js per CDN geladen wird.

```bash
cd reply-logo-awesome
python3 -m http.server 8000
```

Dann <http://localhost:8000> im Browser öffnen. Der Server wird mit `Strg+C` beendet.

Alternativ ohne Python:

```bash
npx serve .
```

## Bedienung

| Eingabe | Wirkung |
| --- | --- |
| Klick oder Leertaste | Logo zerlegen und neu zusammensetzen |
| Ziehen | Kamera drehen |
| Scrollen | Zoomen |
| Mausbewegung | Logo leicht kippen |

## Technik

- **three.js 0.170** über eine Import-Map von jsDelivr, ohne npm-Abhängigkeiten.
- **Geometrie:** `SVGLoader` liest die Logo-Pfade, `ExtrudeGeometry` macht daraus Körper mit Fase.
- **Partikel:** `MeshSurfaceSampler` verteilt die Zielpunkte auf der Logo-Oberfläche. Die Bewegung von der Galaxie zum Logo rechnet ein eigener Vertex-Shader auf der GPU.
- **Spiegelung:** eine an der Bodenebene gespiegelte Kopie des Logos unter einem halbtransparenten Raster-Shader.
- **Post-Processing:** `EffectComposer` mit `UnrealBloomPass` und `OutputPass`, Tone-Mapping `NeutralToneMapping`, damit das Markengrün erhalten bleibt.

## Anpassen

Die wichtigsten Stellschrauben stehen als Konstanten am Anfang des Modul-Skripts in `index.html`:

| Konstante | Bedeutung |
| --- | --- |
| `PARTICLES` | Anzahl der Partikel |
| `LOGO_WIDTH` | Breite des Logos in Welteinheiten |
| `FLOOR_Y` | Höhe der Bodenebene |
| `DOWN`, `HOLD`, `UP` | Dauer von Zerlegen, Schweben und Zusammensetzen in Sekunden |

Der URL-Parameter `?t=<Sekunden>` verschiebt die Startzeit der Animation, zum Beispiel `?t=20`, um direkt das fertig zusammengesetzte Logo zu sehen.

## Hinweis zur Marke

Das Reply-Logo ist eine Marke der Reply S.p.A. Die Demo ist für interne Zwecke gedacht. Für eine externe Veröffentlichung gelten die Brand-Vorgaben von Reply.
