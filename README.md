# Lexical Dictionaries of Nativism — Canada (FR / EN)

*[Version française](README.fr.md)*

This repository contains two regular-expression dictionaries (French and English) used to identify **nativist sentences** in the electoral manifestos of Canadian provincial political parties. They were built for the master's thesis *Le nativisme comme réponse au populisme : proposition conceptuelle et théorique mise à l'épreuve en contexte canadien* (Nativism as a response to populism: a conceptual and theoretical proposal tested in the Canadian context), UQAM, Sociology, 2026.

---

## 1. What the dictionaries measure

### Definition of nativism

The dictionaries operationalize the definition of nativism proposed in the thesis:

> Nativism is the perception that a society's cultural diversity is too great and that the state should adopt policies to tighten the culture around a specific national myth.

On this view, a society's cultural diversity sits on a **spectrum running from nativism to multiculturalism**. Each pole corresponds to a movement that pushes society toward its own side: nativism toward convergence around a national myth, multiculturalism toward recognizing and maintaining plurality.

What places a position on the nativist side is **the demand for cultural homogeneity around what is held to make the nation distinct from other nations**. The content of that distinctiveness varies (language, religion, history, even liberal values when they are presented as what sets the nation apart). The criterion is the definition of the nation, not the content of the culture. That definition is itself debated among the members of the political community; the dictionaries measure the positions taken in that debate, not its outcome.

The definition is descriptive. It sets no threshold of "acceptable" diversity. It measures where political actors stand on the spectrum; what is desirable is left to democratic debate.

A sentence is coded as nativist when it expresses one of these ideas: diversity is excessive or threatening; the national culture must be defended or asserted; or the state should act to reduce diversity, whether through cultural, linguistic or religious policy or through immigration control.

<!-- TO COMPLETE: thesis section or page where the definition is developed. -->

### Dimensions (indicative only)

Each pattern belongs to one dimension (column `category`). **The dimensions only organize the patterns and were not used in the thesis analysis.** Only the overall score matters (see section 4).

| Dimension | Content |
|---|---|
| `cultural_dim` | Attachment to national culture, identity, tradition and history; hostility toward "enemies" of the nation; immigration framed as overwhelming; patriotism; great replacement; deportation |
| `political_rights_dim` | Restricting immigration, caps, status revocation, migrants framed as "illegal", refusal to integrate, dominant national culture, self-segregation |
| `economic_dim` | Immigrants "taking jobs", fiscal burden, national preference ("X first") |
| `religious_dim` | Islam linked to immigration or danger, veils, sharia, fundamentalism, ritual slaughter, etc. |
| `law_and_order_dim` | Foreign or ethnic groups linked to crime; parallel legal orders |

---

## 2. Important caveat: these dictionaries are not universal

**The dictionaries must be adapted to the corpus they are applied to.** They do not work universally.

A lexical approach detects words and word combinations, not meaning. The same term can be nativist in one context and harmless in another ("heritage" in "our national heritage" vs. "heritage fund"; "tradition" in identity discourse vs. "traditional industries"). To produce valid results, each pattern was tuned to the study corpus in order to:

- **reduce false positives** (sentences flagged wrongly), through exclusions (*vetoes*) such as references to First Nations, the arts and culture sector, natural heritage or sustainable development;
- **reduce false negatives** (nativist sentences missed), by broadening terms or adding patterns specific to the debates found in the corpus.

Applied as-is to another corpus (another country, another period, parliamentary debates, social media, news), the dictionaries will almost certainly produce errors in both directions. Any reuse should include a **fresh manual validation** on a sample of the new corpus (see section 5).

### Original corpus

The dictionaries were tuned to the **manifestos of all provincial political parties, in every province, in the elections preceding the Canadian federal election of October 21, 2019**. That federal election corresponds to the 2019 Canadian Election Study (CES) used in the thesis.

The manifestos come from the **Poltext** political document collection of the Center for Public Policy Analysis (CAPP) at Université Laval (https://www.poltext.org/). The manifestos themselves are not redistributed in this repository.

### The French and English dictionaries are not translations of each other

The two dictionaries were built separately and reflect the debates of each linguistic context. The French dictionary, applied mainly to Quebec manifestos, includes patterns tied to the French language, francization, integration, secularism (*laïcité*) and reasonable accommodation, with no direct English equivalent. The English dictionary contains patterns specific to English Canada ("Keep Alberta strong", references to "the West").

**Scores produced by the two dictionaries are therefore not directly comparable** without cross-validation.

---

## 3. Repository contents

| File | Description |
|---|---|
| `nativism_dict_FR_regex.csv` | French dictionary: `category`, `Regex`, `Concept` |
| `nativism_dict_ENG_regex.csv` | English dictionary: `category`, `Regex`, `Concept`, `Rule` (plain-language description of each pattern and its exclusions) |

Notes:

- In the French file, some rows have an empty `category`. Since dimensions are not used to compute the score, these rows count toward the overall score like any other.
- Patterns use inline flags (`(?i)` case-insensitive, `(?x)` extended mode), positive and negative lookaheads, and word boundaries (`\b`). <!-- TO COMPLETE: regex engine used (e.g. stringr / ICU in R, or PCRE with perl = TRUE). -->

---

## 4. Usage and scoring

### Unit of analysis: the sentence

Many patterns require two terms to appear "in the same sentence", so text must be split into sentences before the dictionaries are applied. Habersack and Werner (2023) applied their dictionary to MARPOR (Manifesto Project) quasi-sentences; this repository uses sentences. <!-- TO COMPLETE: segmentation method. -->

### Score

- A sentence matched by at least one pattern counts as **one point only**. If several patterns match the same sentence, no further points are added.
- A political actor's score is the **percentage of nativist sentences** out of all sentences in the corpus associated with that actor:

  *score = (number of nativist sentences / total number of sentences) × 100*

### Corpus comparability

The score is a proportion. It is comparable across actors only if their corpora are equivalent:

- **One document per actor** (one manifesto per party, for example) works, provided the documents are of comparable length.
- **Short documents bias the results**: a handful of matched sentences is enough to produce a high percentage, or zero. It may be preferable to exclude very short manifestos or to report their length.
- **For large or heterogeneous corpora** (social media posts, for example), draw a random sample of equal size for each actor.

### Minimal example in R

```r
library(readr)
library(stringr)
library(purrr)

dict <- read_csv("nativism_dict_ENG_regex.csv")

sentences <- c(
  "We will protect our heritage and our way of life.",
  "The Heritage Fund will support local museums.",
  "We will reduce hospital wait times."
)

# One logical column per pattern
hits <- map(dict$Regex, ~ str_detect(sentences, regex(.x)))
hits <- do.call(cbind, hits)

# A sentence is nativist if at least one pattern matches (one point per sentence)
is_nativist <- rowSums(hits) > 0

# Score: percentage of nativist sentences
score <- mean(is_nativist) * 100
```

---

## 5. Construction and validation

- **Starting point.** The dictionaries adapt the nativism dictionary of Habersack and Werner (2023), designed for Austrian, German and Swiss manifestos, to the Canadian context and to French.
- **Procedure.** Patterns were refined iteratively over about two months. Each version was applied to the corpus, flagged and unflagged sentences were read, and patterns were corrected until no false positives or missed nativist sentences remained in the corpus.
- **Limitation.** Validation was carried out on the same corpus used to build the dictionaries. Measured performance is therefore *in-sample* and likely overstates performance on new texts. For any reuse, we recommend hand-coding a random sample of the new corpus and reporting precision and recall.

---

## 6. Limitations

- **Negation, irony and reported speech.** A sentence that reports or denounces nativist discourse ("we reject great-replacement rhetoric") may be flagged. Some exclusions address this, but not all.
- **Classification choices.** Some patterns, especially in French (French language, francization, social cohesion, shared values), rest on the view that asserting a common national culture falls on the nativist side of the spectrum. This follows from the thesis definition. Researchers who separate civic nationalism, interculturalism and nativism should remove or isolate these patterns.
- **Indigenous peoples.** Several patterns exclude references to First Nations, Métis and Inuit. There, the vocabulary of tradition, belonging and territory carries a different meaning that is not nativism in the sense defined here.
- **Sensitive content.** By design, the dictionaries contain offensive or stigmatizing terms. Their presence does not reflect the author's views.

---

## 7. Citation

If you use these dictionaries, please cite the thesis and the article they build on:

> <!-- TO COMPLETE: Last name, First name --> (2026). *Le nativisme comme réponse au populisme : proposition conceptuelle et théorique mise à l'épreuve en contexte canadien*. Master's thesis, Université du Québec à Montréal. <!-- Archipel link -->

> Habersack, F., & Werner, A. (2023). How non-radical right parties strategically use nativist language: evidence from an automated content analysis of Austrian, German, and Swiss election manifestos. *Party Politics*, 29(5), 865–877. https://doi.org/10.1177/13540688221103930

Corpus source: Poltext, Center for Public Policy Analysis (CAPP), Université Laval. https://www.poltext.org/

---

## License

<!-- TO COMPLETE: e.g. CC BY 4.0 for the dictionaries. -->

## Contact

<!-- TO COMPLETE -->
