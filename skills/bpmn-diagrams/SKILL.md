---
name: bpmn-diagrams
description: Draws process diagrams for agentic systems with a small BPMN 2.0 subset in TikZ, with lanes for the model, the code and the person, so the diagram shows who decides each step and where every cap is enforced. Use when drawing, redrawing or reviewing a workflow, agent loop or process diagram for a LaTeX document, or when the user asks for a BPMN, swimlane or process-flow diagram of an agent or automated workflow.
---

# BPMN diagrams for agentic systems

A diagram here is a design specification: it says who decides at each step and where the limits sit. It is not run by a workflow engine. The BPMN shapes carry fixed meanings, so a reader can check the design from the picture.

## Hard rules

1. **Three lanes:** MODEL (chooses the next step), CODE (RUNTIME) (rules, limits, tools), HUMAN (approval, review). A lane is drawn only if someone in it acts.
2. **Where control flow branches, draw a diamond in the lane of whoever decides.** A box is work, including work that selects or proposes something ("Propose the next source"). Never hide a branch inside a box ("read results: search again or stop").
3. **Every loop passes a diamond in the code lane that enforces the cap** ("turns left (of 8)?", "budget left (of $2)?": the label keeps the limit). A cap written only in a label does not count. Retries after a rejection are loops too.
4. **A model's proposed action goes through code before it runs.** The model lane never connects straight to a tool.
5. **The acceptance check sits in the code or human lane,** never in the model lane: whether the work is accepted is not left to the model's own judgement.
6. **Every path ends in a named end event** (heavy ring with a label), and every arrow ends on a shape (event, task or gateway), never on another line. The one exception is a documented gap (rule 11): a branch the sources do not describe stops at its gateway with a text annotation, and no end event. To merge paths, end each arrow on the shared task or add a gateway.
7. **Solid arrows are sequence flow, including loops.** Dashed arrows are message flows between organisations only; dotted arrows (open line arrowhead) are data; a dotted line without an arrowhead joins a text annotation to its shape.
8. **The start event matches how a run starts:** message (a call or request arrives), timer (on a schedule), plain circle (on demand: someone hands over the work).
9. **Feedback across runs is data, not control flow:** a data store with a dotted arrow into the step that reads it.
10. **Coral marks the one open decision** the fixed workflow leaves to the model: the box where the model selects or proposes, or the diamond where it branches. Nothing else is coral.
11. **Draw what the sources say, and nothing more.** Every step, owner, cap, exit and outcome comes from the source diagram, caption or text. Do not invent a policy (for example "dropped" for a rejected draft). Neutral wording covers labels, not behaviour: a neutral label does not license a continuation, a merge or an ending the sources do not state. An end event is a statement too: it says the process stops there. Where the sources are silent on what follows a branch, draw no arrow and no end event for it; attach a text annotation to the gateway naming the branch ("no (caps reached): next step not stated in sources"), and report the gap. Never route the path into an existing step, or end it, to make it look complete.
12. **Labels keep the source's meaning.** A task or gateway label may shorten the source's wording, never its meaning: keep the condition, its object, quantifiers (every, each, at most), numbers and scope (what a shared limit covers). "Code checks that every cited figure appears in the filing" becomes "every cited figure in the filing?", not "figures OK?"; "at most 3 searches per question, retries included" becomes "searches left for this question (of 3, retries included)?". If the meaning does not fit, use a longer label or a text annotation.

## When to use

- Drawing or redrawing a workflow, agent loop or process diagram for a LaTeX document.
- Reviewing an existing diagram for the rules above.
- Not for architecture diagrams (components and connections), timelines or charts, and not for executable BPMN (`.bpmn` XML for a workflow engine).

## Workflow

1. **Complete the inventory, a table of requirements in `requirements.md` beside the `.tex`, before creating or editing the drawing.** Split the sources (the old diagram if any, its caption, the surrounding text and tables) into single requirements: each step, its owner, each arrow and branch condition, each cap with its number and scope, each exit (what happens when a cap runs out or a check fails), each outcome, and how a run starts. One row per requirement, with the source quoted word for word. Add nothing the sources do not state. Where the sources are silent on a branch, write a gap row ("not stated"): a gap in the sources is not an omission in the drawing. Mark each row `draw` or `text`: starts, steps, owners, arrows, branch conditions and their criteria, caps, exits and outcomes are `draw`; only descriptive data that decides none of these (what a message contains, stored fields) may be `text`, and stays in the document's prose. If the `draw` rows do not fit one legible page, split into linked diagrams that keep the sequence and return paths; each cap keeps its own code-lane gateway on every path it limits. Never merge caps or drop a row to make it fit.
2. **Draw the lanes** with `\exlanes` from `assets/bpmn_tikz.tex`. Adapt it: drop a lane nobody acts in.
3. **Place events, then tasks, then gateways.** Start event at the left of the lane that notices the trigger. Tasks are verb phrases taken from the source. Each gateway gets a short question as its label that keeps the source's condition (rule 12), and each outgoing arrow gets its answer ("yes", "no: turns spent"). The examples in this skill show the form only: do not copy their steps, objects or values into a diagram whose sources do not state them.
4. **Draw the flows.** Prefer right-angled paths (`|-`, `-|`). Loops back into the model lane go over the top of it; return and bypass paths in the code lane run along its bottom strip (y 0.45-0.8), under the shapes on y 1.45. End every arrow on a shape.
5. **Add the legend** with `\exlegend{legbox or legopen}{extras}`: `legbox` when the coral mark is a task, `legopen` when it is a gateway; extras add timer, message or data. Remove built-in items the diagram does not use.
6. **Compile and look at the result** (see Output format). Fix overlaps and labels that cross lane borders or touch shapes before checking the rules.
7. **Check every rule** with the checklist below, and report any you could not meet and why.
8. **Check faithfulness both ways, in the table.** Add a column to `requirements.md`: for each `draw` row, the drawn element or elements (one requirement can map to several elements, and one element to several requirements), marked same, weakened, dropped or gap (gap only where the sources are silent); for each `text` row, where the prose covers it. Then list every drawn element (task, gateway, arrow, label, end event) with no requirement behind it, marked added. Every weakened, dropped or added row is fixed in the drawing, or reported with its reason; this includes changes of behaviour (a new gateway, a moved cap, a rerouted return, a dashed loop made solid). Shared presentation changes (lanes, sizes, connectors, legend) can be listed once for all diagrams. The faithfulness lines of the checklist may be ticked only with this table in the report; a tick without the table is a failed check.
9. **For a diagram in a published document,** have the table checked by a fresh reviewer (a subagent or a second model) that sees only the sources, `requirements.md`, the `.tex` and the render, and returns each discrepancy with the source quote and the diagram element. The table makes the check auditable; it does not validate itself.

Checklist to copy into the report:

```
- [ ] lanes: model, code, human (only those used)
- [ ] every branch is a diamond in its owner's lane; boxes are work only
- [ ] every loop, retries included, passes a code-lane cap diamond
- [ ] model proposals pass through code before any tool runs
- [ ] acceptance check in code or human lane
- [ ] every path ends in a named end event, or stops at a gateway with a gap annotation; every arrow ends on a shape
- [ ] solid = sequence, dashed = message only, dotted with arrowhead = data, dotted without arrowhead = annotation only
- [ ] start event type matches how a run starts
- [ ] feedback across runs drawn as a data store
- [ ] coral on the open decision only
- [ ] requirement table in `requirements.md`, written before the drawing: every `draw` requirement drawn or marked a gap, every drawn element traced to a requirement; weakened, dropped and added rows fixed or listed
- [ ] labels keep the source's condition, object, quantifiers, numbers and scope
- [ ] every gap in the sources marked with a text annotation on its gateway, not filled in or ended
```

## Output format

- TikZ inside a `tikzpicture` with the `dp` option, using the styles and helpers in `assets/bpmn_tikz.tex`: paste the file into the preamble, or `\input` it, once per document. Its header lists the packages it needs.
- Compile twice with `pdflatex`. Render pages to PNG with Ghostscript (`gs -sDEVICE=png16m -r200`; `mgs` in MiKTeX) and look at them. On one Windows machine `pdftoppm` dropped line segments and broke circles, so check a doubtful render with a second renderer before changing the drawing.
- Report: the diagram's page, the requirement table from `requirements.md` (source quote, draw or text, drawn element, same / weakened / dropped / added / gap), the checklist with each item ticked or explained, every addition, merge or rename against the sources, the gaps where the sources were silent, and any label you shortened to fit.

## References

- `references/bpmn_subset.md`: the shapes used and their meanings, the reason for each rule, how the start event maps to how a run starts, and what BPMN features are left out.
- `assets/bpmn_tikz.tex`: the TikZ styles and helpers to paste into a document.
- `examples/`: three worked diagrams (a phone agent, an investigation agent, a weekly planning run with gap annotations), each a standalone file that compiles from that folder. Form only.

No scripts: the drawing is TikZ styles and the check is the checklist. A script would pay off only if `.bpmn` XML files were needed for a modelling tool.

Codex reads skills from `.agents/skills/`; copy this folder there if Codex should use it too.
