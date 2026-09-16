# shared_content

Modul B - shared_content - fuer [Kundenname].
Wird in Produktrepos (Modul A) optional als Git-Submodul unter shared/ eingebunden.

## Struktur
- reuse/            - Kundenweite wiederverwendbare Bausteine (c_reuse, t_reuse, r_reuse, ts_reuse)
- reuse/warnings/   - Kundenweite Sicherheitshinweise (hazardstatement)
- reuse/notes/      - Kundenweite allgemeine Hinweise
- reuse/images/     - Kundenweit wiederverwendbare Bilder und Warnsymbole
- links/            - Kundenweit genutzte externe Links als Keys
- metadata/         - valuelists.xml - Override-Wertelisten fuer Modul E
- publishing/       - design-values.xml - Override-Gestaltungswerte fuer Module G und H
- shared_termbase.tbx - Kundenweit verbotene Benennungen fuer Modul F

## Wichtig
B muss eigenstaendig gegen C, D, E, F pruefbar sein, auch ohne
eingebundenes Produktrepo (siehe eigene Pipeline in .github/workflows/validate.yml).
G und H (PDF- bzw. HTML-Publishing) werden separat aufgerufen, nicht
als Teil dieser Push-Pipeline - fuer Vorschau/Kontrolle von B allein.

reuse/shared_names.ditamap enthaelt wieder einen companyname-Keydef
als kundenweiten Override. Der Merge mit A's Wert (A gewinnt bei
Ueberschneidung) laeuft ueber ein eigenes Merge-Skript in A's
Pipeline beim kombinierten Build - B stellt nur den Keydef bereit.

Einrichtungsanleitung: siehe SETUP.md (sofern vorhanden)
