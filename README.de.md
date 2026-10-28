# TCplup

[English](README.md) | **Deutsch**

Ein Updater für Plugins des [Total Commanders](https://www.ghisler.com). Er liest die Liste deiner Plugins, sucht auf [totalcmd.net](https://totalcmd.net), [ghisler.com](https://www.ghisler.com/plugins.htm) oder GitHub nach neueren Versionen und lädt sie herunter und installiert sie – mit einem Klick für alle.

![TCplup mit der Plugin-Liste einer Total-Commander-Installation und fünf verfügbaren Updates](docs/screenshot.png)

## Download

**[Neueste Version](https://github.com/naderi/tcplup/releases/latest)** – `TCplup.exe` herunterladen und starten. Eine Installation ist nicht nötig.

- Oder mit [Scoop](https://scoop.sh):

  ```powershell
  scoop bucket add naderi https://github.com/naderi/scoop-bucket
  scoop install naderi/tcplup
  ```

**Voraussetzungen:** Windows 10 oder 11 mit .NET Framework 4.5 oder neuer (unter Windows 10/11 vorinstalliert), Total Commander 32 oder 64 Bit (installiert oder portabel).

## So funktioniert es

1. **Plugin-Liste.** TCplup findet die `wincmd.ini` des Total Commanders (Umgebung einer portablen Installation, laufender Total Commander, Registry, übliche Ordner – oder mit *Ändern …* wählen). *Plugin-Liste* bestimmt, was geprüft wird:
   - **Installierte Plugins (wincmd.ini)** – die Plugins, die der Total Commander eingetragen hat (auch wenn der Abschnitt per `RedirectSection` in eine andere Datei ausgelagert ist, wie bei `wincmd_plugins.ini`).
   - **Alle Plugins im Plugin-Ordner** – jedes Plugin im Plugin-Ordner, eingetragen oder nicht.
2. **Installierte Version.** Aus der Dateiversion des Plugins; hat es keine, aus der `pluginst.inf`, dann aus der Readme des Plugins. Nach einem Update durch TCplup merkt sich das Programm die Version, solange die Plugin-Dateien unverändert bleiben.
3. **Neueste Version.** Aus einer Quelle. Der *Update-Modus* bestimmt, wo Plugins ohne eigene Quelle gesucht werden:
   - **Automatisch** – zuerst [totalcmd.net](https://totalcmd.net) (`https://totalcmd.net/plugins/<name>`), dann die Plugin-Tabellen von [ghisler.com](https://www.ghisler.com/plugins.htm).
   - **totalcmd.net** oder **ghisler.com** – nur diese eine.

   Der Name ergibt sich aus Ordner- und Dateinamen des Plugins (auf totalcmd.net mit der Suche der Seite als Rückfall); eine Seite, die offensichtlich zu einem anderen Plugin gehört, wird ignoriert. Eine Quelle lässt sich auch **pro Plugin** festlegen (Rechtsklick → *Quelle festlegen …*): ein **GitHub-Repository** (`owner/repo` oder seine Adresse – genutzt wird das neueste Release), eine totalcmd.net-Seite oder ghisler.com. Eine so festgelegte Quelle gilt in jedem Modus zuerst. Leere Eingabe schaltet wieder auf automatisch.
4. **Update.** Das Archiv wird heruntergeladen, seine SHA-256-Prüfsumme geprüft, wenn die Quelle eine liefert (totalcmd.net, GitHub), und die Dateien werden in den Ordner des Plugins kopiert.

## Bedienung

| Element | Funktion |
|---|---|
| Liste **Plugins** | Nach Typ gruppiert: **Lister**-, **Content**-, **Packer**- und **Dateisystem**-Plugins. Auf eine Gruppenzeile klicken (oder Links/Rechts bzw. Leertaste darauf), um sie zu- oder aufzuklappen; sie zeigt die Anzahl der Plugins und der Updates. **Orange** = Update verfügbar (standardmäßig angehakt), **grün** = aktuell, **grau** = unbekannt oder keine Quelle gefunden. Auf das Kästchen klicken (oder Leertaste), um ein Plugin an- oder abzuwählen. In jeder Gruppe stehen Plugins mit Update oben. Mehrere Plugins wählst du mit Strg und Umschalt. |
| **Legende** (über der Liste) | *Update verfügbar*, *aktuell*, *unbekannt* und *ignoriert* sind Filter: Klick zeigt nur diese Plugins, erneuter Klick wieder alle. *ignoriert* ist deine Ignore-Liste. |
| **Prüfen** | Sucht für alle Plugins erneut nach Updates. |
| **Aktualisieren (n)** | Installiert die angehakten Plugins. Während des Updates wird *Beenden* zu **Abbrechen**. |
| **Plugin-Liste** | Installierte Plugins oder alle Plugins im Plugin-Ordner. |
| **Update-Modus** | Automatisch (totalcmd.net, dann ghisler.com), nur totalcmd.net oder nur ghisler.com. |
| **Beim Start nach Updates suchen** | Sucht beim Start von TCplup nach Updates (installiert aber nie von selbst). |
| **Heruntergeladene ZIP-Dateien behalten** | Behält die ZIP-Dateien im Ordner `downloads` neben TCplup. |
| **ⓘ** (unten links) | Info-Fenster mit Version, Entwickler, Website und Programm-Updates. |

Rechtsklick auf ein Plugin (oder die markierten Plugins): **Aktualisieren** – installiert sofort, auch wenn es aktuell ist –, *Quellseite öffnen*, *Plugin-Ordner öffnen*, *Quelle festlegen …*, *Diese Version überspringen* (nur diese Version wird ausgeblendet) und **Plugin ignorieren (nie aktualisieren)**. Ein ignoriertes Plugin bleibt abgeblendet und mit *ignoriert* in der Liste; es wird nicht geprüft, nicht angehakt und nicht aktualisiert, bis du *Plugin nicht mehr ignorieren* wählst. Das Menü **Extras** enthält *Plugins neu einlesen* (F5), *Total-Commander-Konfiguration wählen …*, *Plugin-Ordner öffnen*, *Alle Updates auswählen* / *Auswahl aufheben* sowie *Alle Gruppen aufklappen* / *Alle Gruppen zuklappen*.

Das Programm erscheint auf Deutsch, wenn Windows auf Deutsch eingestellt ist, sonst auf Englisch. Es folgt dem hellen oder dunklen Windows-Design und der Akzentfarbe.

## Gut zu wissen

- **Der Total Commander kann weiterlaufen.** Ein Plugin in Benutzung lässt sich nicht überschreiben; TCplup benennt die alte Datei um (`*.tcplup-old`) und legt die neue an ihre Stelle, die alten Dateien werden beim nächsten Start gelöscht. Für die neuen Versionen den Total Commander neu starten.
- **Deine Einstellungen bleiben.** Vorhandene `.ini`-Dateien im Plugin-Ordner werden nie überschrieben.
- **Sicher installiert.** Archive mit Pfaden außerhalb des Plugin-Ordners werden abgelehnt, eine fehlgeschlagene Installation wird zurückgenommen, und ein Download mit falscher Prüfsumme wird nicht installiert.
- **Versionsnummern werden verglichen, nicht geraten.** `2.2.3` und `2.23` oder `2.0.0.4` und `2.04` gelten als dieselbe Version. Eine Seite, die älter ist als die installierten Dateien, wird nicht als Update angeboten.
- ZIP-Archive werden direkt installiert. Für **7z**- und **RAR**-Archive (manche Plugins werden so veröffentlicht) braucht TCplup [7-Zip](https://www.7-zip.org) (gefunden im `PATH`, in Programme oder in Scoop); ohne 7-Zip zeigt das Plugin *für dieses Archiv wird 7-Zip benötigt*.
- Nicht jeder Eintrag auf ghisler.com verweist direkt auf ein Archiv. TCplup folgt Links auf eine GitHub-Releases-Seite (dann wird das neueste Release genutzt) und auf eine Download-Seite (dann wird das erste dort verlinkte ZIP, 7z oder RAR genommen). Namen werden ohne angehängtes „plugin“ oder „plug“ verglichen, daher findet `sftpplug` den Eintrag *SFTP* und `cloudplugin` den Eintrag *Cloud*.
- Plugins ohne Quelle zeigen *keine Quelle gefunden*. Plugins, die auf GitHub (oder einer anderen Seite) liegen, lassen sich von Hand mit ihrer Quelle versehen. GitHub erlaubt ohne Anmeldung 60 Anfragen pro Stunde, das reicht für einige Plugins.

## Programm-Updates

TCplup aktualisiert sich selbst aus seinen GitHub-Releases: einmal täglich (im Info-Fenster abschaltbar) und auf Wunsch; ein oranger Punkt am ⓘ-Knopf zeigt, dass ein Update vorliegt. Der Download wird nur installiert, wenn seine Signatur stimmt. Mit Scoop weist TCplup nur auf `scoop update tcplup` hin.

## Lizenz

Freeware – frei nutzbar, aber **nicht zum Verkauf**. Siehe [LICENSE](LICENSE).

## Unterstützung

Wenn TCplup dir Zeit spart, kannst du die Entwicklung auf Ko‑fi unterstützen. ☕

<a href='https://ko-fi.com/N7N123QIX0' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://storage.ko-fi.com/cdn/kofi2.png?v=6' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>

---

© 2026 [Ali Naderi](https://github.com/naderi) · [digitalent.dev](https://digitalent.dev/de/)
