---
title: Versuch 1
---

<!-- ![HSD EI](Bilder/HSD_Marke_v4_HSD_RotRGB_EI_Schw_v2.png) -->

# Embedded Systems II – Praktikum - Versuch 1 


## 1. Einleitung

Im Rahmen der Versuchsreihe soll eine digitale Thermostatventilsteuerung erstellt werden. Ziel dabei ist, die Programmierung eines Mikrocontrollers mithilfe einer aktuellen Entwicklungsumgebung zu erlernen und dabei das Versionskontrollsystem Git zu verwenden. Ein *„Learning by doing“* und selbständiges Suchen von verwertbaren Informationen im WWW und über generative KI ist dabei ausdrücklich erwünscht.

## 2. Allgemeines

### 2.1 Nutzung von USB Sticks, Email und Messenger Dienste

Um eine nachvollziehbare und sichere Zusammenarbeit im Embedded Systems II Praktikum zu gewährleisten, darf jeglicher von Ihnen vorbereiteter Code ausschließlich über Ihr Git-Repository eingebracht werden.

Die Nutzung von USB Sticks ist generell verboten.

Kein Austausch von Code über E-Mail, WhatsApp, Telegram oder andere Messenger.

Bei Nichtbeachtung wird der Versuch abgebrochen und führt zur Nichtbewertung des Versuchs.

### 2.2 Betreuung und Abnhame der Versuche

Wenn Sie während der Bearbeitungszeit auf Probleme stoßen, dürfen Sie gerne die Betreuung um Hilfe bitten. Nutzt Sie die Möglichkeit zur Rückfrage, bevor Sie zur Abnahme antreten. Wer sich zur Abnahme meldet, signalisiert, dass der Versuch vollständig bearbeitet, verstanden und getestet wurde. Das bedeutet:

- Die technische Umsetzung (z. B. Schaltung, Code, Funktion) muss funktionsfähig und nachvollziehbar sein.

- Es wird erwartet, dass der Code erklärt und Fragen dazu von beiden Versuchsteilnehmern oder Versuchsteilnehmerinnen sicher beantwortet werden können.

- Der Umgang mit den verwendeten Tools (IDE, GIT, Debugger, etc.) sollte sicher demonstriert werden.

- Die Abnahme ist keine Fehlersuche – dafür ist die Fragephase vorher da.

Sollte sich während der Abnahme herausstellen, dass sie nicht über das geforderte Wissen verfügen und Fragen nicht beantworten können, wird die Abnahme abgebrochen und Sie können die verbleibende Praktikumszeit zur Nacharbeitung nutzen. Wiederholter Abbruch der Abnahme oder bei der Präsentation von lediglich kopierten oder KI-generierten Lösungen ohne Eigenleistung führen zur Nichtbewertung des Versuchs.

### 2.3 Versuchsvorbereitung vor dem Praktikumsversuch

Zusätzlich zu den nachfolgenden Schritten ist eine intensive Vorbereitung mit der Thematik aus der Vorlesung und dem Versuchsstoff notwendig. Bereiten Sie sich bitte gut vor. Darüber hinaus führen Sie bitte ***vor*** dem Praktikumsversuch folgende Schritte durch:

Erstellen Sie sich einen Account auf www.github.com

Erstellen Sie ein Repository mit dem Namen *„ES II Praktikum Gruppe \<Nummer\>“* und setzen die Sichtbarkeit auf **privat**, wobei die Nummer, die ist, unter der Sie sich in Moodle angemeldet haben. *(z.B. B17)*

Falls Ihnen ihr Gruppenpartner oder Ihre Gruppenpartnerin bekannt ist, fügen Sie sich gegenseitig als **Collaborators** zu ihren Repositorys hinzu. Wenn nicht, holen Sie diesen Schritt zu Beginn des Versuches nach. Da während des Versuchs das Versionskontrollsystem Git mit der Kommandozeile bedient werden soll, können Sie Ihre Kenntnisse im Umgang mit der Kommandozeile hier spielerisch auffrischen: [Wargames](https://overthewire.org/wargames/)

## 3 Hardware

Für die Versuche stehen zwei verschiedene Mikrocontroller (ATmega328P auf einem SeeeduinoV4-Board (rot), ARM Cortex M0 auf einem Arduino Due (grün)) zur Verfügung. Zur Erweiterung mit Sensoren und Aktoren können Sie das Grove-Starter Kit nutzen. Achten Sie darauf, dass der Schiebeschalter auf die korrekte Spannung gestellt ist, bevor Sie das Board mit dem PC verbinden.

### <p style="text-align:center"><font color="red">Rotes Board → 5V</font></p>
### <p style="text-align:center"><font color="green">Grünes Board → 3,3V</font></p>

Die Boards werden mit dem MicroUSB-Kabel an den PC angeschlossen und werden darüber auch mit der nötigen Betriebsspannung versorgt.

<!-- Drei Bilder Hier einfügen -->

## 4 Versuchsdurchführung

### 4.1 Installation der Entwichlungsumgebung (IDE)

Im ersten Schritt installieren Sie die IDE, indem Sie zunächst die Installationsdatei Arduino IDE 2 für Linux herunterladen. Die Datei befindet sich danach im Ordner Download und muss entpackt werden. Als Ziel bietet sich `~/arduino-ide` an. Anschließend muss die Datei arduino-ide als ausführbar gekennzeichnet werden. Dies kann in einem Terminal wie folgt geschehen:

Variante A (Appimage):
```batch
cd ~/Downloads
wget https://downloads.arduino.cc/arduino-ide/arduino-ide_2.3.6_Linux_64bit.AppImage
chmod +x arduino-ide_2.3.6_Linux_64bit.AppImage
```
Zukünftig können Sie die IDE mit einem einzigen Befehl starten:
```batch
~/Downloads/arduino-ide_2.3.6_Linux_64bit.AppImage --no-sandbox
```

Variante B (Zip):
```batch
cd ~/Downloads
wget https://downloads.arduino.cc/arduino-ide/arduino-ide_2.3.6_Linux_64bit.zip
unzip arduino-ide_2.3.6_Linux_64bit.zip
mv arduino-ide_2.3.6_Linux_64bit ~/arduino-ide
chmod +x ~/arduino-ide/arduino-ide
```
Zukünftig können Sie die IDE mit einem einzigen Befehl starten:
```batch
~/Downloads/arduino-ide_2.3.6_Linux_64bit.AppImage --no-sandbox
```

### 4.2 Konfiguration der Entwicklungsumgebung (IDE)
Starten Sie die IDE und legen Sie unter File → Preferences → Sketchbook location einen Pfad fest.

Installieren Sie im Boards Manager die beiden Boards Arduino Due (grün) und Seeeduino V4 (rot).
Damit das rote Board genutzt werden kann, muss unter File → Preferences → Additional boardsmanager URLs:
```
https://raw.githubusercontent.com/Seeed-Studio/Seeed_Platform/master/package_legacy_seeeduino_boards_index.json
```
eingetragen sein.

Im Library Manager suchen Sie die Bibliotheken *„Grove – LCD RGB Backlight“* und *„Servo by Michael Margolis, Arduino“*.

Sie sind nun in der Lage, die beiden Boards mit eigener Software zu programmieren.

### 4.3 Vorbereitung Ihres GIT Repository

- Entscheiden Sie, welchen ihrer beiden GIT Accounts Sie am Praktikumsrechner benutzen wollen. Erstellen Sie sich in diesem Account einen Personal Access Token (PAT). „Merken“ (Zwischenablage, Editorfenster) Sie sich den Token, denn wenn Sie das Fenster schließen, können Sie den Token nicht erneut einsehen. Haben Sie den Token vergessen, müssen sich einen neuen erstellen. Es empfiehlt sich dann, den alten Token zu löschen.
  
- Wichtig: Beim Fine Grained Token muss beim Erstellen (geht auch nachträglich) noch die
Permission: „Content“ mit Read / Write hinzugefügt werden, um pushen und pullen zu
dürfen.

  - Wenn Sie Ihre erste Git-Operation durchführen, die eine Authentifizierung erfordert (wie git clone oder git push), werden Sie nach Ihrem Benutzernamen und Passwort gefragt.
  
  - Benutzername: Geben Sie Ihren Benutzernamen beim Git-Hosting-Anbieter ein.
  
  - Passwort: Fügen Sie das Personal Access Token ein, das Sie gerade generiert haben.
  
- Öffnen Sie ein Terminal und führen die nachfolgenden Schritte aus.
  
  - Konfigurieren Sie den Credential Helper so, dass der Token nur für diese Sitzung gespeichert wird. Setzen Sie den Timer hoch, so dass der Token für die Dauer des Versuches nicht aus dem Cache entfernt wird.  
  `git config --global credential.helper 'cache --timeout=7200'`
  
  - Teilen Sie dem Programm git Ihren Nutzernamen und Emailadresse mit.
  
  - Clonen Sie Ihr erstelltes Repository in ihren Benutzerordner auf dem Rechner.

### 4.4 Testen der Sensoren und Aktoren

Entwickeln Sie ein Programm, welches alle Komponenten nutzt. Im ersten Schritt brauchen die Komponenten noch nicht alle miteinander interagieren. Benutzen Sie zunächst das rote Board. Bedienen Sie sich dabei gerne an den Beispielen der IDE und den Beispielen des Starter Kits. Schreiben Sie Ihr Programm so, dass es auf beiden Boards lauffähig ist.

- Erstellen Sie ein Programm, dass die interne LED ihrer Hardware in Betrieb nimmt.

- Comitten Sie Ihre fertige Lösung in ihr Repository. Geben Sie eine sinnvolle Commit Message an.

- Erweitern Sie Ihr Programm so, dass die LED mit einem Taster an und ausgeschaltet wird.

- Comitten Sie Ihre fertige Lösung in ihr Repository. Geben Sie eine sinnvolle Commit Message an.

- Erweitern Sie Ihr Programm so, dass der Zustand der LED auf der seriellen Schnittstelle ausgegeben wird.

- Comitten Sie Ihre fertige Lösung in ihr Repository. Geben Sie eine sinnvolle Commit Message an.