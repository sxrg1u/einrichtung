# Installationsanleitung: Entwicklungsumgebung für Vite und React (Windows 11)

Diese Anleitung zeigt Schritt für Schritt, wie du auf einem neuen Windows-11-Gerät alles einrichtest, um mit **Vite** und **React** zu programmieren.

**Inhalt**

1. [Voraussetzungen](#1-voraussetzungen)
2. [Node.js installieren](#2-nodejs-installieren)
3. [Git installieren](#3-git-installieren)
4. [Visual Studio Code installieren](#4-visual-studio-code-installieren)
5. [Projekt erstellen, ESLint und Prettier einrichten](#5-projekt-erstellen-eslint-und-prettier-einrichten)
6. [Absolute Pfade einrichten](#6-absolute-pfade-einrichten)

---

## Schnellübersicht: Alle Befehle

| Nr. | Zweck | Befehl |
|---|---|---|
| 1 | Node.js installieren (Alternative) | `winget install OpenJS.NodeJS.LTS` |
| 2 | Git installieren (Alternative) | `winget install --id Git.Git -e --source winget` |
| 3 | VS Code installieren (Alternative) | `winget install Microsoft.VisualStudioCode` |
| 4 | Node.js prüfen | `node -v` |
| 5 | npm prüfen | `npm -v` |
| 6 | Git prüfen | `git --version` |
| 7 | Git-Name setzen | `git config --global user.name "Vorname Nachname"` |
| 8 | Git-E-Mail setzen | `git config --global user.email "deine.email@firma.ch"` |
| 9 | Projekt erstellen | `npm create vite@latest my-react-app -- --template react` |
| 10 | In Projektordner wechseln | `cd my-react-app` |
| 11 | Pakete installieren | `npm install` |
| 12 | Projekt starten | `npm run dev` |
| 13 | Linting ausführen | `npm run lint` |
| 14 | Prettier installieren | `npm install --save-dev --save-exact prettier` |
| 15 | Alias `@` in `vite.config.js` eintragen (im Block `resolve`) | `alias: [{ find: "@", replacement: fileURLToPath(new URL("./src", import.meta.url)) }]` |
| 16 | Datei `jsconfig.json` erstellen (neben `package.json`) | `{ "compilerOptions": { "baseUrl": ".", "paths": { "@/*": ["src/*"] } }, "include": ["src"] }` |
| 17 | Import mit Alias verwenden | `import Test from "@/components/Test"` |

> Alle Befehle ab Nr. 11 im Ordner `my-react-app` ausführen.
> Nr. 15 bis 17 sind keine Befehle, sondern Code für Dateien. Die vollständige Anleitung steht in Kapitel 6.

---

## 1. Voraussetzungen

- Windows 11
- Administratorrechte (für die Installation der Programme)
- Internetverbindung

Alle Befehle in dieser Anleitung führst du in **PowerShell** aus. Du öffnest sie mit einem Klick auf das Startmenü, dann tippst du `PowerShell` und drückst Enter. Später benutzt du das Terminal in VS Code.

---

## 2. Node.js installieren

Node.js brauchst du, damit Vite und React auf deinem PC laufen. Mit Node.js wird auch **npm** installiert, das Programm zum Installieren von Paketen.

### Schritt 1: Installer herunterladen

1. Öffne https://nodejs.org
2. Klicke auf den Button für die **LTS-Version** (Windows Installer, `.msi`).

> LTS ist die stabile Version. Vite braucht mindestens Node.js 20.19 oder 22.12. Die aktuelle LTS-Version erfüllt das.

### Schritt 2: Installieren

1. Starte die heruntergeladene `.msi`-Datei.
2. Klicke auf **Next** und akzeptiere die Lizenzbedingungen.
3. Lass den Installationsordner unverändert.
4. Lass bei den Optionen alles so, wie es voreingestellt ist. Achte darauf, dass **"Add to PATH"** aktiviert ist.
5. Die Option zum Installieren zusätzlicher Tools (Chocolatey, Python) brauchst du nicht anzukreuzen.
6. Klicke auf **Install** und bestätige die Adminrechte mit **Ja**.
7. Klicke auf **Finish**.

> **Alternative per Terminal (PowerShell):**
> ```powershell
> winget install OpenJS.NodeJS.LTS
> ```

### Schritt 3: Installation prüfen

Öffne ein **neues** PowerShell-Fenster (ein altes Fenster kennt Node.js noch nicht) und gib ein:

```powershell
node -v
npm -v
```

Erwartete Ausgabe: zwei Versionsnummern, z. B. `v22.x.x` und `10.x.x`.

### Häufige Probleme

| Problem | Lösung |
|---|---|
| `node` oder `npm` wird nicht erkannt | PowerShell schliessen und neu öffnen. Hilft das nicht: PC neu starten. |
| Fehlermeldung "Ausführung von Skripts ist auf diesem System deaktiviert" | PowerShell öffnen und `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` ausführen. Mit `J` bestätigen. |
| Node.js-Version ist zu alt für Vite | Neue LTS-Version von https://nodejs.org installieren. Sie ersetzt die alte. |

---

## 3. Git installieren

Git verwaltet den Code und die Versionen deines Projekts.

### Schritt 1: Installer herunterladen

1. Öffne https://git-scm.com/download/win
2. Lade den Installer **"Git for Windows/x64 Setup"** herunter.

### Schritt 2: Installieren

1. Starte die Datei und klicke dich durch den Installer. Die Standardeinstellungen passen, ausser hier:
   - **Default editor:** Wähle einen einfachen Editor (z. B. *Visual Studio Code* oder *Notepad++*) statt Vim.
   - **Initial branch name:** Wähle *Override the default branch name* und gib `main` ein.
   - **PATH environment:** Lass *Git from the command line and also from 3rd-party software* ausgewählt.
2. Klicke auf **Install** und danach auf **Finish**.

> **Alternative per Terminal (PowerShell):**
> ```powershell
> winget install --id Git.Git -e --source winget
> ```

### Schritt 3: Installation prüfen

Öffne ein **neues** PowerShell-Fenster (ein bereits offenes Fenster kennt Git noch nicht) und führe aus:

```powershell
git --version
```

Erwartete Ausgabe: eine Versionsnummer, z. B. `git version 2.x.x`

### Schritt 4: Git einrichten

Setze deinen Namen und deine E-Mail-Adresse. Sie erscheinen bei jedem Commit:

```powershell
git config --global user.name "Vorname Nachname"
git config --global user.email "deine.email@firma.ch"
git config --global init.defaultBranch main
```

Kontrolle:

```powershell
git config --list
```

Hier müssen dein Name und deine E-Mail-Adresse auftauchen.

### Häufige Probleme

| Problem | Lösung |
|---|---|
| `git` wird nicht erkannt | PowerShell schliessen und neu öffnen. Hilft das nicht: PC neu starten oder Git neu installieren und die PATH-Option prüfen. |

---

## 4. Visual Studio Code installieren

Visual Studio Code (VS Code) ist der Editor, in dem du den Code schreibst.

### Schritt 1: Installer herunterladen

1. Öffne https://code.visualstudio.com
2. Klicke auf **Download for Windows**.

### Schritt 2: Installieren

1. Starte die heruntergeladene Datei.
2. Akzeptiere die Lizenzbedingungen und klicke auf **Next**.
3. Aktiviere bei den zusätzlichen Aufgaben:
   - **Add "Open with Code" action to Windows Explorer directory context menu**
   - **Add to PATH**
4. Klicke auf **Install** und danach auf **Finish**.

> **Alternative per Terminal (PowerShell):**
> ```powershell
> winget install Microsoft.VisualStudioCode
> ```

### Schritt 3: Installation prüfen

1. Starte VS Code über das Startmenü.
2. Es öffnet sich das Willkommensfenster. Dann ist die Installation in Ordnung.

> **Hinweis:** Falls deine Firma lieber WebStorm (JetBrains) verwendet, funktionieren ESLint und Prettier dort ebenfalls. Diese Anleitung verwendet VS Code.

---

## 5. Projekt erstellen, ESLint und Prettier einrichten

In diesem Kapitel erstellst du dein erstes React-Projekt mit Vite. Danach richtest du zwei Hilfstools ein:

- **ESLint** (Linter): findet Fehler im Code.
- **Prettier** (Formatter): bringt den Code automatisch in ein einheitliches Format.

### Schritt 1: Ordner in VS Code öffnen

1. Erstelle einen Ordner für deine Projekte, z. B. `C:\Users\DeinName\projekte`.
2. Öffne Visual Studio Code.
3. Klicke auf **File > Open Folder...** und wähle diesen Ordner aus.
4. Bestätige mit **Yes, I trust the authors**.

### Schritt 2: Terminal öffnen

1. Klicke in VS Code auf **Terminal > New Terminal**.
2. Unten öffnet sich ein Terminal. Es zeigt den Pfad deines Ordners.

### Schritt 3: Vite-Projekt erstellen

1. Gib im Terminal diesen Befehl ein:

   ```powershell
   npm create vite@latest my-react-app -- --template react
   ```

2. Wenn `Ok to proceed? (y)` erscheint, tippe `y` und drücke Enter.
3. Bei der Frage **Which linter to use?** wähle mit den Pfeiltasten **ESLint** und drücke Enter.
4. Falls weitere Fragen kommen (z. B. JavaScript oder TypeScript), wähle **JavaScript**.
5. Falls gefragt wird, ob gleich installiert und gestartet werden soll, wähle **No**. Das machen wir im nächsten Schritt selbst.

### Schritt 4: Projekt öffnen und Pakete installieren

1. Wechsle in den Projektordner:

   ```powershell
   cd my-react-app
   ```

2. Installiere alle Pakete:

   ```powershell
   npm install
   ```

3. Öffne den Projektordner in VS Code: **File > Open Folder...** und wähle `my-react-app`.

> **Wichtig:** Ab jetzt muss `my-react-app` der geöffnete Hauptordner sein. Sonst funktionieren ESLint und Prettier nicht richtig.

### Schritt 5: Projekt starten

1. Öffne ein Terminal (**Terminal > New Terminal**). Der Pfad muss mit `my-react-app` enden.
2. Starte das Projekt:

   ```powershell
   npm run dev
   ```

3. Öffne im Browser die Adresse, die im Terminal steht (meistens http://localhost:5173).
4. Du siehst die Vite-React-Startseite. Beenden kannst du das Projekt im Terminal mit `Ctrl + C`.

### Schritt 6: Auto Save aktivieren (empfohlen)

ESLint und Prettier arbeiten nur mit **gespeicherten** Dateien. Damit du das Speichern nicht vergisst:

1. Klicke auf **File > Auto Save**.
2. Es erscheint ein Haken. Ab jetzt wird automatisch gespeichert.

### Schritt 7: Was ist ein Linter?

Ein Linter prüft deinen Code automatisch auf Probleme. Er vergleicht den Code mit Regeln. Er findet zum Beispiel:

- Syntaxfehler (z. B. fehlende Klammern)
- Mögliche Laufzeitfehler (z. B. Variablen, die es nicht gibt)
- Stilprobleme (z. B. falsche Einrückung)
- Unnötigen Code (z. B. nicht benutzte Variablen)
- Verstösse gegen Best Practices (z. B. `==` statt `===`)
- React-spezifische Fehler

Im Projekt ist **ESLint** schon vorbereitet:

- Die Regeln stehen in der Datei `eslint.config.js`.
- In der Datei `package.json` gibt es das Script `"lint": "eslint ."`.

### Schritt 8: Linting ausführen

1. Öffne ein Terminal im Projektordner (Pfad endet mit `my-react-app`).
2. Führe aus:

   ```powershell
   npm run lint
   ```

3. Wenn alles in Ordnung ist, erscheint keine Ausgabe. Sonst zeigt ESLint die Datei, die Zeile und das Problem.

### Schritt 9: ESLint in VS Code aktivieren

Damit du Fehler schon beim Schreiben siehst:

1. Klicke links auf das **Extensions**-Symbol (oder `Ctrl + Shift + X`).
2. Suche nach **ESLint** (Herausgeber: Microsoft).
3. Klicke auf **Install**.
4. Starte VS Code neu. Falls eine Meldung erscheint, klicke auf **Allow**.

> Die Extension arbeitet im Hintergrund. Du erkennst sie an den roten oder gelben Wellenlinien im Code und am Tab **PROBLEMS**.

### Schritt 10: Linting testen

1. Öffne die Datei `src/App.jsx`.
2. Füge ganz unten diese Zeile ein:

   ```js
   const unused = 1
   ```

3. Speichere mit `Ctrl + S` (bei aktivem Auto Save geschieht das automatisch).
4. VS Code unterstreicht die Zeile. Wenn du mit der Maus darüberfährst, steht dort, dass die Variable nie benutzt wird.
5. Führe `npm run lint` aus. Der Fehler erscheint auch im Terminal.
6. Lösche die Zeile wieder.

### Schritt 11: Was ist ein Formatter?

Ein Formatter bringt deinen Code automatisch in ein einheitliches Format (Einrückung, Anführungszeichen, Zeilenlänge). Das bringt dir:

- **Einheitlichen Stil:** Alle im Team schreiben gleich aussehenden Code.
- **Zeitersparnis:** Du musst nichts von Hand formatieren.
- **Weniger Merge-Konflikte:** Unterschiedliche Schreibstile führen nicht mehr zu Konflikten.

Wir verwenden **Prettier**. Es ist der Standard im Frontend-Bereich.

### Schritt 12: Prettier installieren

1. Öffne ein Terminal im Projektordner (Pfad endet mit `my-react-app`).
2. Führe aus:

   ```powershell
   npm install --save-dev --save-exact prettier
   ```

### Schritt 13: Prettier konfigurieren

1. Klicke im Explorer mit der rechten Maustaste auf eine leere Stelle unter den Dateien von `my-react-app` und wähle **New File**.
2. Nenne die Datei `.prettierrc` (mit Punkt am Anfang, ohne Endung).
3. Füge diesen Inhalt ein:

   ```json
   {
     "printWidth": 100,
     "trailingComma": "es5",
     "tabWidth": 4,
     "semi": false,
     "singleQuote": false
   }
   ```

4. Speichern.

Das bedeutet:

- Zeilen sind maximal 100 Zeichen lang.
- Am Ende von Listen steht ein Komma (wenn möglich).
- Einrückungen sind 4 Leerzeichen.
- Keine Semikolons am Zeilenende.
- Texte stehen in doppelten Anführungszeichen (`"Beispiel"`).

### Schritt 14: Prettier in VS Code einrichten

1. Öffne die Extensions (`Ctrl + Shift + X`).
2. Suche nach **Prettier - Code formatter**.
3. Klicke auf **Install**.
4. Öffne die Einstellungen mit `Ctrl + ,`.
5. Suche nach **Default Formatter** und wähle **Prettier - Code formatter** aus der Liste.
6. Suche nach **Format On Save** und setze bei **Editor: Format On Save** einen Haken.
7. Starte VS Code neu.

### Schritt 15: Prettier testen

1. Öffne die Datei `src/App.jsx`.
2. Verändere absichtlich die Einrückung einer Zeile, z. B. mit ein paar zusätzlichen Leerzeichen am Zeilenanfang.
3. Speichere mit `Ctrl + S`.
4. Prettier korrigiert die Einrückung automatisch. Dann funktioniert es.

> **Hinweis:** Beim ersten Speichern ändert Prettier evtl. viele Zeilen, weil die Vite-Vorlage einen anderen Stil hat (z. B. 2 statt 4 Leerzeichen). Das ist normal.

### Häufige Probleme

| Problem | Lösung |
|---|---|
| `npm` wird nicht erkannt | Terminal schliessen und neu öffnen. Hilft das nicht: PC neu starten. |
| `npm run lint` meldet `ENOENT ... package.json` | Du bist im falschen Ordner. Mit `cd my-react-app` in den Projektordner wechseln. |
| `npm run lint` zeigt keine Ausgabe, obwohl ein Fehler im Code ist | Datei mit `Ctrl + S` speichern, dann nochmal ausführen. Prüfen, ob die Datei in `my-react-app/src` liegt. |
| ESLint unterstreicht nichts | Prüfen, ob du den Ordner `my-react-app` geöffnet hast (nicht den Ordner darüber). Danach VS Code neu starten. |
| Prettier formatiert beim Speichern nicht | Prüfen: Ist **Prettier - Code formatter** als Default Formatter gewählt? Ist **Format On Save** aktiv? Liegt `.prettierrc` direkt in `my-react-app`? Danach VS Code neu starten. |
| Port 5173 ist belegt | Vite nimmt automatisch den nächsten Port. Nimm die Adresse aus dem Terminal. |

---

## 6. Absolute Pfade einrichten

Mit einem **Alias** kannst du in Imports `@/` statt `../../` schreiben. `@` steht dabei für den Ordner `src`. Das macht Imports kürzer und übersichtlicher, vor allem in grösseren Projekten und im Team.

Beispiel:

```jsx
// Ohne Alias
import CustomButton from "../../components/CustomButton"

// Mit Alias
import CustomButton from "@/components/CustomButton"
```

### Schritt 1: vite.config.js anpassen

1. Öffne die Datei `vite.config.js` (im Hauptordner von `my-react-app`).
2. Ersetze den ganzen Inhalt durch:

   ```js
   import { defineConfig } from "vite"
   import react from "@vitejs/plugin-react"
   import { fileURLToPath } from "url"

   export default defineConfig({
       plugins: [react()],
       resolve: {
           alias: [{ find: "@", replacement: fileURLToPath(new URL("./src", import.meta.url)) }],
       },
   })
   ```

3. Speichern.

Damit weiss Vite, dass `@` der Ordner `src` ist.

### Schritt 2: jsconfig.json erstellen

1. Klicke im Explorer mit der rechten Maustaste auf eine leere Stelle unter den Dateien von `my-react-app` und wähle **New File**.
2. Nenne die Datei `jsconfig.json`. Sie muss im Hauptordner liegen, neben `package.json`.
3. Füge diesen Inhalt ein:

   ```json
   {
     "compilerOptions": {
       "baseUrl": ".",
       "paths": {
         "@/*": ["src/*"]
       }
     },
     "include": ["src"]
   }
   ```

4. Speichern.

Damit versteht VS Code den Alias, und die Autovervollständigung funktioniert.

### Schritt 3: Alias testen

1. Erstelle im Ordner `src` einen neuen Ordner `components`.
2. Erstelle darin die Datei `Test.jsx` mit diesem Inhalt:

   ```jsx
   function Test() {
       return <p>Alias funktioniert</p>
   }

   export default Test
   ```

3. Öffne `src/App.jsx` und füge oben bei den Imports hinzu:

   ```jsx
   import Test from "@/components/Test"
   ```

4. Füge im `return` von `App` direkt nach `<>` diese Zeile ein:

   ```jsx
   <Test />
   ```

5. Speichern.
6. Starte das Projekt im Terminal (Pfad endet mit `my-react-app`):

   ```powershell
   npm run dev
   ```

7. Öffne die Adresse aus dem Terminal im Browser. Dort steht **"Alias funktioniert"**.
8. Beende das Projekt mit `Ctrl + C`.

### Häufige Probleme

| Problem | Lösung |
|---|---|
| Fehler `Failed to resolve import "@/components/Test"` | Prüfen, ob `vite.config.js` genau wie oben aussieht. Danach `npm run dev` beenden (`Ctrl + C`) und neu starten. |
| Der Alias funktioniert im Browser, aber VS Code unterstreicht den Import | Prüfen, ob `jsconfig.json` im Hauptordner liegt (neben `package.json`). Danach VS Code neu starten. |
| Die Seite zeigt "Alias funktioniert" nicht | Prüfen, ob `<Test />` im `return` von `App` steht und `App.jsx` gespeichert ist. |

---

## Fertig

Deine Entwicklungsumgebung ist eingerichtet. Du kannst jetzt mit der Entwicklung von Frontend-Projekten mit Vite und React beginnen.