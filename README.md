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
Wir gehen davon aus, dass alles zusammengebaut ist und eine erste "elektrische" Inbetriebname, siehe [beelogger Anleitung](https://beelogger.de/solar_und_universal/universal/uebersicht/inbetriebnahme/) erfolgreich abgeschlossen wurde. Die Arduino-IDE ist installiert und der Beelogger wird mit Strom z.B. Akku oder Netzteil versorgt, alle Sensoren inkl Waage sind angeschlossen. WLAN ist vorhanden und Passwort ist bekannt.
- [ ] Stromversorgung via Akku oder Netzteil vorhanden, Alle Module sind aufgesteckt und "Elektrischer" erster Test war ok.
- [ ] Temperatur Sensor DHT11/22 und DS18B20 sind angeschlossen
- [ ] Waage mit Wägezellen sind angeschlossen
- [ ] Arduino-IDE installiert, [Beelogger Libs](https://beelogger.de/solar_und_universal/alle_programmcodes/bibliotheken/) sind konfiguriert

### System Checks
Die von den Beelogger Sketchen verwendeten [Bibliotheken](https://beelogger.de/solar_und_universal/alle_programmcodes/bibliotheken/) müssen in genau der zur Verfügung gestellten Version verwendet werden. Sie müssen im "libraries" Verzeichnis deiner Sketche entpackt abgelegt werden.
Die "System-Setup/-Check Sketche" sind auf der [beelogger... Test & Kalibrierung](https://beelogger.de/solar_und_universal/alle_programmcodes/kalibrierung_test/) bereitgestellt.



### Checkliste
 |Test|Status|Anmerkungen|
 |---|---|---|
 
 |Arduino Libs in genau der bei beelogger angegebenen Version in IDE konfiguriert|||
 |jetzt|aber| gut|
