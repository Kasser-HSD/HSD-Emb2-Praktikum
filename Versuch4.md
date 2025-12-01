<img src="https://github.com/user-attachments/assets/ee957726-a971-4228-80aa-eb613d5ee2a3#gh-dark-mode-only" width="512">
<img src="https://github.com/user-attachments/assets/e1048d2f-873f-4170-ba79-cb7fb7549ccf#gh-light-mode-only" width="512">

# Embedded Systems II – Praktikum </br></br> Versuch 4 


## 1. Einleitung

Die gelernten Inhalte aus den Versuchen 1 bis 3 sollen jetzt auf ein modernes System – hier
ESP32C6 – übertragen werden.

Kernstück ist das Entwicklungsboard XIAO ESP32C6 der Firma Seeed Sudio, welches das SoC
(System on Chip) ESP32C6 der Firma Espressif nutzt. Das SoC, welches um ein RISC-V-System
aufgebaut ist, beherrscht neben den bereits bekannten Schnittstellen zusätzlich im 2,4 GHz-Bereich
die Funkschnittstellen IEEE802.15.4 (Zigbee, Thread), Bluetooth LE und Wi-Fi. Somit sind viele
Funktionen, die ein aktuelles Embedded System braucht, auf einer Fläche von 21mm * 17,8mm
untergebracht.

Auf dem Board selbst befinden sich neben einer USB-C-Schnittstelle und einer Antenne bzw.
einem Anschluss für eine externe Antenne nur zwei LED und zwei Taster.
Weitere Anschlussmöglichkeiten sind über 22 Kontakte verfügbar. Aufgrund der geringen Anzahl
sind diese je nach Anwendung anders zu belegen. Es ist zu beachten, dass sich daher manche
Anwendungen gegenseitig ausschließen.

Um unkompliziert mit dem Board arbeiten zu können, kann das XIAO Expansion Board genutzt
werden. Dies verfügt neben vier weißen Schnittstellen, die mit den bereits bekannten Sensoren und
Aktoren belegt werden können, über ein kleines OLED-Display und einen Taster (Button D1).

## 2. Aufsetzen der Entwicklungsumgebung

Da sich der genutzte Prozessor (RISC-V) erheblich von den bekannten aus den vorangegangenen
Versuchen (ARM Cortex-M0 und ATmega328) unterscheidet, muss zunächst die passende
Toolchain installiert werden.

Dazu laden Sie bitte die neueste stabile Version (zur Zeit 2.3.4) des [Arduino Studio](https://downloads.arduino.cc/arduino-ide/arduino-ide_2.3.4_Linux_64bit.AppImage) herunter.

Die heruntergeladene Datei muss als ausführbar markiert werden:
``` shell
chmod +x ~/Downloads/arduino-ide_2.3.4_Linux_64bit.AppImage
```
Danach kann diese gestartet werden:
``` shell
~/Downloads/arduino-ide_2.3.4_Linux_64bit.AppImage --no-sandbox
```

Gegebenenfalls startet diese erst beim zweiten Aufruf vollständig.

Im Anschluss sollte die passende Boards Manager URL bei File → Preferences ergänzt werden.
Die beiden Kästchen bei „Show verbose output during“ sind sinnvollerweise auch zu setzen.
```
https://raw.githubusercontent.com/Seeed-Studio/Seeed_Platform/master/package_legacy_seeeduino_boards_index.json
https://files.seeedstudio.com/arduino/package_seeeduino_boards_index.json
```

Im Board Manager wird danach esp32 by Espressif Systems ausgewählt.

Das passende Board XIAO_ESP32C6 wird im Drop-Down-Menü Select Board eingestellt.

Die Entwicklungsumgebung ist nun bereit.

Zur Ansteuerung des OLED-Displays und des LED-Streifens fehlen lediglich noch zwei
Bibliotheken, die über den Library Manager geladen werden können.

Adafruit NeoPixel by Adafruit:

U8g2 by oliver:


Damit die Konfiguration des XIAO_ESP32C6 passend ist, müssen noch unter Tools folgende
Einstellungen getroffen werden:

1. Core Debug Level -> Debug
   
2. Erase All Flash before Sketch Upload -> Enabled
   
3. Partition Scheme -> Zigbee 4MB with spiffs
   
4. Zigbee Mode -> Zigbee ED (End Device)
   
Die erste Einstellung hilft, eventuelle Fehler zu finden. Die dritte und vierte werden benötigt, um
die Funktionalität Zigbee Endgerät bereitzustellen. Da hier auch Daten nicht-flüchtig in einem
SPIFFS gespeichert werden, um einen Reboot oder Spannungsverlust zu überdauern, muss
mindestens einmal die zweite Option ausgewählt werden, damit der interne Flash-Speicher darauf
vorbereitet ist.

## 3. Versuchsdurchführung

Ziel des Versuchs ist es, ein dimmbares RGB-Leuchtmittel zu erstellen, welches über Zigbee
angesteuert wird. Zu diesem Zweck ist ein Raspberry Pi 5 System als Zigbee Coordinator im Labor
installiert. Der Versuch ist zum Teil schon so vorbereitet, dass sich Ihr System am Coordinator
anmelden kann und das Licht an- und ausgeschaltet werden kann.
Ihre Aufgabe ist es, den Quellcode zu analysieren und danach

1. den Hersteller und Modellnamen, der über Zigbee an den Coordinator übermittelt wird, auf
Ihren Gruppennamen zu ändern und

2. die Ansteuerung so zu erweitern, dass die Helligkeit und die Farbe über Zigbee eingestellt
werden kann.

Falls ein erneutes Anlernen am Zigbee-Coordinator nötig sein sollte, kann entweder der User-
Button für drei Sekunden gedrückt werden oder es sollte „Erase All Flash before Sketch Upload ->
Enabled“ gesetzt sein.
