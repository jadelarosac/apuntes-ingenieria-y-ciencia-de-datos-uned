# Transform `md-material` into Obsidian notes

Rules and objectives for turning the slide dumps in `mineria-de-textos/.agents/md-material/` (`Tema_1_Introduccion_PLN.md`, `Tema_2_Extraccion_Informacion.md`, ...) into Obsidian-friendly notes. Reference style: `modelado-estadistico-de-datos/`.

- [x] **Objective: synthesis, not transcription**
  - [x] Write a summary of each topic, not a slide-by-slide copy of it
  - [x] Keep only what matters: definitions, key ideas, algorithms, formulas, metrics, trade-offs and concrete examples
  - [x] Drop repetition, filler, slide titles repeated across slides, "Conceptos preliminares"-style headers and page numbers
  - [x] Merge slides that cover the same idea (e.g. the many "Evaluación de tareas de PLN" or "Extracción de relaciones" slides) into one short section
  - [x] Keep concrete names: models, algorithms, corpora, datasets, tools, libraries, functions, metrics (e.g. Lesk, Porter/Snowball, CoNLL-2002/2003, MUC-7, ADE, BIO, TimeML/TIMEX3, `nltk.ne_chunk`, `es_core_news_sm`)
  - [x] When in doubt about whether something is important, keep it in one line rather than a paragraph

- [x] **Objective: one note per theme, not per "Tema"**
  - [x] Never produce `tema-1.md`, `tema-2.md`, `topic-1.md`...; split and name by subject (`topic.md`, `another-topic.md`)
  - [x] Each note is thematically self-contained: it can be read alone without the slides
  - [x] Each note is as short as possible while staying self-contained; if it grows long, split it into two themes and link them
  - [x] A theme that appears in several "Temas" (e.g. evaluation, nltk/spaCy usage) goes in a single note that collects all of it
  - [x] Final split (adjusted after reading the material):
    - [x] `mineria-de-textos.md`: overview / entry point (what text mining is, main tasks, corpora and annotations) with links to every other note
    - [x] `corpus.md`: corpus types, example corpora (Brown, SUSANNE, Europarl, ADE, BioScope, DDI, DIANN...) and evaluation campaigns
    - [x] `normalizacion-de-textos.md`: regular expressions, word normalization, word and sentence segmentation (tokenization), stop words, edit distance
    - [x] `stemming-y-lematizacion.md`: morphological analysis, stemming (Porter, Lancaster, Snowball), lemmatization
    - [x] `pos-tagging.md`: tagsets, taggers, baseline
    - [x] `analisis-sintactico.md`: constituents, dependencies (projective parsing), chunking
    - [x] `desambiguacion-del-sentido.md`: WSD, dictionary-based methods, Lesk algorithm, WordNet
    - [x] `evaluacion-pln.md`: precision, recall, F1, accuracy, baseline, error analysis, evaluation specific to NER/IE
    - [x] `extraccion-de-informacion.md`: tasks and subtasks, template filling, main techniques
    - [x] `reconocimiento-de-entidades.md`: NER, class encoding (BIO), rules, dictionaries, regex, ML features, deep learning, Transformers
    - [x] `extraccion-de-relaciones.md`: lexico-syntactic patterns, supervised, semi-supervised and unsupervised extraction
    - [x] `expresiones-temporales-y-sucesos.md`: temporal expression extraction, temporal normalization, event extraction
    - [x] `nltk-spacy.md`: tool reference (NLTK resources, spaCy objects, pipeline and token attributes); task code lives in each topic note

- [x] **Objective: cite sources**
  - [x] Every concrete model, algorithm, book, paper, corpus, dataset or tool gets a source, with a link whenever possible
  - [x] Prefer stable links: ACL Anthology, DOI, arXiv, official docs (nltk.org, spacy.io, huggingface.co), author pages
  - [x] Reuse the links already present in the material (e.g. `https://aclanthology.org/W02-2024/`, `https://aclanthology.org/W03-0419/`, `https://www.nltk.org/book/`, `https://web.stanford.edu/~jurafsky/slp3/`, `https://spacy.io/api/token`)
  - [x] Add the original paper when the slides only name a method (e.g. Lesk 1986 for WSD, Porter 1980 for stemming, Hearst 1992 for lexico-syntactic patterns, Ramshaw & Marcus 1995 for BIO chunking, Lample et al. 2016 for BiLSTM-CRF, Devlin et al. 2019 for BERT)
  - [x] Verify every link and reference; do not invent URLs, authors or years. If a link cannot be confirmed, cite without a link
  - [x] Cite inline where the concept appears (e.g. `algoritmo de Lesk ([Lesk, 1986](https://doi.org/10.1145/318723.318728))`) and collect them in a final `## Referencias` section
  - [x] Point to the relevant chapter of the course bibliography: Jurafsky & Martin, _Speech and Language Processing_ (3rd ed. draft) and Bird, Klein & Loper, _Natural Language Processing with Python_

- [x] **Rule: Obsidian format (match `modelado-estadistico-de-datos`)**
  - [x] Write the notes in Spanish
  - [x] File names in kebab-case Spanish, no accents or "tema" prefixes
  - [x] Place notes directly in `mineria-de-textos/` (flat, like `modelado-estadistico-de-datos/`)
  - [x] One `#` H1 title per note, then `##` / `###` sections; headings are short so they work as link anchors
  - [x] English technical terms in italics next to the Spanish one: `reconocimiento de entidades (_Named Entity Recognition_, NER)`
  - [x] Math in LaTeX: `$...$` inline and `$$ ... $$` blocks (e.g. precision, recall, F1, edit distance)
  - [x] Link related notes with wikilinks, including heading links: `[[evaluacion-pln#F1]]`, `[[normalizacion-de-textos#Lematización]]`
  - [x] Link instead of repeating: if a concept is explained in another note, link it and give at most one line of context
  - [x] Use bullet lists for enumerations and short comparison tables only when they are clearer than prose
  - [x] No YAML frontmatter (the example notes have none)

- [x] **Rule: clean the PDF extraction artefacts**
  - [x] Rebuild sentences broken into fake tables (`| La | extracción | ...`) into normal prose
  - [x] Remove page numbers, course headers ("Máster Universitario en Tecnologías del Lenguaje"), stray bullets (`•`, `●`, `–`)
  - [x] Fix typos from the source (e.g. "morfolológico", "Pryectivo", "imprimer")
  - [x] Only keep real tables (e.g. confusion matrices, tag sets, NER class examples) and rebuild them correctly

- [x] **Rule: code**
  - [x] Keep code only when it shows how to do something concrete (minimal snippet per idea)
  - [x] Use fenced blocks with language (` ```python `), reassembled from the broken table cells
  - [x] Keep the needed `nltk.download(...)` calls and spaCy model names
  - [x] Check the code is valid Python and uses the current API names

- [x] **Done criteria**
  - [x] All important content of every `Tema_*.md` is covered by some note (nothing relevant lost)
  - [x] No note is a 1:1 copy of the slides; each one is noticeably shorter than its source content
  - [x] Every note is reachable from `mineria-de-textos.md` and has at least one wikilink to a related note
  - [x] All wikilinks resolve to existing notes/headings
  - [x] Every concrete model, paper, book, corpus and tool mentioned has a source
  - [x] No PDF artefacts remain
