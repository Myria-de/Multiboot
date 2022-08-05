# Multiboot mit ISO-Dateien

ISO-Dateien lassen sich in das Grub-Menü des installierten Systems einbauen. Das ist nützlich, wenn Sie beispielsweise ein Livesystem regelmäßig als sicheres Surfsystem verwenden oder Rescuezilla für Backups starten.

**Schritt 1:** Installieren Sie unter Ubuntu 20.04, 22.04 oder Linux Mint 20 das Paket „grml-rescueboot“, im Terminal mit
```
sudo apt install grml-rescueboot
```
**Schritt 2:** Standardmäßig soll der Ordner „/boot/grml“ die ISO-Dateien aufnehmen, den Sie mit 
```
sudo mkdir /boot/grml
```
erstellen. Wenn das Verzeichnis „/boot“ auf einer eigenen Partition liegen, die nicht genügend Platz bietet, können Sie den Pfad in der Datei „/etc/default/grml-rescueboot“ ändern. Beispielsweise mit der Zeile
```
ISO_LOCATION="/grml"
```
Erstellen Sie das angegebene Verzeichnis.

**Schritt 3:** Passen Sie die Datei „/etc/default/grub“ an. Hier müssen die Zeilen
```
GRUB_TIMEOUT_STYLE=menu
GRUB_TIMEOUT=10
```
enthalten sein, damit das Grub-Menü angezeigt wird. Beim Wert hinter „GRUB_TIMEOUT=“ geben Sie die Anzahl der Sekunden an, nach denen der Standardeintrag automatisch gestartet wird.

**Schritt 4:** Kopieren Sie die gewünschten ISO-Dateien in den konfigurierten Ordnern und führen Sie danach
```
sudo update-grub
```
aus.
Wenn Sie den Rechner neu starten, sehen Sie zusätzliche Einträge im Grub-Menü, über die sich die ISO-Dateien booten lassen.

**Zusätzliche Anpassungen für grml-rescueboot:** Das Grub-Menü wird mit update-grub über Scripte im Ordner „/etc/grub.d/“ erzeugt. Die Datei „42_grml“ sucht nach Dateien im ISO-Ordner und erstellt die dafür passenden Einträge. Das Script setzt darauf, dass im ISO die Datei „/boot/grub/loopback.cfg“ enthalten ist, die Optionen für das direkte Booten der ISO-Datei enthält. Bei gängigen Distributionen ist die Datei vorhanden, bei einigen anderen aber nicht. Das bekannte System Rescue (www.system-rescue.org) beispielsweise und auch Gparted Live (https://gparted.org) lassen sich daher nicht booten. 

Das Script „42_grml“ lässt sich aber dafür anpassen. Die relevanten Zeilen für System Rescue sehen Sie in der Abbildung. Dazu gehört noch „/grml/sysresc_loop.cfg“, das als Ersatz für „/boot/grub/loopback.cfg“ (im ISO) dient. Die Beispieldateien können Sie hier herunterladen. In unseren Konfigurationsdateien finden Sie Optionen für Gparted Live und das LinuxWelt-Rettungssystem sowie Anpassungen für Ubuntu 22.04, damit das System gleich mit deutschsprachiger Oberfläche startet.

Erstellen Sie ein Backup der originalen Datei „/etc/grub.d/42_grml“. Ersetzen Sie das Original mit der Datei „grub.d/42_grml“ aus dem Download.

Die neue Datei enthält ab Zeile 113 bedingte Verzweigungen für Gparted Live 1.4.0-1, Ubuntu 22.04, System Rescue-9.02 und das LinuxWelt-Rettungssystem 8.43. Die angegebenen/gewünschten ISO-Dateien und die Konfigurationsdateien müssen im Ordner „/grml“ liegen beziehungsweise in dem Ordener, den Sie in der Datei „/etc/default/grml-rescueboot“ angegeben haben. Passen Sie die Bezeichnungen der ISO-Dateien und Pfade bei Bedarf an.

Den inhalt des Orders „grml“ aus dem Download kopieren Sie nach „/grml“ auf der Festplatte. Die Bezeichnungen der cfg-Dateien für das jeweilige System sind in der Datei „/etc/grub.d/42_grml“ festgelegt. Der Inhalt gilt für das jeweils zugehötige System. Für andere Live-Systeme müssen Sie die Parameter anpassen.

![](https://github.com/Myria-de/Multiboot/raw/main/GRML.png?raw=true)
