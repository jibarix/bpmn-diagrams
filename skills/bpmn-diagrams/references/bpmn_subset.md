# The BPMN subset used here

Source: OMG, Business Process Model and Notation (BPMN) 2.0.2, January 2014, https://www.omg.org/spec/BPMN/2.0.2/. Only the shapes below are used; they keep their standard meanings.

## Shapes

| Shape | BPMN name | Meaning here | TikZ style |
|---|---|---|---|
| Thin circle | None start event | The run starts on demand | `bpev` |
| Thin circle with envelope | Message start event | A call, email or request arrives | `bpmessage` |
| Thin circle with clock | Timer start event | The run starts on a schedule | `bptimer` |
| Heavy ring | End event | One named outcome of the run: the path stops here | `bpend` |
| Rounded box | Task | One step of work, including selecting or proposing something | `box` (house style) |
| Diamond with X | Exclusive gateway | A branch in control flow: exactly one outgoing path is taken | `bpgw` |
| Coral diamond or coral box | (house mark) | The decision the fixed workflow leaves open to the model | `bpopen`, `focal` |
| Cylinder | Data store | Data kept across runs | `bpstore` |
| Solid arrow | Sequence flow | What happens next, loops included | `flow` |
| Dotted arrow, open line arrowhead | Data association | A step reads or writes data | `bpdata` |
| Open bracket with text, dotted line without arrowhead | Text annotation and association | A note on a shape; here it marks a branch the sources do not describe. It has no effect on the flow | `bpnote`, `bpassoc` |
| Dashed arrow, open circle tail | Message flow | A message between two organisations (pools) | `bpmsg` |
| Horizontal band | Lane | Who acts or decides | `\exlanes` (adapt: drop lanes nobody acts in) |

## Why each rule

1. **Three lanes.** The design question these diagrams answer is who decides each step: the model, the code or a person. Lanes answer it at a glance.
2. **Branches as diamonds in their owner's lane.** A box that says "search again or stop" hides who stops the loop. A diamond in the model lane says the model decides; a diamond in the code lane says a rule decides. Work that selects or proposes something ("Propose the next source") is still a task: it produces a choice, and the branch that acts on it is a diamond.
3. **Loop caps as code-lane diamonds.** An agent loop is bounded only if code stops it. A label such as "until the budget runs out" does not show where the check runs; a diamond "budget left?" in the code lane does. A retry after a rejection is a loop too, so it must pass the same cap.
4. **Proposals through code.** The model proposes; code validates and runs the tool. A model-lane arrow straight to a tool would mean the model acts unchecked.
5. **Acceptance in code or human lane.** A check the model runs on its own work is still the model's judgement; acceptance needs a rule in code or a person.
6. **Named end events; arrows end on shapes.** Each outcome (verified, handed to a person, ended by code) is one the evaluation must count. An unnamed end hides an outcome. In BPMN a sequence flow connects two flow nodes, so an arrow that stops on another line has no defined target; end it on the shared task or add a gateway. The exception is a documented gap (rule 11): there the diagram is an incomplete fragment, and says so.
7. **Solid loops, dashed messages.** In BPMN a dashed line is a message between organisations. A loop drawn dashed, common in informal diagrams, reads to a BPMN reader as a message.
8. **Start event matches how a run starts.** A run starts on an event, on a schedule or on demand. The start shape records which, so a reader sees what triggers the work.
9. **Feedback as data.** Results that arrive weeks later do not continue the run; they are read by a later run. A data store shows that; a control-flow arrow back to the start would claim the run waits for them.
10. **One coral mark.** The diagram's point is the one decision handed to the model. Colouring more than one dilutes it.
11. **Faithful to the sources.** A redraw that adds a policy, drops a step or merges two steps changes the design it claims to show. The inventory and the faithfulness check catch this: every item comes from the old diagram, caption or text, and every change is listed with its reason. Neutral wording is not enough on its own: an arrow into an existing step states what happens next, and an end event states that the process stops (BPMN 2.0.2, 10.5.3), whatever note is attached to it. Where the sources are silent, the branch gets no arrow and no end event; a text annotation on the gateway names the branch and says the sources do not describe it. The drawing is then an incomplete fragment at that point, which is what the sources support.
12. **Labels keep the meaning.** A reader checks the design from the labels, so a shortened label that drops a quantifier ("every"), an object ("a knowledge-base page"), a number ("of 5") or a scope ("fixes included") states a weaker design than the sources do. In a small blind test on Claude Haiku, a model with this skill dropped the object and the quantifier from a check, left the number off a cap, and copied an example step from this skill into a process that did not have it. Hence the rule, the requirement table that maps each requirement to its drawn elements and each element back to a requirement, and the note that examples show form only.

## How a run starts

| How a run starts | Start event | Example |
|---|---|---|
| Always on; a service notices an event | Message start | A phone call arrives |
| On a schedule | Timer start | Weekly planning run |
| On demand; someone hands over the work | None start | A list of companies is handed over |

## Left out, and when to add it

- **Pools and message flows**: add a second pool only when an outside organisation takes part (a client's system, a supplier). Then use `bpmsg` between pools.
- **Parallel and inclusive gateways, intermediate events, boundary events, subprocesses, compensation**: not needed for the diagrams so far. Add one only when the design depends on it, and add it to this table.
- **BPMN XML (`.bpmn` files) and workflow engines**: these diagrams are not executed. If files a modelling tool can open are ever needed, add a script that writes BPMN 2.0 XML; until then a script would add nothing a checklist does not.
