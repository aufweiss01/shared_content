# Offene Punkte - Modul B (shared_content)

- Kontoname aufweiss01 wurde beim Anlegen in validate.yml, branch_guard.yml
  und CODEOWNERS eingesetzt. Bei einer Organisation pruefen, ob
  CODEOWNERS ein Team (@organisation/team) statt des Kontos nennen soll.
- Branch-Schutz und externe Partner einrichten (Entscheidungen
  28.09.2026) - GitHub-Einstellungen, keine Dateien. Reihenfolge und
  Details siehe README.md, Abschnitt "Branch-Schutz und externe Partner":
  a) Standardbranch develop; b) CODEOWNERS/validate.yml/branch_guard.yml
  per Pull Request gegen develop; c) einmal Pull Request develop nach
  main; d) danach in main-protect "validierung" und "branch-guard" als
  erforderliche Checks; e) Code-Owner-Freigabe, Bypass nur Repository
  admin fuer Pull Requests; f) Ruleset hotfix/* mit Restrict creations;
  g) Freigabe von Fork-Workflows fuer alle Externen; h) erst dann
  Partner einladen (Rolle Write); i) Voraussetzung GitHub Team.
- Offen, im Pilot zu verifizieren: Ist die Bypass-Rolle
  "Repository admin" bei einem Repo im persoenlichen Konto (keine
  Organisation) in den Rulesets waehlbar? Ergebnis an den
  Planungs-Chat melden.
- Offen, im Pilot zu verifizieren: Der Check "branch-guard" laeuft
  erstmals bei einem Pull Request gegen main, nachdem die Datei dort
  liegt (nach Schritt c). Ob er in main-protect schon vorher
  eintragbar ist, ist ungeprueft. Ergebnis an den Planungs-Chat melden.
- Private Nutzung mit externen Partnern setzt GitHub Team voraus - bei
  GitHub Free wirken Rulesets nur in oeffentlichen Repos.

## metadata/valuelists.xml - @product
Enthaelt bisher nur die Namenskonvention als Kommentar, keine festen
Werte - produktabhaengig, erst bei konkretem Kundenprojekt/Produkt-
portfolio festzulegen (siehe modul_e_valuelists_vorschlag_v18.md).

## .github/workflows/validate.yml
Ruft C und D konkret auf (Konstellation "B allein"), E und F optional
ueber vars.USE_MODULE_E/vars.USE_MODULE_F (Abschnitt 6).

## Inhaltliche Platzhalter
- reuse/images/warnsymbol.png: enthaelt nur eine strukturell
  gueltige 1x1-Platzhalter-PNG (sonst DOTX008E in C, siehe unten),
  kein echtes Warnsymbol - vor Produktiveinsatz ersetzen
- reuse/warnings/warnings.dita, reuse/notes/notes.dita,
  reuse/ts_reuse.dita, shared_termbase.tbx: Beispiel-Baustein durch
  echte kundenweite Inhalte ersetzen oder loeschen
- Alle Topics: audience-Attribute und lifecycle-stage im Prolog
  sind absichtlich leer - redaktionell befuellen
