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
![2](https://github.com/user-attachments/assets/7b102b03-f6d6-49f3-97d0-355383d35911)

Weitere Anschlussmöglichkeiten sind über 22 Kontakte verfügbar. Aufgrund der geringen Anzahl
sind diese je nach Anwendung anders zu belegen. Es ist zu beachten, dass sich daher manche
Anwendungen gegenseitig ausschließen.
![3](https://github.com/user-attachments/assets/6ea495c3-8bba-4771-b48b-3062017f6b1f)

Um unkompliziert mit dem Board arbeiten zu können, kann das XIAO Expansion Board genutzt
werden. Dies verfügt neben vier weißen Schnittstellen, die mit den bereits bekannten Sensoren und
Aktoren belegt werden können, über ein kleines OLED-Display und einen Taster (Button D1).
![4](https://github.com/user-attachments/assets/cd840531-705e-4001-a490-90f0af21f938)

## 2. Aufsetzen der Entwicklungsumgebung

Da sich der genutzte Prozessor (RISC-V) erheblich von den bekannten aus den vorangegangenen
Versuchen (ARM Cortex-M0 und ATmega328) unterscheidet, muss zunächst die passende
Toolchain installiert werden.

Dazu laden Sie bitte die neueste stabile Version (zur Zeit 2.3.4) des [Arduino Studio](https://downloads.arduino.cc/arduino-ide/arduino-ide_2.3.4_Linux_64bit.AppImage) herunter.

![Arduino IDE](https://github.com/user-attachments/assets/cd365573-8002-425d-934a-886b53cee427)

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
<img width="1300" height="775" alt="7" src="https://github.com/user-attachments/assets/a0f86d38-1947-424f-9e4b-b53d1a7ad80f" />

<img width="1300" height="775" alt="8" src="https://github.com/user-attachments/assets/dcfdfabe-dadf-487c-bd18-95dee37a854a" />

Im Board Manager wird danach esp32 by Espressif Systems ausgewählt.

<img width="1300" height="775" alt="9" src="https://github.com/user-attachments/assets/6d984d4a-d9f1-4000-a3a2-4a36cd8f4038" />

Das passende Board XIAO_ESP32C6 wird im Drop-Down-Menü Select Board eingestellt.

<img width="1300" height="775" alt="10" src="https://github.com/user-attachments/assets/469d59d9-4050-4217-aab1-41f8a5939b6b" />

Die Entwicklungsumgebung ist nun bereit.

Zur Ansteuerung des OLED-Displays und des LED-Streifens fehlen lediglich noch zwei
Bibliotheken, die über den Library Manager geladen werden können.

Adafruit NeoPixel by Adafruit:

<img width="1300" height="775" alt="1" src="https://github.com/user-attachments/assets/ca1d643b-d9db-4177-8bc6-6fb3a49a43bd" />

U8g2 by oliver:

<img width="1300" height="775" alt="0" src="https://github.com/user-attachments/assets/f9164d34-b7c4-4986-8787-d4f0c6691abc" />

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
