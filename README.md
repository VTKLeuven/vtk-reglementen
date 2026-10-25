# VTK Reglementen

De LaTeX-bronbestanden van de Statuten, het Huishoudelijk Reglement en het Intern Reglement van Vlaamse Technische Kring vzw (werkingsjaar 2025–2026).

| Map | Document | Goedgekeurd door |
|---|---|---|
| `Statuten_2526/` | Statuten | AV |
| `Huishoudelijk_Reglement_2526/` | Huishoudelijk Reglement | AV |
| `Intern_Reglement_2526/` | Intern Reglement | BO |

## Compileren

Ga naar de map van het document en draai `pdflatex`, bijvoorbeeld:

```
cd Statuten_2526
pdflatex Statuten_NL.tex
```

Je kan een map ook gewoon als zip uploaden in Overleaf.

De PDF's staan mee in de repo. Op `main` is dat de geldende versie, op een voorstel-branch de versie met de rode en groene markering.

## Wijzigingen

Niets in deze teksten verandert zonder goedkeuring van het BO of de AV. Op `main` staat altijd de tekst die op dat moment geldt.

Een wijziging voorstellen:

1. Maak een aparte branch vanaf `main`, bv. `voorstel/bo-spelling-ir` of `voorstel/av-hr-werkgroepen`.
2. Pas de tekst niet gewoon aan, maar markeer wat verandert:
   ```latex
   \verwijderd{oude tekst}    % rood en doorgestreept
   \toegevoegd{nieuwe tekst}  % groen
   ```
   Verder blijft alles exact zoals het origineel, ook de datum.
3. Push de branch en open een pull request, zodat iedereen het voorstel kan nalezen.

Goedgekeurd? Haal dan op de branch de markering weg en zet in `CHANGELOG.md` bij het voorstel de datum van goedkeuring. Daarna gaat alles als één commit naar `main` (squash merge), met als titel bv. "Intern Reglement: wijzigingen goedgekeurd op het BO van 25 oktober 2026" en de changelog als beschrijving. Wordt het voorstel weggestemd, dan sluit je de pull request zonder te mergen.
