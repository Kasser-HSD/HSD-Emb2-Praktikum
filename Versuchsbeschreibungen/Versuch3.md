<img src="https://github.com/user-attachments/assets/ee957726-a971-4228-80aa-eb613d5ee2a3#gh-dark-mode-only" width="512">
<img src="https://github.com/user-attachments/assets/e1048d2f-873f-4170-ba79-cb7fb7549ccf#gh-light-mode-only" width="512">

# Embedded Systems II – Praktikum </br></br> Versuch 3 


## 1. Einleitung

Im ersten Versuch haben Sie alle benötigen Sensoren und Aktoren in Betrieb genommen. Sie
sollten nun sinnvolle Daten erhalten und daraus Aktionen generieren können.

## 2. Veruschsdurchführung

Ziel der Versuchsreihe ist weiterhin, die Steuerung eines Thermostatventils zu erstellen. Aufgabe in
diesem Versuch ist es, die verschiedenen Teile Ihres Programms in sinnvolle Tasks aufzuteilen.

Zunächst installieren Sie die Bibliothek TaskSched:

![Versuch 3_arduino](https://github.com/user-attachments/assets/c6d66640-dd25-4ab4-8a32-402f07856866)


Überlegen Sie, welche Teile Ihres Programms unabhängig voneinander laufen können. Z.B.
Temperaturmessung, Stellmotor, Display, Eingabe.

Die Eingabe brauchen Sie erst aktivieren, wenn der Taster betätigt wurde und können diese
deaktivieren, sobald die Solltemperatur mit dem Taster bestätigt wurde oder ein Timeout von 20
Sekunden eingetreten ist.

Der Stellmotor könnte alle drei Sekunden bedient werden. Ist die Solltemperatur erreicht, soll der
Stellmotor auf 50% eingestellt sein, bei -5°K Abweichung auf 100% und 0% bei +5°K. Werte zwischen -5°K und +5°K
sollen entsprechend sinnvoll zwischen 100% und 0% liegen.
