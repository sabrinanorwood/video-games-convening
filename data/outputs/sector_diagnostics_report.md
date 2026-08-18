# Round 2 sector diagnostics — what the data can legitimately support

**Purpose.** A reviewer asked whether different priorities were driven more by
different sectors, particularly **academia vs industry**. This is a *descriptive*
diagnostic to decide whether that story is supportable **before** deciding whether
to report it. No significance tests were run (the sector groups were not sampled or
pre-registered for hypothesis testing).

**Reproducible code:** `code/5_sector_diagnostics.Rmd`.
**Raw variables used:** `sector` (+ `sector_other`) for sector; `design and exp_1..13`,
`respsonse and gov_1..10`, `research and data_1..7` (rank values, 1 = most preferred)
for priorities. Source file: `data/round 2 - delphi/Delphi round2 cleaned manual.csv`
(the same file the STV analysis uses).

---

## TL;DR

- **Academia and industry broadly agree on *Research & data*** (both put
  *Industry–academic data sharing* first) and share several *Design* priorities
  (*Harms & benefits*, *User trust & safety*).
- **The clearest divergences are coherent and plausible**, but rest on a **small
  industry sample (n ≈ 14)**:
  - *Design*: **Dark patterns** is academia's #1 (80.8% top-5, 38.5% first) but
    industry's low priority (28.6% top-5, **0% first**).
  - *Governance*: academia leans to **external Regulation & enforcement** and
    **dark-pattern scrutiny**; industry leans to **Self-regulation & co-regulation**
    (industry 35.7% first vs academia 3.8%) and **Sector-wide collaboration**.
- **Is the aggregate "just academia"?** Only partly, and only in one domain:
  - *Design*: the aggregate STV ranking is **identical** to the academia-only
    ranking — so here the academic plurality does drive the result.
  - *Research*: the aggregate reflects a **cross-sector consensus core** (matches
    industry's set 5/5, academia 3/5).
  - *Governance*: the aggregate is a **genuine blend**; its #1 (*Self-regulation*)
    is industry's top pick, not academia's.
- **Recommendation:** worth reporting **only as a clearly-labelled, descriptive
  supplementary analysis with sample sizes shown** — it answers the reviewer and is
  reassuring for *Research*. Presenting it as tested "sector effects" would mislead:
  industry n ≈ 14 and the other sectors (≤ 6) are too small for stable rankings.

---

## 1. How sector was actually measured (and a wording discrepancy)

- Sector is a **single multi-select field** (`sector`), plus a free-text
  `sector_6_TEXT` / `sector_other` for "Other". There is **no separate "primary
  sector" variable** in the data.
- The **question text** reads: *"Which of the following best describes your primary
  professional sector? (Select the one that best reflects your role in this space)."*
  So the wording asks for **one** sector…
- …but the item was implemented/exported as a **checkbox (multiple-answer)** field,
  so respondents *could* select more than one. In practice **only 1 of 60**
  respondents did (Academic researcher + Other). Everyone else picked one.
- **Discrepancy for the manuscript:** the text ("primary … select the one") and the
  instrument (multi-select) disagree. The manuscript's two statements — "primary
  professional sector" and "participants could select more than one sector" — are
  *both* traceable to this: the *prompt* said primary, the *field* allowed multiple.
  Recommended wording: *"Participants indicated their professional sector(s) from a
  fixed list; the item allowed multiple selections but only one respondent selected
  more than one, so sector is treated as effectively single-select."*
- **Parsing caveat (matters for reproducibility):** the exported strings concatenate
  the *full* option labels, and the Industry label itself contains commas. Splitting
  on `,` — or on `;` as `2_delphi_round2.Rmd` does — corrupts the field. Sector must
  be decoded by matching the **canonical option text as a substring** (done in
  `5_sector_diagnostics.Rmd`). *Note:* the existing `2_delphi_round2.Rmd` pie chart
  splits on `;`, which silently drops the one multi-select respondent's academia
  selection — a minor issue, but it means that figure's academia count can be one low.

## 2. Round 2 denominator (why 59, 60 and 53/52/51 all appear)

These count **different things** and are not in conflict:

| Quantity | n |
|---|---|
| Raw Qualtrics responses (2 meta rows removed) | **61** |
|  … with a sector recorded → **likely the manuscript's n** | **59** |
|  … "Finished" (Progress = 100) | 49 |
| Records in the analysis / voting file | **60** |
|  … with ≥ 1 valid domain ballot | 53 |
|  … blank ballots (a sector, but no rankings) | 7 |
| Valid ballots — Design & experience | **53** |
| Valid ballots — Response & governance | **52** |
| Valid ballots — Research & data | **51** |

**Reconciliation (not silently resolved):**
- **n = 59** most plausibly = raw responses **with a sector** (61 − 2 missing sector).
- The **analysis file has 60** rows — one *fewer* than the raw export. The missing
  record is an **Industry respondent who submitted no rankings** (raw Industry = 16,
  analysis-file Industry = 15). **Why that single blank row was removed during the
  manual cleaning is not documented in code** and can't be verified here — worth a
  one-line note in the methods, or confirming against your records.
- The STV denominators are **53 / 52 / 51** (valid ballots per domain). Seven of the
  60 records are blank ballots.

## 3. Sector sample sizes

Membership counts (a respondent counts in every sector they selected) and the
valid-ballot counts actually available per domain:

| Sector | Selecting | Single-sector | Valid: Design | Valid: Governance | Valid: Research | Interpretable? |
|---|---:|---:|---:|---:|---:|---|
| Academia | 27 | 26 | 26 | 26 | 26 | **yes** |
| Industry | 15 | 15 | 14 | 14 | 14 | **yes (but modest)** |
| Policy/gov | 6 | 6 | 4 | 3 | 3 | too small |
| Civil society | 5 | 5 | 5 | 5 | 5 | too small |
| Other | 5 | 4 | 5 | 5 | 4 | too small |
| Funder | 1 | 1 | 0 | 0 | 0 | not usable (no ballots) |

- Only **Academia** and **Industry** are large enough for even a descriptive
  comparison. Academia moves in ~3.8-point steps (1/26); **Industry moves in ~7-point
  steps (1/14)** — a shift of one or two industry respondents changes several
  percentages, so read industry numbers as *coarse*.
- **Policy/gov, Civil society, Other** (≤ 6) are reported for completeness only;
  their within-sector rankings are **not stable**. **Funder** (n = 1, no ballots) is
  unusable.
- Academia is **about half** of all valid ballots in each domain (26 of 53/52/51 ≈
  49–51%); industry ≈ 26–27%; the remaining ~23% are the small sectors.

## 4. Priority preferences — academia vs industry (descriptive)

Full tables (all sectors, first-preference % and top-5 % by domain and priority)
are in the output files. Highlights, **Academia (n ≈ 26) vs Industry (n ≈ 14)**:

**Where they agree**
- *Research & data*: near-identical. *Industry–academic data sharing* is the top
  priority for both (Academia 84.6% / Industry 85.7% top-5). *Funding*,
  *Interdisciplinary work* and *Open data* are high in both.
- *Design*: both rate *Harms & benefits* and *User trust & safety* highly.
- *Governance*: both rate *Child safety & protection* and *Data sharing &
  transparency* highly.

**Where they differ (largest gaps; industry n ≈ 14 → treat as indicative)**

| Domain | Priority | Academia | Industry | Direction |
|---|---|---:|---:|---|
| Design | **Dark patterns** (top-5) | 80.8% | 28.6% | academia ≫ industry |
| Design | Dark patterns (**first choice**) | 38.5% | **0%** | academia only |
| Design | Motivations (top-5) | 23.1% | 50.0% | industry > academia |
| Design | Social connection & community | 38.5% | 64.3% | industry > academia |
| Governance | **Self-regulation & co-reg.** (first) | 3.8% | **35.7%** | industry ≫ academia |
| Governance | Sector-wide collaboration (top-5) | 53.8% | 78.6% | industry > academia |
| Governance | **Regulation & enforcement** (first) | 23.1% | 7.1% | academia > industry |
| Governance | Dark patterns – regulatory (top-5) | 73.1% | 28.6% | academia ≫ industry |

The governance pattern is the familiar **external-regulation (academia) vs
self-regulation (industry)** contrast, and the design pattern is a
**harms/critical framing (academia) vs design-positive framing (industry)**
contrast. Both are coherent and plausible — which is a reason to report them
carefully, and a reason *not* to over-read them from n ≈ 14.

## 5. Exploratory sector-specific STV

Meaningful for **Academia (n ≈ 26)** — a reasonable exploratory run. For
**Industry (n ≈ 14)** it is **borderline**: with a Droop quota, 5 seats from ~14
ballots means each "winner" rests on ~3 first-preferences, so ordering is fragile.
Reported as a **sensitivity check only**. STV was **not** run for Policy/Civil
society/Other/Funder (too small — would produce output but not information).

| Domain | Group (n) | Elected, in order |
|---|---|---|
| Design | **All (53)** | Dark patterns › Harms › Long-term effects › User trust › Who is playing |
| Design | **Academia (26)** | Dark patterns › Harms › Long-term effects › User trust › Who is playing |
| Design | **Industry (14)** | Harms › Motivations › User trust › Long-term effects › Creativity |
| Governance | **All (52)** | Self-regulation › Child safety › Regulation & enf. › Dark patterns (reg) › Monopolisation |
| Governance | **Academia (26)** | Regulation & enf. › Dark patterns (reg) › Data sharing › CSR & labour › Child safety |
| Governance | **Industry (14)** | Self-regulation › Child safety › Sector-wide collab. › Monopolisation › Data sharing |
| Research | **All (51)** | Ind–acad data sharing › Open data › Funding › Interdisciplinary › Sector-wide |
| Research | **Academia (26)** | Ind–acad data sharing › Open data › Funding › Theory development › Qualitative |
| Research | **Industry (14)** | Ind–acad data sharing › Open data › Sector-wide › Funding › Interdisciplinary |

- **Design:** aggregate = academia **exactly (5/5)**; industry overlaps 3/5.
- **Research:** aggregate = **industry set 5/5**, academia 3/5 (academia's distinctive
  *Theory development* and *Qualitative research* don't reach the aggregate top-5).
- **Governance:** aggregate and academia overlap 3/5; aggregate's **#1 is industry's
  #1** (Self-regulation), which academia ranks low — a genuinely mixed result.

## 6. What the patterns actually show

- **Are academia and industry broadly prioritising the same things?** *Mostly yes in
  Research; partly in Design and Governance.* They share a core (data sharing,
  harms/trust, child safety) but diverge on a few salient items.
- **Clearest descriptive differences?** (1) **Dark patterns** — central for academia,
  peripheral for industry. (2) **Regulation & enforcement vs Self-regulation** in
  governance. (3) Industry weights **positive design** items (motivations, social
  connection, creativity) more.
- **Large enough to be worth reporting?** The *gaps* are large (30–50 points), but so
  is the *uncertainty*: industry n ≈ 14, minor sectors ≤ 6. They are worth reporting
  **descriptively, with N shown**, not as effects.
- **Strengthen or mislead?** *Strengthen* if framed as a transparent, caveated
  supplementary description that answers the reviewer and shows the aggregate is
  robust for Research. *Mislead* if framed as demonstrated sector effects.
- **Is the aggregate disproportionately academic?** **In Design, yes** (aggregate =
  academia exactly). **In Research and Governance, no** — Research reflects consensus
  and Governance is pulled toward self-regulation by the non-academic sectors. So the
  honest statement is *domain-specific*, not a blanket "the ranking is an academic
  artefact."

## 7. Suggested framing for the manuscript

1. Report the **agreement in Research** and the **shared core** first — it is the most
   robust finding and reassures on generalisability.
2. Present the **academia/industry contrasts** (dark patterns; regulation vs
   self-regulation) as a **descriptive supplementary table with sample sizes**, and
   state plainly that industry n ≈ 14 and minor sectors ≤ 6 preclude inference.
3. Note the **one domain (Design) where the aggregate mirrors the academic
   plurality**, as a transparency point about composition.
4. Do **not** run or report significance tests for these groups.

---

### Output files (in `data/outputs/`)

- `sector_Ns.{xlsx,csv}` — sector sample sizes (membership, single-sector, valid ballots/domain).
- `sector_first_pref_by_priority.{xlsx,csv}` — first-preference n and % by sector × domain × priority.
- `sector_top5_by_priority.{xlsx,csv}` — top-5 inclusion n and % by sector × domain × priority.
- `sector_rank_distribution.csv` — n ranked, mean and median rank by sector × domain × priority.
- `academia_vs_industry_supp.xlsx` / `academia_vs_industry_top5.csv` — compact supplementary comparison.
