<img src="https://github.com/user-attachments/assets/ee957726-a971-4228-80aa-eb613d5ee2a3#gh-dark-mode-only" width="512">
<img src="https://github.com/user-attachments/assets/e1048d2f-873f-4170-ba79-cb7fb7549ccf#gh-light-mode-only" width="512">

# Embedded Systems II – Praktikum </br></br> Versuch 2 


## 1. Einleitung

Im ersten Versuch haben Sie ein paar der benötigen Sensoren und Aktoren in Betrieb genommen.
Falls diese noch nicht zuverlässig funktionieren, arbeiten Sie dies nach, weil die Versuche
aufeinander aufbauen.

## 2. Veruschsdurchführung

Versuchen Sie die genannten Aufgaben durch Recherchen im Netz und anhand ähnlicher Beispiele
zu lösen und erstellen Sie nach **jedem** Schritt einen Git-Commit:

### a) Eingebaute LED

Die eingebaute LED lassen Sie mit jedem Durchlauf des loop() blinken. Jeder Durchlauf soll
zunächst nur alle drei Sekunden stattfinden.

### b) Serielle Schnittstelle über USB zum PC

Geben Sie am Anfang oder Ende des `loop()` die Zeit in Millisekunden aus, die seit dem Starten
vergangen ist. Was fällt Ihnen auf?

Nutzen Sie die serielle Schnittstelle ausgiebig, um sich zu vergewissern, ob Ihr Programm so
arbeitet, wie Sie es erwarten.

Erstellen Sie Ihr Programm so, dass Sie über ein `#define DEBUG` alle Ausgaben ein- bzw.
ausschalten können:

``` arduino
#define DEBUG
#ifdef DEBUG
  #define DEBUG_PRINT(x) Serial.print(x)
  #define DEBUG_PRINTLN(x) Serial.println(x)
#else
  #define DEBUG_PRINT(x)
  #define DEBUG_PRINTLN(x)
#endif 
```

### c) Taster

Durch Druck des Tasters sollte der Durchlauf des loop() eine Minute lang alle 0,5 Sekunden
stattfinden. Dummerweise wird der Taster im normalen Betrieb nur alle drei Sekunden abgefragt.
Wie können Sie erreichen, dass ein Druck auf die Taste eine Reaktion auslöst, auch wenn Sie nur
kurz drücken?

### d) Potentiometer / Servo

Übertragen Sie den eingestellte Position des Potentiometers auf den Servo:

Potentiometer links: Servo 5°  
Potentiometer rechts: Servo 175°  

**Achtung!!!** Fahren Sie den Servo nie bis an den Anschlag. Das unangenehme Geräusch ist mit
hohem Materialverschleiß verbunden.

### e) Temperatursensor

Versuchen Sie, aus dem Messwert eine möglichst genaue Temperatur zu berechnen. Beachten Sie
dabei, dass je nach Version der Leiterplatte des Sensors unterschiedliche Bauteile verwendet sein
können.

Hinweis:

``` arduino
const int pinTempSensor = A0;
const unsigned int B = 4275; // B value of the thermistor
int value;
float curr_temp;
value = analogRead(pinTempSensor);
curr_temp = (float) ADCMax / value - 1.0;
curr_temp = 1.0 / (log(curr_temp) / B + 1.0 / 298.15) - 273.15;
```

### f) LCDisplay 1

Geben Sie die gemessene Temperatur inklusive der Zeichen *„°C“* auf dem LCDisplay aus.

### g) LCDisplay 2

Obwohl das LCDisplay mit einfarbigem Hintergrund auch für 3,3V geeignet ist, funktioniert es
zunächst nicht mit dem Arduino Due. Beachten Sie die Änderungen an den Dateien rgb_lcd.*,
welche Sie auf Moodle finden. Was ist geändert und warum?

Abschließend noch ein Hinweis, um mit möglichst wenig Aufwand ein Programm zu schreiben,
was auf beiden Prozessoren läuft:

``` arduino
#define DEBUG
// green board
#if defined(ARDUINO_ARCH_SAM)
  #define MAX_VOLTAGE 3300
  #define WireMaster Wire1
  #define SETRES(x) analogReadResolution(x)
// red board
#elif defined(ARDUINO_ARCH_AVR)
  #define ADC_RESOLUTION 10
  #define MAX_VOLTAGE 5000
  #define WireMaster Wire
  #define SETRES(x) while(false){}
// untested
#else
  #error "Wrong Hardware"
#endif
const unsigned long ADCMax = (1 << ADC_RESOLUTION) - 1;
```

Nutzen Sie diese Defines in Ihrem eigenen Projekt.