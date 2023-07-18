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
- [ ] Voltmeter für die Messung der Akkuspannung liegt bereit

### System Check {System_Check_XXXXXX.ino}- Prüft auf vorhandene Komponenten (Uhr, Sensoren, WLAN...)
Die von den Beelogger Sketchen verwendeten [Bibliotheken](https://beelogger.de/solar_und_universal/alle_programmcodes/bibliotheken/) müssen in genau der zur Verfügung gestellten Version verwendet werden. Sie müssen im "libraries" Verzeichnis deiner Sketche entpackt abgelegt werden.
Die "System-Setup/-Check Sketche" sind auf der [beelogger... Test & Kalibrierung](https://beelogger.de/solar_und_universal/alle_programmcodes/kalibrierung_test/) bereitgestellt.

![Screenshot der Verzeichnisse mit den Test-Sketchen](https://github.com/bees4gymeck/beelogger/assets/137496089/c54bddf9-f9d7-4b71-be54-4644164f3a0e)

Ein erster Check der Komponenten wird mit ```System_Check_230607.ino``` durchgeführt.
Die Ausgabe des Serial Monitors könnte wie folgt aussehen:
```
DS18B20 ?
1  :  [C]: 25.81

I2C-Scanner:
scanning of I2C bus from 0x8 to 0x77...
 found at addr: 0x57
 found at addr: 0x68
I2C scan done
 
EE-Prom
 is present at: 0x57
1, 2, 3, 4, 
    EE-Prom: 4 kByte
 
DS3231
 reset alarms
 temperature from DS3231: 26.7°C
 time in DS3231: 18.7.2023 14:06:00 
 
Testing Com.-Modul
AT
OK
 found
AT+GMR
AT version:1.1.0.0(May 11 2016 18:09:56)
SDK version:1.5.4(baaeaebb)
compile time:May 20 2016 15:08:19
OK
 
Low-Power activ.
Sleep forever!
```

### ADC-Kalibrierung-Programmcode für beelogger - {beelogger_Kalibrierung_ADC_XXXXXX.ino}
Die Spannungsüberwachung des Akkus muss mit ```beelogger_Kalibrierung_ADC_230607.ino``` kalibriert werden. Hierzu muss die Akkuspannung am Beelogger mit einem Voltmeter gemessen werden. Wenn du *keinen Akku* hast bitte trotzdem die Spannung messen damit die Test-Sketche weiter unten funktionieren.

Siehe auch unten in der Checklisten Tabelle.

![grafik](https://github.com/bees4gymeck/beelogger/assets/137496089/f4a29980-83c6-4dc1-8d82-4a733088f9a9)

Ausgabe im Monitor
```
beelogger Kalibrierung ADC 07.06.2023
 beelogger-Universal mit Arduino Nano 
Kalib_Spannung: 7840

Starte Uhrbaustein:
Datum und Uhrzeit aktuell im Uhrbaustein: 
18.7.2023 15:9:14 
Uhrbaustein initialisiert.

die mit dem Multimeter gemessene Akkuspannung: 7.84 V
der gemessene digitale 'Bitwert': 2113
 
Die mit dieser Kalibrierung ermittelte Akkuspannung betraegt:7.84 V
 
 Die Zeilen für die Konfiguration:
 
const long Kalib_Spannung =  7840;    // Hier ist der Wert aus der Kalibrierung einzutragen
const long Kalib_Bitwert  =  2113;    // Hier ist der Wert aus der Kalibrierung einzutragen
```
Die Zeilen für die Konfiguration sind gleich für den Power ON/OFF Test im Script ```beelogger_Test_Pwr_OnOff_XXXXXX.ino``` notwendig.

### Power – ON/OFF -Test-Programmcode für beelogger-Universal und beelogger-SMD {beelogger_Test_Pwr_OnOff_XXXXXX.ino}
Hier wird die Funktion der Uhr und deren eingebautem Temperatursensor getestet. Der Temperatursensor aus der Uhr wird übrigens für die Temparaturkalibrierung der Waage verwendet. 
Wir kopieren die Zeilen von oben  hier rein: beelogger_Test_Pwr_OnOff_230401.ino.



### Checkliste
 |Variable|Wertebereich|Wert|Anmerkungen|
 |---|---|---|---|
 |Kalib_Spannung|7.84V -> 7840mV|7840|ADC: Gemessen wird die Spannung des Akkus an den Anschlussklemmen der beelogger Platine und im Sketch in Millivolt eingetragen werden.|
 |Kalib_Bitwert||2113|Wird ermittelt mit ```beelogger_Kalibrierung_ADC_XXXXXX.ino```|
 |||||
 
