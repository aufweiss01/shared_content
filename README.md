# shared_content

Modul B aus dem modularisierten DITA-Dokumentenmanagement
(Konfigurationsprotokoll v23, Abschnitt 7). Enthält die kundenweit
wiederverwendbaren Bausteine für [Kundenname]. Wird in Produktrepos
(Modul A) optional als Git-Submodul unter `shared/` eingebunden und muss
eigenständig prüfbar sein – auch ohne eingebundenes Produktrepo.

## Wofür dieses Repo zuständig ist – und wofür nicht

- **Zuständig:** Kundenweite Bausteine (`reuse/`: `c_/t_/r_/ts_reuse.dita`,
  Warnhinweise, allgemeine Hinweise, Bilder), kundenweite Keys
  (`reuse/shared_names.ditamap`, `links/external_links.ditamap`),
  kundenweite Override-Werte für Metadaten (`metadata/valuelists.xml`) und
  Gestaltung (`publishing/design-values.xml`), kundenweit verbotene
  Benennungen (`shared_termbase.tbx`).
- **Nicht zuständig:** DTD-/Referenzprüfung (Modul C), Linkprüfung (D),
  Metadaten-Regelprüfung (E), Terminologieprüfung (F), PDF-/HTML-
  Publikation (G/H) – B ruft diese Module auf, implementiert ihre Logik
  aber nicht selbst. Ebenfalls nicht in B: Titelseiten- und
  Impressum-Topics (`titlepage.dita`, `imprint.dita`) – sie liegen in A.

## Struktur

- `reuse/` – Kundenweite wiederverwendbare Bausteine (`c_reuse`, `t_reuse`,
  `r_reuse`, `ts_reuse`) und die Keyspace-Map `reusables.ditamap`
- `reuse/warnings/` – Kundenweite Sicherheitshinweise (`hazardstatement`)
- `reuse/notes/` – Kundenweite allgemeine Hinweise
- `reuse/images/` – Kundenweit wiederverwendbare Bilder und Warnsymbole
- `links/` – Kundenweit genutzte externe Links als Keys
- `metadata/` – `valuelists.xml`, Override-Wertelisten für Modul E
- `publishing/` – `design-values.xml`, Override-Gestaltungswerte für
  Module G und H
- `shared_termbase.tbx` – Kundenweit verbotene Benennungen für Modul F
- `.github/workflows/validate.yml` – Pipeline „B allein“ (Module C–F)
- `.github/workflows/branch_guard.yml` – Aufruf des Wächters aus Modul J
- `.github/CODEOWNERS` – Code Owner (siehe „Branch-Schutz und externe
  Partner“)

## Wichtig

B muss eigenständig gegen C, D, E, F prüfbar sein, auch ohne
eingebundenes Produktrepo (eigene Pipeline in
`.github/workflows/validate.yml`). G und H (PDF- bzw. HTML-Publishing)
werden separat aufgerufen, nicht als Teil dieser Pipeline – für
Vorschau/Kontrolle von B allein.

**`companyname` und Impressum-Keys:** `reuse/shared_names.ditamap`
enthält kundenweite Keydefs als Override zu A: `companyname`, `legalname`,
`street`, `housenumber`, `postalcode`, `addressaddition`, `city`,
`website`, `email`. „Override“ bezeichnet nur B's Rolle (optional,
kundenweit). Bei Überschneidung mit A gewinnt A; den Merge übernimmt
`merge_names.py` in A's Pipeline – B stellt nur die Keydefs bereit.
`businessunit` und `docdate` stehen bewusst nicht in B.

## Einrichtung

```cmd
modul_b.bat [Kontoname] ["Handle1 Handle2 ..."]
```

Legt den Ordner `shared_content` neben der `.bat`-Datei an und bricht ab,
wenn er bereits existiert.

Das Skript braucht **zwei Werte**, die oft, aber nicht immer gleich sind.
Beide werden als Parameter übergeben oder beim Start abgefragt:

1. **Konto der Module** (Parameter 1): das GitHub-Konto, in dem die Module
   C bis J liegen. Der Name wird automatisch in die fünf `uses:`-Zeilen
   (`validate.yml`, `branch_guard.yml`) eingesetzt; die Vorlagen enthalten
   nur einen Platzhalter.
2. **Code Owner** (Parameter 2): ein oder mehrere GitHub-Handles, die
   Pull Requests freigeben dürfen. Mehrere durch Leerzeichen trennen und
   in Anführungszeichen setzen, z. B. `modul_b.bat aufweiss01 "aufweiss01
   max-muster"`. Ohne Angabe (bzw. mit Enter bei der Abfrage) gilt das
   Konto aus Parameter 1. Alle Handles werden in `CODEOWNERS` für `*` und
   `/.github/` eingetragen.

Jeder Name bzw. Handle darf nur Buchstaben, Ziffern und Bindestrich
enthalten (ohne `@`), höchstens 39 Zeichen, kein Bindestrich am Anfang
oder Ende; bei ungültiger Eingabe bricht die `.bat` ab, bevor etwas
angelegt wird. **Code Owner brauchen Schreibrecht im Repo** – Handles ohne
Schreibrecht ignoriert GitHub. Teams (`@organisation/team`) nimmt die
`.bat` nicht entgegen und müssen von Hand in `CODEOWNERS` eingetragen
werden. Für das Einsetzen wird PowerShell benötigt (unter Windows
vorhanden).

**Bereits angelegtes Repo (z. B. Pilot):** `modul_b.bat` nicht im
bestehenden Repo ausführen. Stattdessen in einem leeren Ordner neu
erzeugen (gleiche Werte für Konto und Code Owner) und nur die geänderten Dateien in den
lokalen Klon des bestehenden Repos übernehmen:

- Ganze Dateien kopieren: `.github/workflows/validate.yml`,
  `.github/workflows/branch_guard.yml`, `.github/CODEOWNERS`,
  `README.md`, `OPEN_ISSUES.md`.
- Von Hand ergänzen (nicht kopieren, sonst gehen echte Inhalte
  verloren): in `metadata/valuelists.xml` und `publishing/design-values.xml`
  jeweils die `DOCTYPE`-Zeile unter der XML-Deklaration; in
  `reuse/shared_names.ditamap` die Keydefs `website` und `email`.

Auf einem eigenen Branch per `git diff` prüfen und per Pull Request gegen
`develop` einbringen.

## CI/CD-Pipeline (`validate.yml`)

Trigger: `pull_request` auf `develop` und `main` (inkrementelle Prüfung
vor dem Merge) und `push` auf `develop` (vollständiger Scan nach dem
Merge). Der Trigger für Pull Requests gegen `main` ist neu
(28.09.2026) – dort ist der Job `validierung` erforderlicher Check.
Ruft Modul C (`--root reuse/reusables.ditamap`, immer voller Lauf),
Modul D (`--files`/`--input` je nach Event) sowie optional Module E/F auf
(`vars.USE_MODULE_E` / `vars.USE_MODULE_F`). Den Kontonamen in den vier
`uses:`-Zeilen setzt `modul_b.bat` ein (siehe „Einrichtung“). Die Version
von Modul C ist `v1.0.1`, die von D, E, F `v1.0.0`.

## Branch-Schutz und externe Partner

Externe Partner arbeiten mit der Rolle **Write** direkt in diesem Repo.
Geschützt wird über zwei Mechanismen: Rulesets mit Code-Owner-Freigabe
(GitHub-Einstellungen, keine Dateien) und den Wächter aus Modul J
(`partner_collaboration`), der Pull Requests nach `main` nur von
`develop` oder `hotfix/*` und nur aus diesem Repo zulässt (keine Forks).

**Zugehörige Dateien:**

- `.github/CODEOWNERS` – Code Owner für alle Dateien (`*`) und eigens für
  `/.github/`. Von `modul_b.bat` mit den angegebenen Code-Owner-Handles
  erzeugt (nicht mit dem Konto der Module, sofern abweichend); alle
  Code Owner brauchen Schreibrecht im Repo. Bei Organisationen ggf. von
  Hand auf ein Team (`@organisation/team`) umstellen. GitHub liest immer die Fassung auf dem **Zielbranch** des
  Pull Requests – die Datei muss daher auf `develop` **und** `main`
  liegen.
- `.github/workflows/branch_guard.yml` – dünne Aufruferdatei, Trigger
  `pull_request_target` gegen `main`, Job `branch-guard` (Name des
  erforderlichen Checks). Kontoname in der `uses:`-Zeile von
  `modul_b.bat` eingesetzt. Kein Checkout von Pull-Request-Code
  (Sicherheit bei `pull_request_target`). Erlaubte Quellbranches:
  Standardwert aus Modul J (`develop,hotfix/*`).

**Einrichtung durch den Administrator – Reihenfolge einhalten:**

- **a)** **Standardbranch** auf `develop` stellen (Settings > General >
  Default branch).
- **b)** `CODEOWNERS`, `validate.yml` und `branch_guard.yml` per Pull
  Request gegen `develop` einbringen.
- **c)** Einmal Pull Request `develop` → `main`, damit die Dateien auch
  auf `main` liegen. Der Wächter läuft bei diesem ersten Pull Request noch
  nicht – `pull_request_target` liest die Workflow-Datei vom Zielbranch,
  und dort liegt sie erst nach diesem Merge.
- **d)** Erst danach in `main-protect` die Checks `validierung` und
  `branch-guard` als erforderlich eintragen – sie stehen erst nach einem
  ersten Lauf zur Auswahl (siehe `OPEN_ISSUES.md`).
- **e)** `develop-protect` und `main-protect`: Code-Owner-Freigabe
  verlangen, 1 Freigabe. Bypass nur für die Rolle „Repository admin“,
  Modus „nur für Pull Requests“ (nötig, weil GitHub Autoren ihre eigenen
  Pull Requests nicht freigeben lässt). Bei einem Repo im persönlichen
  Konto prüfen, ob diese Rolle wählbar ist (siehe `OPEN_ISSUES.md`).
- **f)** Eigenes Ruleset für `hotfix/*` mit „Restrict creations“ – Bypass
  wie oben, damit nur der Administrator Hotfix-Branches anlegt.
- **g)** Settings > Actions > General: Workflows aus Fork-Pull-Requests
  nur nach Freigabe ausführen, Einstellung sinngemäß „Require approval for
  all outside collaborators“ (Bezeichnung kann je nach GitHub-Stand
  abweichen, z. B. „external contributors“).
- **h)** Erst danach Partner einladen (Settings > Collaborators, Rolle
  **Write**). In einem öffentlichen Repo **keinen** `SUBMODULE_PAT`
  hinterlegen.
- **i)** **Voraussetzung für private Repos:** Bei GitHub Free wirken
  Rulesets nur in öffentlichen Repos. Private Nutzung mit externen
  Partnern setzt GitHub Team voraus.

**Hotfix-Ablauf:** Der Administrator legt `hotfix/…` von `main` aus an,
Pull Request `hotfix/…` → `main`, danach von Hand Pull Request `main` →
`develop`, damit die Korrektur auch in `develop` ankommt. Eine
Automatik für die Rückführung ist bewusst zurückgestellt.

## Betrieb als Submodul unter `shared/` in A

Ist B in A als Submodul eingebunden, gehören die Dateien unter
`shared/.github/` nicht zum Dateibaum von A (A enthält nur einen Verweis
auf einen Commit von B). Daraus folgt:

- Die Workflows aus B (`validate.yml`, `branch_guard.yml`) laufen in A
  **nicht** – GitHub führt nur Workflows aus `.github/workflows/` im
  Wurzelverzeichnis des jeweiligen Repos aus.
- `CODEOWNERS` aus B wirkt nur im Repo B selbst. In A gilt A's eigene
  Datei; eine Änderung des Submodul-Verweises fällt dort unter `*`.
- Der Schutz von B (Rulesets, Code-Owner-Freigabe, Wächter) wird
  ausschließlich in B eingerichtet. Änderungen an B gehen zuerst durch B's
  Pull Requests; erst danach aktualisiert A den Verweis per eigenem Pull
  Request.
- Die Prüfmodule in A durchsuchen `shared/` nur nach `.dita`-, `.ditamap`-
  bzw. `.tbx`-Dateien; `.github/` wird dabei nicht erfasst.
