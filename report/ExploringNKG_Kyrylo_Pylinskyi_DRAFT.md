# Exploring Representations of Narratives in Knowledge Graphs

**Kyrylo Pylinskyi**
Jagiellonian University, Kraków · Project: Exploring Representations of Narratives in Knowledge Graphs
Working draft (notes and citations), November 2026

> **Note on conventions.** This is a working draft consisting of notes and citations. Citations are numbered and link to the reference list. Page numbers refer to the published pagination of each source, except for [1], where they refer to the pages of the manuscript. Text in quotation marks is quoted verbatim; all other notes are paraphrases. **[To be completed]** marks parts still to be written.

**Keywords:** Narratives · Knowledge Graphs · Ontologies · Narrative Knowledge Graphs · Perspectives · Large Language Models

---

**Abstract.**

> **[To be completed]**

---

## 1. Introduction

### 1.1 Motivation

- Two kinds of truth: "the truth of what actually happened (verifiable, recordable, retrievable) and the truth of what something meant, how it was experienced, the narrative one builds around it" [[1](#ref-lippolis), p. 1].
- This maps onto the cognitive-science distinction between *semantic memory* (general, factual, decontextualised) and *episodic memory* (autobiographical, situated, narratively organised) [[1](#ref-lippolis), p. 2].
- Existing knowledge infrastructures aim at factual truth and are "structurally blind to the perspectival, narrative, metaphorical texture through which humans actually construct and communicate meaning" [[1](#ref-lippolis), p. 2].
- LLMs absorb this texture implicitly, but "the LLM has no explicit representation of the perspective it is adopting, the community whose assumptions it is deploying, or the narrative frame through which it is organizing information" [[1](#ref-lippolis), p. 3].
- KGs are proposed as a corrective, an explicit, human-readable layer of factual grounding for LLM outputs, but standard KGs are designed for factual truth only [[1](#ref-lippolis), p. 3].
- Narratives are essential to human sense-making; systems able to construct them would help build truly "human-centric" systems [[2](#ref-blin), p. 133].
- Yet "few efforts have been made to identify core elements common to each work such as events and agents, and to unify existing representations" [[2](#ref-blin), p. 134].
- Pre-trained language models show strong text generation and question answering, but have "inherent limitations in knowledge coverage", are vulnerable to societal biases, and struggle with semantic coherence and long-range dependencies [[3](#ref-dekok), pp. 29–30].
- KGs are suitable structures for human knowledge, machine-readable and adaptable, and text generation from KGs is well studied [[3](#ref-dekok), p. 30].

### 1.2 Project Context: The Human–LLM–NKG Triangle

- A human interacts with an LLM mediated by a Narrative Knowledge Graph (NKG) [[1](#ref-lippolis), pp. 3, 6].
- This report is the first step of the project: understanding how narratives can be represented in KGs, as the basis for implementing a Human–LLM–NKG interaction system.

### 1.3 Research Questions

- **RQ1.** How do existing Semantic Web models represent narratives?
- **RQ2.** How can these models be classified, and what are their advantages and disadvantages?
- **RQ3.** What does an NKG add over a standard KG in capabilities and applications?
- **RQ4.** How do these representations interact with LLMs, and which of them can serve as the NKG layer of a Human–LLM–NKG interaction system?

---

## 2. Background

### 2.1 Knowledge Graphs

- **[To be completed]** Definition (Hogan et al. 2021), RDF triples, OWL ontologies vs instance data, SPARQL.
- *Narrative graph:* a KG in which an ontology is populated with data to represent a narrative [[2](#ref-blin), p. 134]. Its benefit is the semantic layer, which allows finer-grained representations and better querying and reasoning.
- Debate continues at the syntactic level about the best KG data model: simple RDF, named graphs or RDF-star [[2](#ref-blin), p. 134].
- De Kok et al. describe a narrative graph by its two components: (1) each event with its *4W* (who, what, when, where), and (2) the links between events through temporal and causal relations [[3](#ref-dekok), p. 30, after Blin].

Knowledge-representation (KR) formalisms, ordered by complexity [[2](#ref-blin), p. 137]:

- **basic:** only binary relations, without variables;
- **n-ary:** relations involving more than two entities;
- **variables:** placeholders replaced by actual values or entities;
- **higher-order:** functions or predicates applied to other functions or predicates;
- **temporal:** relations involving time;
- **epistemic:** knowledge and beliefs about knowledge;
- **causal:** cause-and-effect relations between entities or events;
- **rules:** logic-based statements defining conditions and implications.

**Table 1.** KG, query and rule constructs and the KR formalisms they support. Adapted from [[2](#ref-blin), Table 3].

| Construct | Description | Formalisms supported |
|---|---|---|
| Simple KG (RDF) | plain (s, p, o) triples | basic, temporal, causal |
| Blank node | nodes without an IRI | n-ary, variables |
| Singleton property | triples annotated through a unique property | n-ary |
| Reification | statement nodes | higher-order, temporal, epistemic |
| Named graph | grouped triples | higher-order |
| Labelled property graph | key-value pairs on nodes and edges | higher-order, temporal, epistemic |
| Hyper-relational graph (RDF-star) | `<<s p1 o1>> p2 o2` | higher-order, temporal, epistemic |
| Query | — | variables |
| Rule | — | temporal, epistemic, rules |

- Trade-offs: reification is RDF-compliant but can be inefficient; named graphs are compact but can become overloaded with identifiers; RDF-star is more efficient but not yet widely adopted [[2](#ref-blin), p. 146].

### 2.2 Narratives

- A recurring simplification defines a narrative as a sequence of events [[2](#ref-blin), p. 134].
- In history and cognitive science, the main elements form an extended version of the *five Ws*: Who, What, Where, When and Why [[2](#ref-blin), p. 135].
- Narrative as "the primary operation through which temporal human experience becomes intelligible as identity, causality, and change over time" (after Ricoeur) [[1](#ref-lippolis), p. 5].

Narrative requirements identified from interviews [[2](#ref-blin), pp. 135–137]:

- **Agents:** animate entities such as humans or animals.
  - *Types* (human, animal, animate object; sometimes an active agent acting with a goal vs a passive spectator), *properties* (e.g. physical characteristics), *roles* in events.
- **Objects:** physical or abstract things that are not agents, with *types*, *properties* and *roles* in events.
- **Locations:** places or settings where events happen, plus *relations* between locations.
- **Provenance:** the origin of the data or statements, including the original *narrative text*.
- **Events and actions:** the core element of every narrative in the interviews.
  - An *action* is performed by an agent and can have pre- and post-conditions; an *event* simply happens.
  - *Types*, *properties* (timestamps, locations) and *relations*: mereological (part–whole), temporal (e.g. Allen's interval algebra), causal.
- **Narrative dynamics:** higher-level elements that bind, influence and drive the progression of the narrative.
  - *Script:* a pattern of related event sequences (e.g. the restaurant scene); includes archetypes, plans, frames, processes.
  - *Storyline:* a coherent sequence of events; an instantiation of a script (scenes, acts, episodes, situations).
  - *State:* the condition of someone or something at a given time or interval.
  - *Goal:* a state desired by an agent.
  - *Outcome/effect:* a state resulting from a causal relation; the new condition after an event.
  - *Perspective:* different interpretations of the same event.
  - *Conditions:* when events or actions can happen (pre-conditions, post-conditions, enablement, etc.).

**Table 2.** Frequency of narrative requirements across the six interviews and the KR formalisms they require. Adapted from [[2](#ref-blin), Table 2].

| Requirement | Interviews (of 6) | KR formalisms required |
|---|---|---|
| Agents | 6 | basic, n-ary |
| Objects | 2 | basic, n-ary |
| Locations | 4 | basic, n-ary |
| Provenance | 3 | higher-order |
| Events / actions | 6 | basic, n-ary, temporal, causal |
| Narrative dynamics | 6 | variables, higher-order, epistemic, rules |
| — script | 4 | variables, rules |
| — storyline | 1 | higher-order |
| — state | 3 | higher-order |
| — goal | 2 | higher-order, epistemic |
| — outcome/effect | 1 | higher-order, epistemic |
| — perspective | 3 | higher-order, epistemic |
| — conditions | 3 | variables, rules |

- Key finding: objects, locations and agents need only basic or n-ary representations, while events and especially narrative dynamics need more advanced formalisms [[2](#ref-blin), p. 137].
- Perspective, the core of an NKG, needs higher-order and epistemic representation, which plain triples cannot provide (cf. Table 1).

### 2.3 Hermeneutics: Situatedness, Plurality, Ambiguity

- Meaning is always constructed by an interpreter, from a particular standpoint, in dialogue with a text, artefact or cultural object [[1](#ref-lippolis), p. 4].
- *Fusion of horizons* (Gadamer): "the encounter between an interpreter's existing framework and a new text or situation, producing meaning that neither party 'had' beforehand" [[1](#ref-lippolis), p. 3].
- **Situatedness.** All interpretation happens from a specific standpoint that shapes what can be understood and how. "A knowledge representation system that does not encode situatedness implicitly claims to offer one, and is epistemically dishonest" [[1](#ref-lippolis), p. 4].
- **Perspectival plurality.** Multiple valid, mutually incompatible interpretations of the same phenomenon can coexist; the space of valid interpretations is not a singleton. Representations that collapse this plurality into one ground truth are systematically distorting [[1](#ref-lippolis), p. 4]. This is not relativism: not all perspectives are equally well evidenced.
- **Ambiguity.** The irreducible openness of meaning that persists even within one situated perspective, allowing multiple coherent but non-equivalent readings [[1](#ref-lippolis), p. 4].
- **Metaphor** is a central instance of the narrative layer: people framing the same illness as a *battle* or as a *dysfunction to be managed* operate "within structurally different narrative worlds, with different implications for agency, outcome, and meaning" [[1](#ref-lippolis), pp. 2, 5].
- **[To be completed]** Link to Kommers et al. (LLMs as "context machines") [[7](#ref-kommers), p. 4] and to Gangemi and Presutti's definition of perspectivization [[6](#ref-gangemi), pp. 211–213].

---

## 3. Methodology

### 3.1 Method of the Reference Study (Blin et al.)

- **Interviews:** 6 open interviews with 9 participants from the MUHAI EU project, about 20 min each, in six domains (Table 3) [[2](#ref-blin), pp. 134–136].

**Table 3.** Interview domains, narrative examples and application scenarios. Adapted from [[2](#ref-blin), Table 1].

| Id | Domain | Narrative example | Application |
|---|---|---|---|
| I1 | History | Main happenings of the French Revolution | Question answering on historical events |
| I2 | Biomedical science | PICO claims (Population, Intervention, Comparison, Outcomes) | Reasoning and decision support |
| I3 | Social media analysis | Twitter claims about the Russo-Ukrainian conflict | Insights from user-generated content |
| I4 | Robotic kitchen | Robot executing a recipe | Cooking task execution |
| I5 | Arts and literature | Questions raised when reading a text or painting | Interpretive question answering |
| I6 | Cognitive science | Importance of values in beliefs | Modelling belief systems |

- Interview questions: (1) which elements are needed to represent your narratives and why; (2) which data structure is needed; (3) which tasks the narratives are used for; (4) one example narrative [[2](#ref-blin), p. 135].
- **Systematic review:** query "narrative OR event-centric" on titles and abstracts in 7 venues (SWJ, JWS, ISWC, ESWC, EKAW, K-CAP, KGSWC) plus snowballing. 708 + 27 papers → 735 screened → 61 included (45 ontologies, 15 resource KGs, 1 both) [[2](#ref-blin), pp. 139–140].
- **Quality metric:** a 5-star vocabulary rating, extending Berners-Lee's Linked Data stars [[2](#ref-blin), p. 143]:
  - 0★ Linked Data without a vocabulary;
  - 1★ dereferenceable human-readable information about the vocabulary;
  - 2★ machine-readable explicit axiomatisation;
  - 3★ the vocabulary is linked to other vocabularies;
  - 4★ metadata about the vocabulary is available;
  - 5★ the vocabulary is linked to by other vocabularies.
- **Limitations:** all interviewees came from one project (MUHAI), the narratives were text- or sensor-based only, a checkmark in the coverage table says nothing about how many relations an ontology offers, and the 5-star rating is the only quality metric [[2](#ref-blin), pp. 135, 145, 147–148].

### 3.2 Method of This Report

- Model selection: start from Blin's retained ontologies (Table 5) and add the perspective and NKG models (Shoilee et al. [[5](#ref-shoilee)], Gangemi and Presutti [[6](#ref-gangemi)], Lippolis and Do Valle Miranda [[1](#ref-lippolis)]).
- Comparison dimensions:
  - narrative requirements covered (Table 2);
  - syntactic mechanism used (Table 1);
  - 5-star vocabulary rating;
  - support for perspective, ambiguity and metaphor (the three hermeneutic dimensions);
  - tooling, adoption, query complexity.
- Follow Blin's five-step selection procedure [[2](#ref-blin), pp. 145–146]:
  1. identify the relevant narrative requirements (Table 2);
  2. derive the KR formalisms they need (Tables 1 and 2);
  3. choose ontologies that cover them (Table 5), preferring high 5-star scores;
  4. extend the best-fitting ontology, keeping to web standards;
  5. build a new ontology only if existing ones are insufficient.
- Blin et al. also advise targeting ontologies that "precisely represent these requirements rather than selecting the most comprehensive ones" [[2](#ref-blin), p. 147].
- **[To be completed]** Running example story (e.g. Benin 1897 object provenance, or an illness narrative).
- **[To be completed]** Competency questions answered with SPARQL (template: [[5](#ref-shoilee), p. 133, Table 2]).

---

## 4. Taxonomy of Narrative Representations

**Table 4.** Proposed levels of narrative representation in knowledge graphs.

| Level | Category | Example models | Main source |
|---|---|---|---|
| L0 | Factual / encyclopaedic KGs | Wikidata, DBpedia, YAGO | [[3](#ref-dekok)] |
| L1 | Event-centric KGs and ontologies | EventKG, SEM, DOLCE/DUL, CIDOC-CRM, LODE | [[2](#ref-blin)] |
| L2 | Narrative graphs (events + temporal/causal links) | FARO, enriched ASRAEL KG | [[3](#ref-dekok)] |
| L3 | Narrative-centric ontologies | Narrative Ontology, ProppOntology, Drammar | [[2](#ref-blin)] |
| L4 | Perspectival / polyvocal KGs | SEM `sem:View`, EDM proxies, Shoilee pattern, CP ontology | [[4](#ref-vanerp)], [[5](#ref-shoilee)], [[6](#ref-gangemi)] |
| L5 | Narrative Knowledge Graphs | NKG (perspective + narrative + metaphor) | [[1](#ref-lippolis)] |

- **[To be completed]** Second, syntactic axis and a figure placing every model on both axes.
- Relation to Blin's classification [[2](#ref-blin), pp. 139–142]. Blin et al. sort ontologies into:
  - **1a** top-level event-centric (16): EO, DUL, DOLCE, CIDOC-CRM, EC, SEM, LODE, Enslaved, ABC, EDM, VLX-Stories, E, SEO, FARO, ProppOntology, BBO;
  - **1b** event-centric, extended from 1a: e.g. SOSA, F, EPFDO (from DUL); COMM, DOLCE-LITE (from DOLCE); FRBR, Jazz Ontology;
  - **2a** narrative-centric (7): RST storytelling, Mediation, Princess Lovely (Emergent\*), StoryTeller (BKOnto), ODY-ONT, DL Ontology for Fairy Tale (CRBOnto), Transmedia Fictional Worlds;
  - **2b** narrative-centric, extended: Drammar (from DOLCE-LITE), AO/Labyrinth\*, FRBRoo, ArCo, StoryFountain, RSO (from CIDOC-CRM), Narrative Ontology (from CIDOC-CRM and EC);
  - **3** media-centric (excluded): OntoMedia, SsVM, Eventory, DISC, VERL, RDF-Events.
- Their comparison keeps only 1a and 2a. Levels L1–L3 here refine that split; L4 and L5 cover perspective models that their review does not treat as a category.
- L0/L1 example, *EventKG*: links events from sources such as Wikidata and YAGO; over 1.3 million events with spatial and temporal coordinates. Limited to sub-/super-events and the 4W, which gives "succinct and somewhat incomplete narratives" [[3](#ref-dekok), p. 30].
- L2 example, the *enriched ASRAEL KG*: event articles and their 4W relations, enriched with cause/enable/prevent relations and event coreference, modelled with NIF, SEM and FARO [[3](#ref-dekok), pp. 31–32].
- Resource KGs built on these ontologies [[2](#ref-blin), p. 141]: EventKG, DIVE, ECG (SEM); Wind in Our Sails, BiographySampo, ArCo, SCULPTEUR (CIDOC-CRM); EventMedia (CIDOC-CRM + LODE); OceanGraph (SOSA/DUL); 3cixty (DUL + Schema.org); History of Vienna (Schema.org); VLX-Stories; Structured Event Entity Resolution (EO).
- The Semantic Web already has building blocks for events, agents, locations and some inter-event relations, but "perspective, goal, outcome, conflict, and evolving interpretation—remain only partially modeled and unevenly standardized" [[1](#ref-lippolis), p. 4].

---

## 5. Model-by-Model Analysis

Template for each model: purpose · core classes and properties · running-example snippet (Turtle) · requirements covered · advantages · disadvantages.

### 5.1 Simple Event Model (SEM)

- SEM provides a foundation for modelling events, but on its own cannot link disparate events or classes of the same type; Blin proposes adding Allen temporal links, causal links and `dbo:alongside` links [[3](#ref-dekok), p. 30].
- In Blin et al.: ★★★★, 7 requirements, event-centric, generic. Can represent perspective through `sem:View` and blank nodes; enough for narratives with simple requirements [[2](#ref-blin), pp. 143, 145]. Recommended for perspectives in social media analysis (I3) [[2](#ref-blin), Table 8].[^sem]
- **[To be completed]** Analysis following the template.

### 5.2 DOLCE / DUL

- In Blin et al.: ★★★★★, 21 requirements each (the broadest), event-centric, upper-level. Cost: complexity and readability; users must define their own patterns, e.g. for perspectives [[2](#ref-blin), pp. 143–144].[^dolce]
- **[To be completed]** Analysis following the template.

### 5.3 CIDOC-CRM

- In Blin et al.: ★★★★★, 11 requirements, event-centric, domain-specific (cultural heritage); basis for many 1b/2b ontologies [[2](#ref-blin), pp. 139–144].[^cidoc]
- **[To be completed]** Analysis following the template.

### 5.4 FARO

- Covers most event relations in the literature, from temporal to causal and finer-grained ones: prevention, enabling, causality and intention [[3](#ref-dekok), p. 30]. A FARO dataset of annotated sentences exists, e.g. "The government has implemented a series of laws to prevent the abuse of animals" → (*laws*, *prevent*, *abuse*) [[3](#ref-dekok), p. 31, Table 1].
- In Blin et al.: ★★★★, 5 requirements, event-centric, generic. Covers a wide range of event relations, including goals, states and conditions [[2](#ref-blin), pp. 145–146].[^faro]
- **[To be completed]** Analysis following the template.

### 5.5 Narrative Ontology (Meghini et al.)

- Category 2b in Blin et al. (extended from CIDOC-CRM and EC) [[2](#ref-blin), p. 142]; not scored in their coverage table, so it needs its own evaluation here.[^no]
- **[To be completed]** Analysis following the template.

### 5.6 Polyvocal Pattern: CIDOC-CRM + PROV-O + Named Graphs (Shoilee et al.)

- **[To be completed]** Analysis following the template [[5](#ref-shoilee)].[^shoilee]

### 5.7 Cognitive Perspectivization Ontology (Gangemi and Presutti)

- **[To be completed]** Analysis following the template [[6](#ref-gangemi)].[^cp]

### 5.8 Narrative Knowledge Graphs (Lippolis and Do Valle Miranda)

- **Goal:** represent knowledge along all three hermeneutic dimensions: "structured enough to be formally manipulable, rich enough to hold perspectival and ambiguous content, and explicitly connected to the individuals and communities from whose standpoint the knowledge is valid" [[1](#ref-lippolis), p. 5].
- **Foundation:** standard RDF/OWL, extended along three axes [[1](#ref-lippolis), p. 5]:
  1. *Perspective-annotated named graphs* (operationalise situatedness). Every assertion is anchored to a perspective node: community, epistemic tradition, temporal and cultural context, evidential basis. An NKG answers "from whose standpoint is this true?" instead of "when is this true?" [[1](#ref-lippolis), p. 5]. Uses RDF 1.2 triple terms; compatible with PROV-O and nanopublications.
  2. *Narrative ontologies* (operationalise the temporal, sequential side of experience). The same sequence of events yields different narratives depending on who tells it [[1](#ref-lippolis), pp. 5–6]. Builds on Ciotti and on Meghini's Narrative Ontology.
  3. *Metaphorical frames* (operationalise ambiguity). Represented as named-graph connectors from experiential source domains to abstract target concepts, grounded in FrameNet / Framester / Amnestic Forgery. Each mapping "activates a distinct frame structure with its own role slots, entailments, and inferential consequences, making divergent framings formally distinguishable" [[1](#ref-lippolis), p. 6].
- **Advantages:** **[To be completed]**
- **Disadvantages / open questions:** vision paper with no implementation yet; **[To be completed]**[^nkg]

---

## 6. Comparative Analysis

### 6.1 Coverage of Narrative Requirements

**Table 5.** Five-star rating, number of narrative requirements covered (out of 23), event-centric (E) or narrative-centric (N), and domain specificity. Adapted from [[2](#ref-blin), Table 7].

| Stars | Ontology | Requirements covered | E / N | Domain-specific |
|---|---|---|---|---|
| ★★★★★ | DOLCE | 21 | E | |
| ★★★★★ | DUL | 21 | E | |
| ★★★★★ | CIDOC-CRM | 11 | E | ✓ |
| ★★★★★ | EDM | 11 | E | ✓ |
| ★★★★★ | EO | 5 | E | |
| ★★★★★ | EC | 2 | E | |
| ★★★★ | ProppOntology | 13 | E | ✓ |
| ★★★★ | Enslaved | 10 | E | ✓ |
| ★★★★ | SEO | 9 | E | ✓ |
| ★★★★ | SEM | 7 | E | |
| ★★★★ | Transmedia\* | 7 | N | ✓ |
| ★★★★ | FARO | 5 | E | |
| ★★★★ | LODE | 5 | E | |
| ★★★ | ABC | 8 | E | |
| ★★★ | ODY-ONT | 7 | N | ✓ |
| ★★★ | Storytelling\* | 5 | N | ✓ |
| ★★ | BBO | 7 | E | ✓ |
| ★★ | CRBOnto | 7 | N | ✓ |
| ★ | VLX-Stories | 11 | E | ✓ |
| ★ | Labyrinth\* | 9 | N | ✓ |
| ★ | E | 7 | E | |
| ★ | Mediation | 6 | N | ✓ |
| ★ | Emergent\* | 6 | N | ✓ |
| — | BKOnto | 4 | N | ✓ |

\* Unofficial names introduced by Blin et al.

### 6.2 Main Findings of Blin et al.

Notes from [[2](#ref-blin), pp. 143–145]:

- *Simple requirements:* for events without intricate relations or narrative dynamics, an event-centric ontology such as SEM may suffice. Event-centric ontologies score higher on the 5-star scale but cover fewer complex requirements.
- *DOLCE and DUL:* cover the broadest range, at the cost of complexity and readability.
- *Domain-specific ontologies* (CIDOC-CRM, EDM, ProppOntology) cover the most requirements after DOLCE/DUL, but are hard to reuse outside their domain.
- *Low-rated ontologies* (e.g. Mediation, Emergent\*) adhere less to web standards, which hurts interoperability and integration.
- *Narrative dynamics are rarely covered.* Among 4-star ontologies: SEO and Transmedia\* (scripts, storylines), SEM (perspectives via `sem:View`), FARO (goals, states, conditions). The others that cover dynamics have low ratings.
- A checkmark only shows that a requirement can be represented, not how many relations the ontology offers for it.
- Overall, simple requirements are well covered by event-centric ontologies; narrative dynamics are less covered or less standardised.

### 6.3 Recommendations per Use Case

**Table 6.** Ontology recommendations for the interview use cases. Adapted from [[2](#ref-blin), Table 8].

| Id | Domain | Recommendation |
|---|---|---|
| I1 | History | DOLCE/DUL if using reification, FARO |
| I2 | Biomedical science | PICO with FARO |
| I3 | Social media analysis | Same as I1 + SEM for perspectives |
| I4 | Robotic kitchen | DUL or derived ontologies |
| I5 | Arts and literature | ProppOntology, extending Emergent\* if time permits |
| I6 | Cognitive science | Extend or create an ontology |

### 6.4 Extensions

- **[To be completed]** Extend Table 5 with the L4 and L5 models (Shoilee pattern, CP ontology, NKG) and with columns for ambiguity and metaphor.
- **[To be completed]** Syntax comparison table (which construct from Table 1 each model relies on).
- **[To be completed]** Competency-question results per model.

---

## 7. KG vs NKG: Capabilities and Applications

**Table 7.** Comparison of the capabilities of standard KGs and NKGs, based on [[1](#ref-lippolis)].

| Capability | Standard KG | NKG |
|---|---|---|
| Kind of truth | Factual: what happened | Factual + experiential: what it meant, to whom |
| Memory analogue | Semantic memory | Semantic + episodic memory |
| Key question | What is true? When is it true? | From whose standpoint is this true? |
| Perspective | Usually implicit or absent (single "view from nowhere") | First-class: every assertion anchored to a perspective |
| Plurality | Conflicting claims collapsed or overwritten | Conflicting accounts coexist as linked subgraphs |
| Ambiguity | Treated as noise to resolve | Kept explicit through metaphorical frames |
| Narrative structure | Events, sometimes sub-events / 4W | Events organised into narratives per standpoint |
| Change over time | Versioning of facts | Revised framings kept as auditable layers |
| Role with LLMs | Factual grounding | Perspectival and narrative conditioning + evaluation |

Applications:

- *Cultural heritage:* documenting contested object histories; layers on top of CIDOC-CRM [[1](#ref-lippolis), pp. 6–7]; cf. [[5](#ref-shoilee)].
- *Healthcare:* patient illness narratives linked to, but not collapsed into, clinical knowledge [[1](#ref-lippolis), p. 7].
- *LLM evaluation:* perspective-valid answers instead of fixed ground truths, against benchmark contamination [[1](#ref-lippolis), pp. 7–8].
- **[To be completed]** News / social media (use case I3 in [[2](#ref-blin)]), historical QA, storytelling [[3](#ref-dekok)].

---

## 8. Implications for the Human–LLM–NKG Triangle

- The loop [[1](#ref-lippolis), p. 6]:
  1. the user submits a query; the NKG supplies the perspectival and narrative context accumulated so far;
  2. the LLM responds against this structured background;
  3. the output is evaluated for perspectival appropriateness as well as factual accuracy; the stored metaphorical frame mappings let the system check whether the output fits the user's active framing;
  4. the output is written back to the NKG as a new annotated event;
  5. when the user revises a framing, the subgraph is updated and the previous version is kept as an auditable layer.
- Over time, personal subgraphs can be aggregated, with privacy controls, into community-level NKGs mapping the perspectival landscape of a domain [[1](#ref-lippolis), p. 6].

### 8.1 Evidence for KG-to-Text Generation (de Kok et al.)

- *Goal:* generate narratives from a graph of events that go beyond What, Who, Where and When, using fine-grained causal relations (cause, prevent, enable, intend) [[3](#ref-dekok), pp. 29, 37].
- *Pipeline* [[3](#ref-dekok), pp. 31–33]:
  1. *Build the narrative graph.* Start from the ASRAEL KG (event articles + 4W relations). Extract cause/enable/prevent relations from the linked articles with a pre-trained REBEL model. Merge co-referent event mentions with an event coreference model (iterative clustering, similarity threshold 0.95): 45,031 mentions clustered, 36,057 unique. Model the result in RDF with NIF, SEM and FARO.
  2. *Select information with SPARQL.* From a main event, select the 4W nodes, prioritising those with more incoming edges, plus up to three mentions from the largest coreference clusters (limited by the generator's input size).
  3. *Generate text with JointGT* (BART/T5 pre-trained on 7 M graph–text pairs from Wikipedia), chosen because pre-trained models beat models trained from scratch and encoding the graph topology helps.
  4. *Fine-tune* on WebNLG combined with the FARO dataset, since WebNLG has no FARO relations.

**Table 8.** Sizes of the datasets used for training and evaluating JointGT. Adapted from [[3](#ref-dekok), Table 2].

| Dataset | Train | Val | Test |
|---|---|---|---|
| WebNLG | 12,876 | 1,619 | 1,600 |
| FARO | 1,800 | 201 | 108 |
| Combined | 14,676 | 1,820 | 1,708 |

**Table 9.** Automatic evaluation of the base model (WebNLG) and the combined model (WebNLG + FARO). Adapted from [[3](#ref-dekok), Table 3].

| Model | Test set | BLEU | METEOR | ROUGE |
|---|---|---|---|---|
| Base | WebNLG | 0.6529 | 0.4681 | 0.7535 |
| Base | FARO | 0.0 | 0.0565 | 0.1299 |
| Combined | WebNLG | 0.6101 | 0.4409 | 0.7260 |
| Combined | FARO | 0.0477 | 0.0877 | 0.1949 |

- The combined model scores slightly lower overall: FARO is a small share of the data, and FARO instances have one triple and one target sentence whose relation is often implicit, so word-matching metrics penalise it [[3](#ref-dekok), pp. 33–34].
- Qualitatively, the combined model writes more coherent sentences that keep the triple's direction; the base model often just mirrors the triple, e.g. (*offer*, *cause*, *reimburse*) → "The cause of the offer is to reimburse." [[3](#ref-dekok), pp. 34–35, Table 4].

**Table 10.** Human evaluation of the combined model against the base model (3 annotators, win/lose/tie, sign test at α = 0.05). Adapted from [[3](#ref-dekok), Tables 6 and 8].

| Setting | Fluency win / lose / tie | p | Adequacy win / lose / tie | p |
|---|---|---|---|---|
| 7 ASRAEL events, automatic graph | 71.4 / 14.3 / 14.3 % (κ = 1.0) | 0.11 | 28.6 / 0.0 / 71.4 % (κ = 0.6) | 0.25 |
| 1 manually annotated article | 33.3 / 16.7 / 50.0 % (κ = 0.73) | 0.34 | 58.3 / 8.3 / 33.3 % (κ = 0.61) | **0.04** |

- On the manually built graph, the combined model is significantly better at adequacy (correctly integrating the triples), but not at fluency. BLEU/METEOR/ROUGE against the article were still slightly higher for the base model (0.1874 / 0.2273 / 0.3738 vs 0.1681 / 0.2081 / 0.3622), because it copies more triple parts into badly formed sentences [[3](#ref-dekok), pp. 36–37, Table 7].
- Implications for the triangle:
  - richer relations in the graph lead to more complete generated narratives, so the narrative layer of the graph matters for the LLM side;
  - output quality depends heavily on the upstream steps (relation extraction, coreference) [[3](#ref-dekok), p. 32];
  - n-gram metrics fail to capture semantic adequacy; human judgement was needed, which supports the call of Kommers et al. to include people in evaluation [[7](#ref-kommers)];
  - the work is KG-to-text only: no perspectives, and a small encoder–decoder model rather than a current LLM.

### 8.2 Further Directions

- **[To be completed]** Text-to-KG extraction (FRED, REBEL); evaluation principles [[7](#ref-kommers)].

---

## 9. Discussion and Open Gaps

- Narrative dynamics are poorly covered and unevenly standardised [[2](#ref-blin), p. 145], [[1](#ref-lippolis), p. 4].
- Existing polyvocal approaches document coexisting perspectives but not how they are narratively constructed or framed by metaphor [[1](#ref-lippolis), p. 7].
- No weighting or certainty for competing claims [[5](#ref-shoilee), pp. 140–141].
- Coverage is uneven: from 21 requirements (DOLCE/DUL) down to 2 (EC), even among 5-star ontologies [[2](#ref-blin), p. 145].
- Perspective, goal and outcome need higher-order and epistemic formalisms (Table 2), which standard RDF supports only through reification, named graphs or RDF-star, each with trade-offs [[2](#ref-blin), pp. 138–139, 146].
- Blin et al.'s evidence base is narrow (one project, text- and sensor-based narratives, a Semantic Web focus) [[2](#ref-blin), pp. 135, 147–148].
- KG-to-text from narrative graphs is still weak: little fine-tuning data for causal relations, selection that only looks at the main event and ignores connected events, and automatic metrics that miss semantic adequacy [[3](#ref-dekok), pp. 33–34, 37].
- **[To be completed]** Query complexity, RDF 1.2 adoption, scalability, privacy of personal subgraphs.

---

## 10. Conclusion and Future Work

- **[To be completed]**

---

## References

1. <a id="ref-lippolis"></a>Lippolis, A.S., Do Valle Miranda, L.: From fact to feeling: narrative knowledge graphs for human-AI interaction. In: SEMANTiCS 2026, Blue Sky track (2026). [https://doi.org/10.3233/SSW260025](https://journals.sagepub.com/doi/10.3233/SSW260025)
2. <a id="ref-blin"></a>Blin, I., ten Teije, A., van Harmelen, F., Tiddi, I.: Structured representations for narratives. In: Alam, M., et al. (eds.) EKAW 2024. LNAI, vol. 15370, pp. 133–154. Springer, Cham (2025). [https://doi.org/10.1007/978-3-031-77792-9_9](https://link.springer.com/chapter/10.1007/978-3-031-77792-9_9). Open-access version: [VU Research Portal](https://research.vu.nl/ws/portalfiles/portal/408390551/Structured_Representations_for_Narratives.pdf)
3. <a id="ref-dekok"></a>de Kok, M., Rebboud, Y., Lisena, P., Troncy, R., Tiddi, I.: From nodes to narratives: a knowledge graph-based storytelling approach. In: Proceedings of the Text2Story'24 Workshop. CEUR Workshop Proceedings, vol. 3671, pp. 29–40 (2024). [https://ceur-ws.org/Vol-3671/paper3.pdf](https://ceur-ws.org/Vol-3671/paper3.pdf). Code: [https://github.com/ANR-kFLOW/KG2Narrative](https://github.com/ANR-kFLOW/KG2Narrative)
4. <a id="ref-vanerp"></a>van Erp, M., de Boer, V.: A polyvocal and contextualised semantic web. In: Verborgh, R., et al. (eds.) ESWC 2021. LNCS, vol. 12731, pp. 506–512. Springer, Cham (2021). [https://doi.org/10.1007/978-3-030-77385-4_30](https://link.springer.com/chapter/10.1007/978-3-030-77385-4_30)
5. <a id="ref-shoilee"></a>Shoilee, S.B.A., de Boer, V., van Ossenbruggen, J.: Polyvocal knowledge modelling for ethnographic heritage object provenance. In: Acosta, M., et al. (eds.) Knowledge Graphs: Semantics, Machine Learning, and Languages. Studies on the Semantic Web, pp. 127–143. IOS Press (2023). [https://doi.org/10.3233/SSW230010](https://journals.sagepub.com/doi/full/10.3233/SSW230010). Data and queries: [https://doi.org/10.5281/zenodo.7437713](https://doi.org/10.5281/zenodo.7437713)
6. <a id="ref-gangemi"></a>Gangemi, A., Presutti, V.: Formal representation and extraction of perspectives. In: Vossen, P., Fokkens, A. (eds.) Creating a More Transparent Internet, pp. 208–228. Cambridge University Press, Cambridge (2022). [https://doi.org/10.1017/9781108641104.016](https://www.cambridge.org/core/books/abs/creating-a-more-transparent-internet/formal-representation-and-extraction-of-perspectives-aldo-gangemi-and-valentina-presutti/68031FA35DEDC64DE2790F4E8EEEC7FB)
7. <a id="ref-kommers"></a>Kommers, C., et al.: Computational hermeneutics: evaluating generative AI as a cultural technology. Front. Artif. Intell. 9, 1753041 (2026). [https://doi.org/10.3389/frai.2026.1753041](https://www.frontiersin.org/journals/artificial-intelligence/articles/10.3389/frai.2026.1753041/full)

---

## Notes

[^sem]: SEM specification: <http://semanticweb.cs.vu.nl/2009/11/sem/>; van Hage, W.R., et al.: Design and use of the Simple Event Model (SEM). Journal of Web Semantics (2011), <https://repository.tudelft.nl/record/uuid:cb87d693-77f6-4775-a7fc-c2e406079d49>.
[^dolce]: DOLCE overview (LOA, ISTC-CNR): <https://www.loa.istc.cnr.it/dolce/overview.html>; DUL ontology: <http://www.ontologydesignpatterns.org/ont/dul/DUL.owl>; Borgo, S., et al.: DOLCE: A descriptive ontology for linguistic and cognitive engineering (2022), <https://philarchive.org/archive/BORDAD-3>.
[^cidoc]: CIDOC-CRM specification and versions: <https://cidoc-crm.org/>.
[^faro]: FARO documentation: <https://anr-kflow.github.io/faro/>; Rebboud, Y., et al.: Beyond causality: representing event relations in knowledge graphs. EKAW 2022, <https://hal.archives-ouvertes.fr/hal-03754069>.
[^no]: Meghini, C., Bartalesi, V., Metilli, D.: Representing narratives in digital libraries: the narrative ontology. Semantic Web 12(2) (2021), <https://content.iospress.com/articles/semantic-web/sw200421>.
[^shoilee]: Data, ShEx shapes and SPARQL queries of Shoilee et al.: <https://doi.org/10.5281/zenodo.7437713>; PROV-O: <https://www.w3.org/TR/prov-o/>; CIDOC-CRM: <https://cidoc-crm.org/>; TriG: <https://www.w3.org/TR/trig/>.
[^cp]: Perspectivization ontology: <http://www.ontologydesignpatterns.org/ont/persp/perspectivization.owl>; Framester schema: <https://w3id.org/framester/schema/>; Ontology Design Patterns portal: <http://ontologydesignpatterns.org/>.
[^nkg]: RDF 1.2 Concepts and Abstract Syntax: <https://www.w3.org/TR/rdf12-concepts/>; PROV-O: <https://www.w3.org/TR/prov-o/>; Narrative Ontology: <https://content.iospress.com/articles/semantic-web/sw200421>; Framester schema: <https://w3id.org/framester/schema/>.
