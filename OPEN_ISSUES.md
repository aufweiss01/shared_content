# Offene Punkte - Modul B

Diese Punkte haengen von noch nicht umgesetzten Modulen oder von
einer noch offenen Architekturfrage im Planungs-Chat ab.

## metadata/valuelists.xml - @product
Enthaelt bisher nur die Namenskonvention als Kommentar, keine festen
Werte - produktabhaengig, erst bei konkretem Kundenprojekt/Produkt-
portfolio festzulegen (siehe modul_e_valuelists_vorschlag_v18.md).

## publishing/design-values.xml
DTD noch nicht von Modul G und Modul H festgelegt. Nach Fertig-
stellung: DOCTYPE-Referenz ergaenzen und Struktur pruefen (Konfigurations-
protokoll v13, Abschnitt 6/8).

## .github/workflows/validate.yml
Ruft C und D konkret auf (Konstellation "B allein"), E und F optional
ueber vars.USE_MODULE_E/vars.USE_MODULE_F (Abschnitt 6). Vor dem
ersten echten Lauf: Platzhalter "^<org^>" in allen "uses:"-Zeilen durch
den tatsaechlichen GitHub-Kontonamen von C-H ersetzen. Die Push-/
Merge-Unterscheidung (pull_request- vs. push-Event auf develop) ist
eine Auslegung des Planungs-Chats, noch nicht real verifiziert.

## shared_termbase.tbx
DOCTYPE referenziert TBXBasiccoreStructV02.dtd per blossem Namen.
Sobald Modul F existiert: pruefen, dass dessen XML-Catalog diesen
Namen korrekt aufloest.

## Inhaltliche Platzhalter
- reuse/images/warnsymbol.png: enthaelt nur eine strukturell
  gueltige 1x1-Platzhalter-PNG (sonst DOTX008E in C, siehe unten),
  kein echtes Warnsymbol - vor Produktiveinsatz ersetzen
- reuse/warnings/warnings.dita, reuse/notes/notes.dita,
  reuse/ts_reuse.dita, shared_termbase.tbx: Beispiel-Baustein durch
  echte kundenweite Inhalte ersetzen oder loeschen
- Alle Topics: audience-Attribute und lifecycle-stage im Prolog
  sind absichtlich leer - redaktionell befuellen
