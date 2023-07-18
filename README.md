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
- [ ] Ein kalibriertes Gewicht von idealerweise 10kg oder eine Waage und z.B. Wasserflaschen als Ersatz für ein kalibriertes Gewicht

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
Die Monitorausgabe:
```
beelogger Power-On-Off-Test 01.04.2023
 beelogger-Universal mit Arduino Nano  
... teste: Uhr(DS3231)
           ADC-Kalibrierwerte
           und Power - ON / OFF 
 
Starte Uhrbaustein:
beelogger-System Datum und Uhrzeit: 18.7.2023 15:23:04 
Temperatur ueber Sensor in RTC: 26.75

 Batterie:        ADC-Bitwert= 2105		7.81 V
 Solarspannung:   ADC-Bitwert= 256		0.95 V

Teste  Power ON: 
Weckintervall: 1 Minute(n)
Wakeup at: 15:24:04
Sleep-Modus mit Power-ON gestartet, bitte warten ... 
```
nach 1 Minute
```
 i.O.
beelogger-System Datum und Uhrzeit: 18.7.2023 15:24:04 

 Batterie:        ADC-Bitwert= 2102		7.80 V
 Solarspannung:   ADC-Bitwert= 256		0.95 V

Teste  Power OFF: 
Weckintervall: 2 Minute(n)
Wakeup at: 15:26:04
Sleep-Modus mit Power-OFF ist aktiviert, bitte warten ... 
```
nach nochmal 2 Minuten
```
 i.O.
beelogger-System Datum und Uhrzeit: 18.7.2023 15:26:04 

 Batterie:        ADC-Bitwert= 2111		7.83 V
 Solarspannung:   ADC-Bitwert= 247		0.92 V

Teste  Power OFF: 
Weckintervall: 4 Minute(n)
Wakeup at: 15:30:04
Sleep-Modus mit Power-OFF ist aktiviert, bitte warten ... 
```
nach nochmal 4 Minuten
```
>>>>> Power - ON / OFF Test erfolgreich!  <<<<< 
................. Test beendet!
```

### Stockwaage – Kalibrierung & Test {beelogger_Kalibrierung_Waage_XXXXXX.ino}
Die Beschreibung zum Kalibrieren der Waage ist auf [Stockwaage – Kalibrierung & Test](https://beelogger.de/sensoren/waegzellen_hx711/stockwaage-kalibrierung-test) beschrieben. Der Sketch für die Kalibrierung ist schon im ZIP Paket oben mit enthalten und heißt: ```beelogger_Kalibrierung_Waage_XXXXXX.ino```

Vor dem Start des Scripts ein Gewicht zum Kalibrieren bereit halten. Entweder ein genau bekanntes Gewicht oder ein vorher mit einer genauen Waage bestimmtest Gewicht von z.b. Wasserflaschen. Ideal wäre für eine echte Waage ca. 10kg. Hier im Test wir nur eine "Demo-Wägezelle" bis 2kg verwendet und das Gewicht ist eine Wasserflasche die vorher auf einer Haushaltswaage gewogen wurde und 1079g hat.

Gewicht in Gramm im Sketch ```beelogger_Kalibrierung_Waage_XXXXXX.ino``` eintragen ![grafik](https://github.com/bees4gymeck/beelogger/assets/137496089/3ba4901f-64ee-4f06-af43-bdd4f91cd2e2)

Ausgabe des Monitors:
```
Waage Kalibrierung 01.04.2023
 beelogger-Universal mit Arduino Nano
Zur Kalibrierung der Stockwaagen bitte den Anweisungen folgen!
Fehlerhafte und nicht angeschlossene Waagen werden auch angezeigt!
Eine Waage, die nicht kalibriert werden soll, kann ausgelassen werden.
... konfiguriert:  1  Waage(n)!
 
  Alle Waagen ohne Gewicht!
 
  HX711 #1 Kanal A!
HX711 o.k. 
Waage Nummer: 1
 Kalibrierung der Null-Lage ohne Gewicht mit '1' und 'Enter' starten!
 Eingabe von 'x': Waage wird nicht kalibriert.
```
Nach Eingabe der "1" in "Message"

```
Null-Lage ...   ...    ...  
 Tara:  506502
 
Waage Nummer: 1
mit genau  1.079  Kilogramm beschweren - Kalibrierung mit '2' und 'Enter' starten!
```

Wasserflasche mit 1079g genau in der Mitte der Waage platziert und Eingabe von "2" und Enter
```
Kalibriere Waage: 1  ...    ...  
Taragewicht 1: 506502
Skalierung  1: 1070750.75
 
Pruefe Gewicht: 1.079
 
 
Kalibriervorgang abgeschlossen. 
 
 Die Zeile für die Konfiguration:
 
const long Taragewicht[4] = { 506502 , 10 , 10 , 10}; // Hier ist der Wert aus der Kalibrierung einzutragen
const float Skalierung[4] = { 1070750.75 , 1.00 , 1.00 , 1.00}; // Hier ist der Wert aus der Kalibrierung einzutragen
 Weiter mit w
```
Nach Eingabe von "w" kann man noch mit anderen Gewichten testen.
```
x = Kalibrierung wiederholen. 
 
 - wiegen mit Waage 1  
  Waage 1:        Gewicht: 1.079 kg   Skalierung: 1070750.75   Taragewicht: 506502

 - wiegen mit Waage 1  
  Waage 1:        Gewicht: 1.079 kg   Skalierung: 1070750.75   Taragewicht: 506502

 - wiegen mit Waage 1  
  Waage 1:        Gewicht: 0.191 kg   Skalierung: 1070750.75   Taragewicht: 506502

...
```
Die Zeilen für die Konfiguration benötigen wir später beim Hauptprogramm.

### WLAN – ESP8266 Konfiguration
Ob die auf [WLAN – ESP8266 Konfiguration](https://beelogger.de/netzwerk/wlan_esp8266/wlan_esp8266-config) beschriebene Konfiguration wirklich notwendig ist, ist noch nicht zu 100% bekannt. Es ist relativ aufwendig.



### Checkliste
Sammlung aller Parameterwerte die bei den Tests ermittelt werden.

 |Variable|Wertebereich|Wert|Anmerkungen|
 |---|---|---|---|
 |Kalib_Spannung|7.84V -> 7840mV|7840|ADC: Gemessen wird die Spannung des Akkus an den Anschlussklemmen der beelogger Platine und im Sketch in Millivolt eingetragen werden.|
 |Kalib_Bitwert||2113|Wird ermittelt mit ```beelogger_Kalibrierung_ADC_XXXXXX.ino```|
 |Kalibriertes Gewicht|1,079kg|1079|Notwendig für Kalibrierung der Waage|
 |Taragewicht||506502|Ergebnis der Waagen Kalibrierung mit ```beelogger_Kalibrierung_Waage_XXXXXX.ino```|
 |Skalierung||1070750.75|Ergebnis der Waagen Kalibrierung mit ```beelogger_Kalibrierung_Waage_XXXXXX.ino```|
 


 
 
