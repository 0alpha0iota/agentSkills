---
name: clinical-trial-research
description: "Use when a user requests biostatistics-focused research on an individual or series of clinical trials, trial dissection or retrospective review, or trial discovery within a therapy, indication, or defined scope."
license: MIT
---

# Clinical trials research

Research clinical trials from a biostatistician's perspective, focusing on trial statistics and related content. Organize research and presentation according to the user-defined method below, with consistent execution rules that keep evidence traceable, versions clear, and plans separate from actual conduct. The user's explicit scope, format, and methodological adjustments for the current task take precedence.

## Portability

Use the tools available in the host environment to implement this method: source discovery and retrieval, document reading, extraction, calculations, file storage, and diagram rendering can use any suitable tools. No particular agent, operating system, model, paid service, API, or other skill is required. If source access or storage is unavailable, record that limitation and its effect as an explicit gap; lack of access or storage is never evidence that a document or result does not exist.

## Research principles

- Present sourced facts. Do not independently judge trial quality, efficacy success or failure, whether safety is acceptable, or development value. Interpretations and summaries of trial results should, wherever possible, accurately quote or paraphrase the investigators' or sponsor's conclusions/discussion, announcements, or slides, with the speaker and date identified. Rigor means verification and accurate wording, rather than independent evaluation.
- Adapt research depth and presentation to the trial's phase, design, and available materials. Retain applicable core modules, and state the status and reason for missing or inapplicable content.
- Manage evidence by independent trial. Map trial aliases, registry identifiers, and sponsor trial serial/protocol numbers. Keep multiple publications, cohorts, extended follow-up reports, and different data cutoffs from the same trial linked; do not count them again as independent trials. Related trials may be researched in one conversation, but each trial must remain separately identified.
- Strictly separate the original protocol's plans, later protocol amendments, the final statistical analysis plan (SAP), and actual conduct/results. Preserve differences between sources, cohorts, analysis populations, endpoints, time windows, and data cutoffs; do not combine them into an unsourced single number or conclusion.
- Associate every substantive fact with a clear source and time context. Evidence-based classification of design labels, terminology explanations, and transparent checking calculations are permitted. Identify whether each is a source statement, an investigator/sponsor interpretation, an extracted classification, or a research-support calculation. Design classification must not become an independent judgment of trial results.
- Report in Chinese by default, retaining English terms, formal names, and abbreviations that facilitate checking. Follow any different language or delivery format specified by the user.

## Two task entry points

### An identified individual trial or series of trials

Use identifying information such as the trial name/code, NCT number, drug name, and phase to establish the research subject and verify that it is unambiguous. Start directly when information is sufficient. If an ambiguity would change the trial being researched, first present the candidates and their differences, then request the necessary clarification. Subsequently perform the nine research steps below for each trial.

### Trial discovery within a therapy, indication, or screening scope

First conduct a broad search and deliver a trial list. The user will typically start a separate conversation for an individual trial or trial series within the same project for detailed research.

1. Convert the user's conditions into explicit inclusion scope, exclusion criteria, and a search cutoff date. State the interpretation adopted for any unresolved condition that affects the list. Cover trial registries, publications, and sponsor sources; iterate searches using indication/therapy synonyms, drug aliases, development codes, and registry identifiers. Retain search queries and coverage boundaries.
2. Verify identities and deduplicate. The delivery table must include at least: trial code/name, registry identifier and sponsor serial number, lead sponsor, drug/therapy, studied indication/population, phase and basic design, status and its update date, basis for meeting the criteria, and key sources. Mark uncertain records as pending verification; do not silently include or exclude them.
3. Record the search date, information cutoff date, covered sources, screening rules, important exclusions, and identity uncertainties. Explicitly explain coverage affected by pagination, search limits, or inaccessible sources; do not claim that all trials have been exhaustively identified.
4. Provide a separately copyable starter prompt for every trial in the list. Include the skill name, verified identifying information, main source links, research scope, time cutoff, project root directory, and that trial's independent materials/deliverables directories. Distinguish each prompt's trial directories with a different `study-key`; do not directly use the same shared project directory as the materials/deliverables directory for multiple trials. A combined prompt for a sequence or series must state which independent trials it includes and why, while retaining trial identifiers internally.
5. End the default delivery here and wait for the user to start individual-trial research separately. Do not automatically conduct detailed research on the entire list in this task, or automatically create or message other conversations. Take those actions only if the user separately and explicitly requests them.

Example starter prompt structure (replace bracketed placeholders with the actual information for that row; mark unverified fields accordingly):

```text
Use the clinical-trial-research skill to research the following trial comprehensively from a biostatistician's perspective:
Name/code: [name]; registry identifier: [identifier]; sponsor trial serial/protocol number: [identifier or not found]; lead sponsor: [name]; drug/therapy: [name]; studied indication: [scope].
Verified entry points: [links to registry records, publications, and sponsor materials].
Research scope and information cutoff date: [scope and date; if no fixed historical cutoff is specified, use the date of this search].
Project root directory: [known absolute directory]; this trial's materials directory: [independent absolute path containing this trial's study-key]; deliverables directory: [independent absolute path containing this trial's study-key]. If the new conversation cannot access these paths, create independent directories for this trial in its workspace and state their paths.
Follow the skill's nine-step workflow. Distinguish the original plans, protocol amendments, final statistical analysis plan, and actual results. Present facts only, and attribute conclusions to the investigators or sponsor. When requesting assistance downloading materials, provide the exact save directory.
```

## Local implementation and evidence management

### Initial record and directories

Establish the research subject, scope, information cutoff date, and date of the current search. By default, the information cutoff is the current search date. For a historical review limited to information knowable at the time, use materials publicly available at that time; list later materials separately and explain that they are outside that historical information set. Record file acquisition date, publication date, document version date, and results data cutoff separately.

Prefer the user's specified working directory and follow the current project's conventions. If the user specifies only a shared project root, create independent subdirectories for each trial. If a dedicated directory for an individual trial is already explicitly specified, reuse it without adding a redundant nesting level. When there is no convention, use `<workspace>/work/clinical-trial-research/<study-key>/` for the materials root and `<workspace>/outputs/<study-key>/` for deliverables; use an independent `<search-key>` for field-wide discovery. Prefer the registry identifier for `study-key`; if none exists, use the verified trial code. Tell the user the resolved absolute paths. Do not write into the skill installation directory or hard-code paths from a particular session. Inspect existing files first, supplement them incrementally, and retain existing versions.

Create the following minimal structure as needed; do not create empty directories when no corresponding materials exist:

```text
<study-root>/
  sources/                 Original downloaded materials, grouped into registry, protocols, saps, publications, sponsor, and supporting
  incoming/                Materials downloaded with the user's assistance
  extracted/               Text, structured tables, key excerpts, and checking calculations
  figures/                 Original figure screenshots, editable flowchart/timeline sources, and rendered images
  search-log.md            Queries, dates, coverage, and search results
  source-register.tsv      Source inventory, including document versions and acquisition status
  evidence-ledger.tsv      Links between key fields/results and locations in original sources
  issues.md                Materials to obtain, conflicts, gaps, and their resolution history
<deliverable-root>/
  research-report.md       Nine-step report; for field discovery, a trial list and individual starter prompts instead
  figures/                 Final figures cited in the report, if any
```

Use the user's requested report format. If none is specified, use Markdown as the primary format; do not additionally create PDF, Word, slides, or a website by default. Keep original materials separate from extracted files. Name files using source type, version/date, and a short title, and register actual filenames in the source inventory. Checksums may be recorded for important originals to identify updates or duplicate downloads.

### Obtaining materials and verifying source authenticity

There are three main source categories. Choose according to the question being answered; a mechanical overall ranking cannot replace item-by-item verification:

| Source | Main uses | Materials that must be linked |
| --- | --- | --- |
| ClinicalTrials.gov and other trial registries, and materials hosted on relevant regulatory websites | Identity, registration history, design, and prespecified plans | Protocol, SAP, amendment records, and attachments; identify the registry record separately from hosted original documents |
| Trial publications, with PubMed and similar services as discovery entry points | Actual conduct, participants, baseline characteristics, analyses and results, and investigator interpretations | Main text, supplements, appendices, corrections, and associated follow-up/subgroup reports |
| Announcements, pipeline materials, and results presentation slides on official sponsor websites | Official development information, supplementary results, and the sponsor's own statements | Announcement date, conference/presentation date, slide numbers, and corresponding data cutoff |

Cross-check that materials belong to the trial using registry identifiers, trial serial numbers, formal titles, sponsor, intervention, design, and other identifying details. Prefer exact matching when a registry identifier exists; without one, combine multiple independent identifying fields. A similar drug name or title alone does not establish that materials concern the same trial. Verify the publishing entity, original link, identifiers in the document body, and version information. Apply the same verification to supporting sources. Do not present a PubMed record, search snippet, or news retelling as a verified full text/original document.

Search all three channels using the registry identifier, all major aliases, and the sponsor serial number. Trace historical protocol/SAP versions, appendices, and publication supplements, paying particular attention to finding the original protocol, final protocol, and final SAP. For novel methods, separately search for original methodological papers or formal methodological literature; do not apply another trial's statistical plan to this trial. When tools or websites change, consult their current official instructions rather than making a fixed tool name or outdated API a prerequisite.

When a valuable document cannot be obtained, first try reasonably available official alternative entry points. If access remains blocked, give the user a consolidated list containing each document's title, required version, original link, purpose, obstacle, and **exact absolute save path and suggested filename**. Create the corresponding `incoming/` directory first. After the user provides a document, verify its trial identity and version by the same standards, register the source, and copy/archive it in the appropriate `sources/` location. Continue research that does not depend on the missing material, clearly marking affected modules as pending completion.

### Source records, field records, and conflict handling

- Each `source-register.tsv` record must include at least: source ID, trial/cohort, material type and title, publisher, URL/DOI/PMID/registry identifier, document version, date stated in the original and its date type, publication/upload date when ascertainable, acquisition date, local file path, results data cutoff when applicable, basis for verifying trial identity, and acquisition status.
- For key fields, participant counts, endpoints, statistical methods, results, and figures, `evidence-ledger.tsv` must record: field/conclusion, value and unit, trial/cohort/population/time window, planned or actual, version/analysis cutoff, source ID, section/table/figure/page location, original excerpt, and verification status. Retain denominators, effect direction, and confidence level for numerical results.
- Place source links and locators close to the relevant report paragraphs, table rows, and figure captions. For PDFs, distinguish printed page numbers from file page sequence; identify slide numbers for slides, and section and date for webpages. Do not merely accumulate references at the end of the report without mapping them to specific facts.
- Distinguish these statuses: original document missing, not found in the search, located but inaccessible, not reported in the source, version unconfirmed, and not applicable. Do not write "there is no SAP," "no amendments exist," or "the analysis was never conducted" without confirmation.
- When sources conflict, first check version, cohort, analysis set, units, time window, and data cutoff. If these explain the difference, present the values side by side and explain it. If the conflict remains unexplained, retain both values and their locators, and record the issue in `issues.md`; do not silently choose one number. When no second source is available, recheck the context of the original and state "verifiable from only one source"; do not claim independent cross-verification.

## Nine-step research and report for an individual trial

The formal report includes the following nine steps in order by default. Provide the corresponding facts, sources, and necessary gap statements in each step; do not substitute a general clinical review. For trials without published results or those terminated early, retain the applicable sections and identify results that have not yet been made public.

### Step 1: Summarize trial identifiers

List the trial code, if any, and name; trial serial number (sponsor protocol/study number); registration information (usually the NCT number, alongside other registry identifiers); lead sponsor; drug/therapy names and aliases; and registered study indication. Distinguish the registered study indication from approved indications. Explain relationships within multipart studies or trial series, and do not invent missing identifiers.

### Step 2: Briefly describe the indication and mechanism of action

Briefly describe the indication's pathophysiology and the drug's pharmacological mechanism or therapy's mechanism of action. As applicable, explain the drug type (ADC, antibody, nucleic acid drug, small molecule, etc.), target, direction of action (inhibition, antagonism, agonism, etc.), and relevance to the study setting. Keep the detail focused on helping a biostatistician understand the population, design, and measurements; do not expand this into a separate disease or commercial review. Mechanistic facts also require sources.

### Step 3: Phase, nature, and basic design

Provide concise design labels and a brief description, covering as applicable: phase, intervention type, masking, randomization, control type, center arrangement, allocation ratio, parallel/crossover, single-arm/multi-arm, dose escalation/expansion, and multistage or adaptive arrangements. Extract labels by considering both the synopsis and full protocol, and identify the corresponding version. Explain stage relationships explicitly, such as a Simon two-stage design or a double-blind period followed by an open-label period. Cite the specific design details supporting a classification; do not state an inference without source support as an established fact.

### Step 4: Study population and final-protocol eligibility criteria

First briefly describe the study population, then present the detailed inclusion/exclusion criteria **in full according to the final protocol**. Preserve key thresholds, testing methods, time windows, prior-treatment requirements, and exceptions; include the original wording when needed. Identify the version and the basis for calling it final. Keep the final protocol's provisions separate from actual implementation; place differences in actual conduct in Step 8.

### Step 5: Planned trial flow in the original protocol

Use the **actual original protocol** as the basis. First provide a written summary, then draw a flowchart covering screening/enrollment through the end of the trial and relevant follow-up.

- Usually show earlier to later stages from left to right. Use branches or swimlanes for arms/cohorts and separate the stages.
- Label planned numbers only, distinguishing groups/cohorts, and do not mix in actual participant flow. State when planned numbers were not assigned to individual groups; do not fill in group counts from the allocation ratio without a source.
- Show applicable screening, enrollment, randomization, treatment/observation stages, planned interim analyses, stage-progression rules, EOT, and follow-up periods, retaining key durations and conditions.
- Use EOT, EOS, and similar abbreviations according to their definitions in this trial's original documents. Verify whether each means end of treatment, end of trial, or end of study; do not automatically treat them as equivalent, and do not omit post-treatment follow-up.
- The figure title and caption must explicitly say "planned flow in the original protocol" and identify the version, date, and source. This is the plan at the time of design; it does not imply what actually happened.

If the original protocol is unavailable, state explicitly: "The original protocol was not obtained; the initial plan cannot be fully confirmed." A separate **planned flow in the earliest available version** or **partial reconstruction of the plan recorded in registry history** may be drawn, but prominently identify its basis, version, and missing parts. Do not label it "original protocol flow," or reconstruct the original flow retrospectively from the final protocol or results.

### Step 6: Protocol revision timeline

Focus on amendments involving core clinical-development decisions or major effects on statistical analysis, such as changes in the target population, dose/treatment arms, cohorts and sample sizes, study stages/flow, objectives/endpoints, observation periods, or analysis arrangements. Minor revisions may remain in the source index; do not overcrowd the timeline with textual corrections.

First make an amendment table: version number, version/effective date and date type, core changes, reasons disclosed in the original source, and source locator. State when reasons are not disclosed. Do not infer causation merely because adverse events (AEs), communications with an institution or agency, and an amendment occurred near one another in time. If only a signature date or version date is available, present it as that date type; do not treat it as a confirmed effective date.

Next compare the original and final protocols item by item, covering treatment, groups, doses, number and size of cohorts, total sample size, observation/treatment periods, visits, trial design/flow, and applicable objectives/endpoints. Present "original version — final version — evidence." When versions are missing, state which two versions were actually compared.

Finally produce a timeline visualization with a time axis. Chronologically mark the original version, final version, and major amendments with version numbers, dates, and brief changes. Add the final SAP version/date, database lock, and database-freeze dates for relevant interim analyses. Use spacing proportional to actual dates where possible. If only a year/month is available or a date is missing, show the date precision or unknown status; do not fabricate exact dates. Stagger labels when events are dense.

Record SAP signature/version dates, amendment dates, freeze/lock dates, and public upload dates separately. If evidence shows that the final SAP changed after a freeze/lock, identify the change date, specific changes, and reasons disclosed in the original source. If the materials show only that an SAP date is later, report only the confirmed sequence and gaps; do not independently conclude that the analysis plan was developed post hoc. Map each interim/final data cutoff separately rather than combining them into one database lock.

### Step 7: Prespecified objectives, endpoints, sample size, and statistical analyses

Extract primarily from the **final SAP**, cross-checking against the **final protocol**. If the SAP is not public or cannot be obtained, extract from the final protocol and verify with publications/supplements and other sources. Explicitly identify source substitutions and remaining gaps. When planned analyses differ from published analyses, show them separately; do not unconditionally describe methods used in a paper as prespecified methods.

1. **Study objectives and endpoints:** Present the relationships between primary, secondary, and exploratory objectives and endpoints, prioritizing primary endpoints and retaining key secondary endpoints. For each study endpoint, briefly describe the measurement method/scale, data type, units, assessment time window, and clinical meaning. Where applicable, retain classification criteria, assessor, event definition, and method of aggregation. Do not mechanically pair objectives and endpoints one to one; follow the relationships in the source.
2. **Sample size estimation and statistical assumptions:** Extract the calculation method, main parameters, assumptions, significance level and one-/two-sidedness, power, expected effect/rate/variability, allocation ratio, adjustment for dropout or non-evaluability, event-count/enrollment/follow-up assumptions, stage/cohort sample sizes, and stopping/expansion rules, selecting what applies to the design. Leave missing parameters missing. For any necessary recalculation, separately list inputs, formulas, code/method, and discrepancies; the recalculation cannot replace the source's plan or alter its assumptions.
3. **Key prespecified analyses:** For the primary endpoint, key secondary endpoints, and other important analyses, describe the primary analysis framework, analysis sets, between-group tests, effect measures, and estimation/interval-reporting methods. Then list applicable sensitivity analyses, missing-data handling, subgroup and interaction analyses, multiplicity control, interim/sequential testing, and thresholds. When applicable, record estimands, intercurrent events, covariates, censoring, and model assumptions, keeping them linked to their endpoints and plan versions.
4. Give additional focused explanations and verifiable methodological references for novel or complex methods. Distinguish evidence that **this trial actually used the method** from references explaining **how the method works**. When verifiable materials on subgroup or multiplicity arrangements are unavailable, mark them as not reported in the source; do not automatically conclude that no such arrangements existed.

### Step 8: Actual conduct and trial results

Use the corresponding publications and supplements as the primary sources, supplemented by official sponsor announcements/slides and other verified materials from this trial. For each results report, identify the report date, data cutoff, follow-up duration, and analysis population. Keep updated reports distinguishable.

**First describe actual conduct.** Present available information on actual screening/enrollment, enrolled numbers, randomization/cohorts, treatment received, withdrawals, completion, and analysis sets, and provide participant flow. A separate actual-flow diagram may follow Step 5's graphical conventions, using accurate numbers and reported key event dates. Mark unknown numbers/dates as missing; do not derive them from plans. Describe visits and follow-up for applicable stages. Check counts by stage, group, and population definition; do not force counts with different definitions to add up to a total.

**Then describe the population and baseline characteristics.** Give an in-a-nutshell summary of the inclusion/exclusion provisions in the version of the final protocol actually implemented. If only the final protocol's provisions can be confirmed, state that evidentiary scope; do not claim that this version was implemented throughout recruitment. Use the paper to present verifiable eligibility criteria actually applied, visits/follow-up, and differences from the protocol. Extract baseline characteristics remarks and the baseline characteristics summary table, retaining each group's N, units, summary-statistic format, missing values, and original table footnotes. Do not independently conclude that baseline differences establish the quality of randomization.

**Next summarize the core results.** Use a "primary — secondary endpoints" or "efficacy — safety" structure as appropriate for the trial. Always give primary and key secondary endpoints the highest presentation priority. Retain factual statuses such as unfavorable results, failure to meet prespecified criteria, or results not publicly disclosed; do not select only results emphasized by the sponsor. For safety, preserve the source's definitions and reporting conventions for the applicable population, exposure, AE categories/grades, seriousness, and denominators. Without a control, do not independently make comparative judgments.

As needed, result tables include: endpoint/time window, analysis population and N, results or event counts for each group, effect size and its direction/reference group, 95% CI, p-value, test/model used, data cutoff, and source. Preserve source precision, exact p-values or inequality signs, one-/two-sidedness, and nominal/adjusted meaning. Explicitly mark unreported p-values or effect sizes; do not invent them. When a source gives another confidence level, report it faithfully with that level identified; do not convert it to a 95% CI without a basis.

If there is a prespecified sequential testing chain/multiplicity strategy, show each step's hypothesis, threshold, p-value, and source-described pass/stop status in the order actually implemented. Distinguish hypotheses eligible for formal testing, those not reached in formal testing, and descriptive/nominal results. Mark undisclosed aspects of implementation as unknown; do not treat all nominal p-values as having passed multiplicity control.

Capture the relevant figures/tables supporting core results from publications or sponsor slides. Preserve titles, legends, axes, necessary footnotes, and source page numbers; record the corresponding population and data cutoff, and place them near the relevant results. If the original figure/table cannot be obtained, state that fact. If necessary, transcribe/redraw verified data and label this explicitly; do not present it as an original figure. Reconstructed figures/tables must not add data absent from the source, and values estimated from an image must not be treated as exact reported values.

### Step 9: Investigator and sponsor summaries

Accurately quote or paraphrase the publications' conclusions/discussion or sponsor announcements/slides. Identify the speaker, source, and date for each paragraph. When investigator and sponsor statements differ, present them side by side. Keep their interpretations separate from the objective data above. Do not turn promotional statements into facts endorsed by the assistant, or add independent judgments of trial success/failure, therapy quality, clinical recommendations, or investment judgments. Keep quotations necessary and brief; faithfully paraphrase longer material.

## Delivery and verification

### Delivery structure

Begin the report with trial identifiers, research scope, information cutoff date, search date, and availability status of key document versions. Follow the nine-step order in the body, and end with material gaps/pending verification items and a source index with locators. Distinguish at least three kinds of visuals: the original planned flowchart; a protocol/SAP revision and database-freeze/lock timeline; and actual participant flow/implementation flow when evidence is available. Embed original results figures and baseline tables in their corresponding sections. Retain editable source files or generation code for visuals and provide rendered images viewable in the current delivery environment; do not deliver only diagram code that cannot be viewed.

Evidence is required to identify a protocol as original/final or an SAP as final. If only the earliest/latest available version can be obtained, consistently call it "earliest/latest available version as of YYYY-MM-DD" and state the unknown version intervals. Even when a source describes itself as final, retain its stated version/date and applicable analysis. Identify material gaps in the affected sections, rather than only in an appendix.

### Predelivery checklist

- [ ] Names, all identifiers, lead sponsor, drug/therapy, and studied indication correspond to one another; trial series, cohorts, and repeated reports have not been incorrectly merged.
- [ ] Cutoff dates, source versions, acquisition dates, and each results data cutoff remain distinguishable, and key facts have nearby source locators.
- [ ] Step 4 uses the final protocol's eligibility provisions; Step 5 uses only evidence of the original plan; Step 7 prioritizes the final SAP; Step 8 concerns actual conduct and results. Substitute versions and evidence gaps are prominently identified.
- [ ] The major-amendment table, original/final comparison, and timeline agree. Version/effective/signature/upload dates have not been confused; amendment reasons or post-lock changes have not been fabricated.
- [ ] Primary/key secondary endpoints, sample size assumptions, key prespecified analyses, missing-data handling, and applicable multiplicity strategies are presented or have a stated missing-information status.
- [ ] Planned and actual numbers remain separate. Actual counts, denominators, analysis sets, event counts, units, time windows, effect directions, and CI levels have each been checked.
- [ ] Key extracted values have been checked against the originals. Numbers, minus signs, decimals, CIs, and footnotes in OCR, converted tables, and screenshots have been manually checked. Cross-verification was performed when an independent second source existed, and conflict-resolution records were retained.
- [ ] The baseline table, key results, applicable actual testing chain, and source figures/tables are included. No p-values, 95% CIs, unknown dates, or unpublished results have been invented.
- [ ] Flowcharts and timelines have been rendered and viewed. Labels are readable and branches/stages/dates are correct. Results screenshots retain necessary context, and report file links work.
- [ ] All results interpretations and final summaries identify the investigators or sponsor as the speaker. No independent trial evaluation, clinical advice, quality score, or therapy ranking has been added.
- [ ] A field-discovery task delivers a deduplicated list, coverage explanation, and one starter prompt per trial. The prompts use trial-specific materials/deliverables directories, and no unauthorized batch deep research or conversation creation has occurred.

Repair identified problems and recheck the affected parts. If an issue remains unresolved, explicitly list the material or access limitations and their effects. Describe completion status truthfully: missing key originals preclude claiming a complete reconstruction of the original design, all amendments, or all prespecified analyses.
