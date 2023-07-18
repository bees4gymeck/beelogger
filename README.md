# Schulbienen beelogger eXplained
Für das Schulbienen-Projekt des [Kreisverband der Imker des Landkreises Erlangen - Höchstadt e.V.](https://imker-erh.de)https://imker-erh.de) wird für das digitale Bienen-Monitoring die freie [beelogger.de](https://beelogger.de) Plattform verwendet.
Ziel dieser Dokumentation ist die Beschreibung der für das Projekt gewählten Beelogger-Variante. Wir verwenden Beelogger in der Version 1.2 mit DS3231 Uhr, WLAN, HX711  und zwei Wägezellen, DHT11/22 für Temparatur/Luftfeuchte außen und DS18B20 für Temparatur im Beuteninnenraum. Stromversorgung ist via Akku-Modul V1.04 und Solarzellen geplant.

Der Aufbau eines Beelogger Gerätes läuft "grob" in folgenden Schritten ab:
1. Bauteile anschaffen
2. Bauteile zusammenbauen (Löten, schrauben, etc.)
3. Arduino und Uhrmodul modifizieren und "elektrische" Inbetriebnahme
4. Programme für Test und Konfiguration der Komponenten ausführen
5. Beelogger Community Server Konto anlegen
6. Hauptprogramm (MultiSketch) anpassen und hochladen
7. Beelogger Server konfigurieren
8. Beelogger an der Beute einbauen und Monitoring prüfen

Diese Dokumentation ersetzt nicht das unbedingt notwendige lesen der originalen Dokumentation auf der Beelogger Website.

## 1-3 Siehe Website
https://beelogger.de/systemuebersicht/systemuebersicht-info
https://beelogger.de/solar_und_universal/universal/uebersicht/bauteile_bezug-1_2-1_3/
https://beelogger.de/solar_und_universal/universal/uebersicht/beschaltung_und_aufbau/
https://beelogger.de/solar_und_universal/universal/uebersicht/inbetriebnahme/

## 4. Programme für Test und Konfiguration der Komponenten ausführen
Wir gehen davon aus, dass alles zusammengebaut ist und eine erste "elektrische" Inbetriebname, siehe [beelogger Anleitung](https://beelogger.de/solar_und_universal/universal/uebersicht/inbetriebnahme/) erfolgreich abgeschlossen wurde.

### 


### Checkliste
 |Test|Status|Anmerkungen|
 |---|---|---|
 |Arduino Libs in genau der bei beelogger angegebenen Version in IDE konfiguriert|||
 |jetzt|aber| gut|
