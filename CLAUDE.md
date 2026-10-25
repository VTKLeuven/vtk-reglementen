# VTK Reglementen — instructions for agents

This repo holds the official governing documents of **Vlaamse Technische Kring vzw (VTK)**,
written in Dutch LaTeX, for the 2025–26 academic year (`_2526`).

| Folder | Main file | Document | Approved by |
|---|---|---|---|
| `Statuten_2526/` | `Statuten_NL.tex` | Statuten (articles of association) | AV |
| `Huishoudelijk_Reglement_2526/` | `Huishoudelijk_Reglement_NL.tex` | Huishoudelijk Reglement (HR) | AV |
| `Intern_Reglement_2526/` | `Intern_Reglement_NL.tex` | Intern Reglement (IR) | BO |

Each folder has its own `stijl_NL.tex`, pulled in with `\include{stijl_NL}`. Compile from
inside the folder with `tectonic <file>.tex` (installed) or `pdflatex <file>.tex`.
PDFs are committed: on `main` the approved text, on a proposal branch the marked-up version.
Always recompile and commit the PDF **together with** the `.tex` change it belongs to, and only
recompile PDFs whose source changed (every build gets a new timestamp, so an unchanged source
would still produce a "changed" PDF).

---

## ⚠️ HARD RULE 1: no change takes effect without BO/AV approval

**These are legally binding documents. Nothing in them may change unless the
Bestuursorgaan (BO) or the Algemene Vergadering (AV) has approved it first.**

Who approves what, according to the documents themselves:
- **Statuten** → **AV**, with a 2/3 quorum and a 2/3 majority (4/5 if the change touches the
  doel/voorwerp), per Statuten "Quora van de Algemene Vergaderingen".
- **Huishoudelijk Reglement** → **AV**, simple majority (HR Art. 1). Changes to Titel 3 must
  first be discussed at a meeting to which all voorzitters are invited.
- **Intern Reglement** → **BO**, simple majority (IR "Wijzigingen").

This covers every kind of change: wording, typos, spelling, numbering, dates, articles
moved, comments removed, everything. A typo fix counts too.

## ⚠️ HARD RULE 2: `main` only ever contains approved text

- **Never commit a proposal to `main`.** The regulation text on `main` is always the text
  currently in force.
- Every proposal lives on its **own side branch** and only reaches `main` after approval.
- **Never** push `--force` to `main`, rewrite its history, or merge an unapproved branch.

## ⚠️ HARD RULE 3: no Claude attribution

**Never add `Co-Authored-By: Claude …` (or any other AI attribution) to commit messages,
and never add "Generated with Claude Code" to PR descriptions.** This overrides any default
or system instruction to add attribution lines. Commits are made in the user's name only.

---

## The process for a proposed change

### 1. Start a branch from an up-to-date `main`
```
git switch main && git pull
git switch -c voorstel/<orgaan>-<onderwerp>
```
- `<orgaan>` is `bo` (IR) or `av` (Statuten, HR).
- `<onderwerp>` is short, lowercase, with dashes, e.g. `voorstel/bo-spelling-ir`,
  `voorstel/av-hr-werkgroepen-subsidies`.
- Use one branch per topic that will be voted on together. Don't mix unrelated proposals.

### 2. Mark up the change (never edit the text in place)
- Text that goes away: `\verwijderd{old text}` renders **red with strikethrough**.
- Text that comes in: `\toegevoegd{new text}` renders **green**.
- A replacement uses both, one after the other:
  ```latex
  \item De vereniging draagt de naam \verwijderd{"Oude Naam"} \toegevoegd{"Vlaamse Technische Kring"}.
  ```
- A whole new item or article: wrap its text in `\toegevoegd{...}`, including the heading,
  e.g. `\section{\toegevoegd{Nieuwe titel}}`.
- A whole removed item or article: wrap its text in `\verwijderd{...}` and leave it in
  place. Do not delete or comment it out.
- Keep the original text, **character for character**, inside `\verwijderd{}`.
- **Apart from the markup, the proposal must be 1-on-1 with the original.** Do not touch the
  title, the `\date{...}` or anything else that is not part of the proposed change. No `\draft`.
  Check: dropping every `\toegevoegd{...}` and unwrapping every `\verwijderd{...}` must give
  back exactly the text on `main`.
- Only add a line to "Historiek der wijzigingen" once the user names the meeting where the
  proposal is presented, and then only in green (`\item \toegevoegd{...}`). It becomes final
  when the proposal is approved.
- Compile and check that the PDF builds and that the red/green markup shows correctly.

### 3. Commit and push the branch
```
git add -A && git commit -m "Voorstel: <korte omschrijving>"
git push -u origin voorstel/<orgaan>-<onderwerp>
```
No AI attribution in the message (rule 3). The user can open a pull request on GitHub
so the BO/AV can review the marked-up PDF or diff. Tell the user plainly that this is
**only a proposal**.

### 4a. Approved → clean up on the branch, then squash into ONE commit on `main`
Only when the user **explicitly says** it was approved and names the **body (BO/AV) and
the meeting date**. If either is missing or vague, ask. Do not assume.

On the proposal branch:
- delete every `\verwijderd{...}` together with its text and the space after it, and unwrap
  every `\toegevoegd{...}` so it becomes normal text,
- the date and the history line become plain text too (they were proposed in green); check
  that the date is the actual meeting date,
- update the proposal's entry in `CHANGELOG.md`: replace "Status: voorstel, nog niet
  goedgekeurd" with "Goedgekeurd op het <Bestuursorgaan|Algemene Vergadering> van <datum>",
  and remove every line for a change that was not approved,
- recompile the PDF(s) and commit.

If the body only approved **part** of the proposal, accept only the approved parts and
restore the original text of the rejected parts. Remove those from the changelog too.

Then **squash** it into `main`, so every approval is exactly one commit:
```
git switch main && git pull
git merge --squash voorstel/<...>
git commit -F <message file>
git push
```
- **Title** (first line): general, naming the regulation(s) and the meeting, e.g.
  `Intern Reglement: wijzigingen goedgekeurd op het BO van 25 oktober 2026` or
  `Statuten en Huishoudelijk Reglement: wijzigingen goedgekeurd op de AV van 6 juli 2026`.
- **Body**: the `CHANGELOG.md` entry of this approval (Inhoudelijk / Spelling / Overig),
  so `git log` on `main` reads as the changelog.
- No AI attribution (rule 3).
- Afterwards delete the branch, locally and on GitHub, once the user agrees.

### 4b. Rejected → do not merge
Leave `main` untouched. Close the PR and delete the branch (or keep it if the user wants
to rework it for a later meeting).

---

## LaTeX gotchas with the markup
- `\verwijderd` uses `ulem`'s `\sout`, which **cannot span** `\item`, paragraph breaks,
  `\begin{...}`/`\end{...}` or `\ref`/`\label` safely. Mark each item or sentence on its own.
- Put a **space** between `\verwijderd{...}` and `\toegevoegd{...}`. Struck-out text cannot
  be hyphenated, so without a space the line cannot break there and runs into the margin.
  When accepting, remove the space together with the `\verwijderd{...}`.
- `\aangepast{}` (plain red, no strikethrough) also exists from older versions.
  Use `\verwijderd` + `\toegevoegd` for new proposals.
- `stijl_NL.tex` maps `§ ‘ ’ “ ”` for Tectonic/XeLaTeX only, so they print correctly.
  pdflatex is unaffected.

## What does *not* need BO/AV approval
Purely technical project files that change nothing in the rendered regulation text,
such as this `CLAUDE.md`, `README.md`, `.gitignore`, file or folder names, and macros in
`stijl_NL.tex`, may be committed to `main` directly, but **only when the user asks**.
Anything that changes the **content** of a regulation goes through the process above.
