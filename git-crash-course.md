
# Versionskontrolle mit Git

## Warum Versionskontrolle? 
Mit Versionskontrollsystemen (VCS) lassen sich Änderungen an Dateien über die Zeit hinweg protokollieren. Dadurch sind Versionen und Änderungen jedes Zeitpunkts zugreifbar und nachvollziehbar.
Unter Softwareentwicklern ist dies der Standard um den Quellcode und seine durchgehenden Entwicklungsprozess zum einen nachvollziehen zu können und zum anderen als Backup zu sichern.
Auch andere Berufsbereiche wie z.B. im Grafik- und Webdesign können so Versionen der Bilder oder Layouts zurückverfolgen.

## Warum Git?
*Git* ist eine freie Software zur verteilten Versionskontrolle, welche von Linus Torvald initiiert wurde. 
Das Adjektiv *verteilt* steht dafür, dass die Daten aller Versionen nicht nur auf einem zentralen Serve, sondern auch bei jedem Benutzer gespeichert wird. Die folgende Abbildung aus dem Buch Git Pro zeigt schemenhaft die distribuierte Versionskontrolle.

![Distribuierte Versionskontrolle](https://git-scm.com/figures/18333fig0103-tn.png)
Der Name *Git* bedeutet in der britischen Umgangssprache so viel wie *Blödmann* und spielt auf die versehentliche Veränderung oder Löschung von Dateien an.
Am besten funktionert Git mit Dateien, die Text beinhalten, kann aber jegliche Dateien versionieren. Bei Textdateien werden nur die Änderungen zwischen den einzelnen Versionen betrachtet und somit effizient gespeichert. Bei binären Dateien werden diese jedesmal ganz gespeichert wenn eine Änderung vorliegt. 
Daher eignet sich Git bestens Quellcode jeglicher Sprachen.

In Git gibt es drei Hauptzustände für eine Datei: *commited*, *modified* und*staged*.
- Ist die Datei commited, so ist sie und die letze Änderung in der lokalen Datenbank gespeichert.
- Ist die Datei modified, so wurde sie seit der letzten Version geändert und noch nicht in der lokalen Datenbank eingecheckt.
- Ist die Datei staged, so ist sie und ihre Änderung für den nächsten *commit*, also das einchecken in die Datenbank, vorgemerkt.

Diese drei Hauptzustände führen zu den Hauptbereichen eines Git-Projektes:
![Arbeitsverzeichnis, Staging-Area und Git Verzeichnis](https://git-scm.com/book/en/v2/images/areas.png)

- Das *.git directory* bzw. *Repository* beinhaltet alle Metadaten und die Datenbank mit allen Versionen des Projekts.
- Im *Working Directory* ist die aktuell gewählte Version des Projekts, welche aus dem Repository exportiert und auf der Festplatte angezeigt wird.
- Die *Staging Area* ist an sich nur eine Datei, die die Dateien im Zustand staged beinhaltet. Diese Datei wird auch *index* genannt.

---------------------------------

# BRanches erklären
Zweige bzw. *branches* werden verwendet um von der aktuellen Version, dem Hauptstamm, abzuzweigen um Features, Bugfixes oder Rumprobieren in einem eigenen *branch* weiter zu programmieren und schließlich, wenn alles funktioniert, wieder in den *master* zurückzuführen. Dieses zurückführen nennt man auch *mergen*.

## Git CLI
Git an sich ist ein *Command Line Interface*, also ein Kommandozeilenprogramm, welches über [Git-SCM](https://git-scm.com/) installiert werden kann. 
Es gibt auch grafische Oberflächen (GUI Clients) für Git, mit denen über Klicks die gleichen Befehlen wie mit der Konsole durchgeführt werden. Zusätzlich lassen sich alle Versionen und die Zusammenarbeit im Team visuell nachvollziehen.

## CLI Crash Course
Um Git für unser Projekt `Website` zu nutzen müssen wir zunächst das *Repository* initiieren. Dabei erstellt Git den versteckten Ordner `.git` im Projektordner.
Wir erstellen zunächst den Ordner `Website` auf dem Desktop und in diesem eine leere Datei `index.html`. 
(Unter Windows öffnen wir die *Git-Bash*, unter Linux oder Mac OS sollte git nach der Installation im Terminal verfügbar sein.)
```bash
$ cd Desktop
$ mkdir website
$ cd website
$ touch index.html
```
---
### 1. Projekt Initialisierung
Wir initiieren nun das Projekt mit dem Befehl:
```bash
$ git init
``` 
```
Initialized empty Git repository in C:/Users/chris/Desktop/website/.git/
```

Mit `git status` kriegen aktuelle Informationen zu dem Projekt:
```bash
$ git status
``` 
```
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        index.html

nothing added to commit but untracked files present (use "git add" to track)
```
Die erste Zeile `On branch master` sagt uns, dass wir uns im Zweig *master* befinden, welcher meist als Hauptstamm verwendet wird. 

`No commits yet` bedeutet, dass noch keine Version in der Datenbank gespeichert wurde.

Des Weiteren zeigt uns Git an, dass es Dateien gibt, die nicht getrackt werden. Das heißt, Änderungen an diesen Dateien werden nicht gestaged und können somit nicht committed werden.

---
### 2. Dateien tracken und stagen
Um diese Datei nun zu tracken verwenden wir den Befehl `git add index.html` bzw. `git add *` wenn wir alle Dateien im Ordner tracken möchten:
```bash
$ git add *
$ git status
```
```diff
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   index.html

```
Die Datei wurde gestaged mit der Änderung, dass eine neue Datei hinzugefügt wurde. Weitere Änderungen müssen jedes mal mit `git add` gestaged werden.

---
### 3. Dateiänderungen sichern bzw. commiten
Wir wollen nun die neue Datei, ohne Inhalt, als aktuelle Version sichern und verwenden den Befehl `git commit`. Mit jedem commit muss eine Nachricht (message) mitgegeben werden, die die Änderung zur vorrigen Version beschreibt:
```bash
$ git commit -m "initial commit"
```
```diff
[master (root-commit) 4c750d6] initial commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 index.html
```
Meist wird bei der Initialisation eines neuen Repositorys die message "initial commit" gewählt.

Wir möchten nun die index.html Datei mit Inhalt füllen und öffnen diese mit dem Texteditor.
Folgenden Inhalt fügen wir ein:
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Hallo Welt</title>
  </head>
  <body>
    <p>
      Hallo Welt
    </p>
  </body>
</html>
```
Der Befehl `git status` sagt uns nun, dass die Datei modifiziert wurde:
```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   index.html

no changes added to commit (use "git add" and/or "git commit -a")
```
Mit dem Befehl `git diff` können wir uns alle Änderung in der Konsole anschauen:
```bash
$ git diff
```
```diff
diff --git a/index.html b/index.html
index e69de29..edcd10a 100644
--- a/index.html
+++ b/index.html
@@ -0,0 +1,11 @@
+<!DOCTYPE html>
+<html lang="en">
+  <head>
+    <title>Hallo Welt</title>
+  </head>
+  <body>
+    <p>
+      Hallo Welt
+    </p>
+  </body>
+</html>
```
Wir sehen alle Zeilen die hinzugefügt wurden in grün und einem Plus zu Beginn der Zeile. Zeilen die gelöscht werden, werden rot und mit einem Minus gekennzeichnet.

Als nächstes wollen wir auch diese Änderung sichern:
```bash
$ git add *
$ git commit -m "hello world html example added in index file"
```
```
[master 558219a] hello world html example added in index file
 1 file changed, 11 insertions(+)
```
Die Version wurde erfolgreich gespeichert, eine Datei geändert und 11 Zeilen wurden hinzugefügt.

---

### 3. Feature in einem Branch entwickeln und mergen
Möchten wir ein Feature implementieren, ist es üblich aus dem Master Branch zu entzweigen und das Feature in einem seperaten Branch zu entwickeln. Schließlich kann die fertiggestellte erweiterte Funktionalität wieder in den Hauptstamm zurück gemergt werden.
Um einen Branch zu erzeugen verwenden wir den Befehl `git branch <branch name>`:
```bash
$ git branch alert-feature
```
Mit nur `git branch` werden alle Branches aufgelistet:
```bash
$ git branch
```
```
  alert-feature
* master
```
Der Stern markiert, welcher Branch ausgewählt ist. Beim Ausführen von `git branch <branch name>` der aktuell ausgewählte Branch kopiert.  
Mit `git checkout <branch name>` wählen wir nun den anderen Branch aus:
```bash
$ git checkout alert-feature
```
```
Switched to branch 'alert-feature'
```
Nun wurde das Working Directory auf den Stand des Branches alert-feature gesetzt und alle Änderungen und Commits werden in diesem Branch gesichert. 
Wir öffnen wieder die Datei index.html und fügen folgendes unter dem `p`-Tag hinzu:
```
<script>
  alert('Hallo Welt')
</script>
```
Wir erhalten:
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Hallo Welt</title>
  </head>
  <body>
    <p>
      Hallo Welt
    </p>
    <script>
      alert('Hallo Welt')
    </script>
  </body>
</html>
```
Wir stagen und commiten die Änderung  um den Branch anschließend zu mergen:
```bash
$ git add *
$ git commit -m "Greeting, when page is openend, added"
```
```
[alert-feature b7654ff] Greeting, when page is openend, added
 1 file changed, 3 insertions(+)
```
Wir sind uns sicher, dass das Feature funktioniert und sich kein Bug eingeschlichen hat und wollen jetzt den Branch alert-feature in den Master Branch mergen.
Dazu wechseln wir zunächst zurück in den Master Branch:
```bash
$ git checkout master
```
```
Switched to branch 'master'
```
Und mergen nun mit dem Befehl `git merge <branch name>`:
```bash
$ git merge alert-feature
```
```
Updating 558219a..b7654ff
Fast-forward
 index.html | 3 +++
 1 file changed, 3 insertions(+)
```
Alle Änderungen wurden nun vom Branch alert-feature in den Master Branch gemerged und dieser ist nun auf dem aktuellsten Stand des Projekts.
Somit können wir den alert-feature Branch mit  dem Befehl `git branch -d <branch name>` löschen:
```bash
$ git branch -d alert-feature
```
```
Deleted branch alert-feature (was b7654ff).
```

---
### 4.  Letzten commit rückgängig machen
Haben wir aus Versehen falsche Änderungen oder Dateien gelöscht und diesen Zustand committed, so können wir den letzten commit rückgängig machen.
Wir löschen die Datei index.html und commiten diese Änderung:

```bash
$ rm index.html
$ git add *
$ git commit -m "project cleanup"
```
```
[master 4dddf2e] project cleanup
 1 file changed, 14 deletions(-)
 delete mode 100644 index.html
```
Bei Git gibt es einen Pointer, den sogenannten *HEAD*, welcher immer auf den aktuellsten commit, also den aktuellsten Stand zeigt.

Wir wollen also den vorigen commit auswählen indem wir `git reset HEAD~1` verwenden, wobei die Tilde bei `HEAD~1` für Position HEAD minus 1 steht. 

Mit `git checkout -- *` verwerfen wir **alle** Änderungen.
```bash
$ git reset HEAD~1
```
```
Unstaged changes after reset:
D       index.html
```
```bash
$ git checkout -- *
```
Somit haben wir den commit vollständig rückgänig gemacht.

---
### 5. Arbeiten mit einem Remote Repository
In diesem Beispiel verwenden wir ein Remote Repository von [GitHub](https://github.com/). Das Anlegen eines Accounts und die Nutzung beliebig vieler Repositories ist bei einer maximalen Teamgröße von 4 Personen kostenlos.
Im GIF wird gezeigt, wie man ein Repository bei Github erstellt und wie man den Link zu dem Repository kopiert.
![create github repo](https://github.com/step4/git-crash-course/blob/master/createRepo.gif?raw=true)

Möchten wir nun unser lokales Repository auf Github speichern, so müssen wir zuerst ein Remoteziel im lokalen Repository konfigurieren und können anschließend lokale Änderungen hochladen bzw. *pushen*.

Wir fügen das Remoteziel mit dem Befehl `git remote add <remote repository name> <remote repository url>` .  Wir benennen unser Remote Repository *origin*, was oft der Standard ist:
```bash
$ git remote add origin https://github.com/step4/website.git
```
Mit `git push --set-upstream <remote repository name> <branch name>` legen wir  fest zu welchem Remote Repository ein Branch seine lokale Datenbank pushen soll.
```bash
$ git push --set-upstream origin master
```
```
Counting objects: 9, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (9/9), 796 bytes | 796.00 KiB/s, done.
Total 9 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To https://github.com/step4/website.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```
Wir haben jetzt den Master Branch gepusht und können alle folgenden commits mit `git push` pushen.

### Quellen:
[Wikipedia - Git](https://de.wikipedia.org/wiki/Git)
[Pro Git Buch](https://git-scm.com/book/de/v1)