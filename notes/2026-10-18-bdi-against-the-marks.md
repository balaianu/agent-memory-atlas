# BDI against the marks

**Status:** a lens, not a finding about any system
**Origin:** a request on 2026-10-18 to analyse the Wikipedia article on the
belief–desire–intention software model, alongside four weight-side memory
papers that were excluded the same day.

## What this note is

BDI is a model of agency, not an artifact. There is no repository to pin, no
commit to screen and no code to read, so it cannot have a report. The source
was the Wikipedia article, which is secondary, and the account of BDI below is
from that article and from familiarity with the literature (Bratman's theory of
practical reasoning, Rao and Georgeff's BDI architecture, AgentSpeak(L)), not
from code read here. [The symbolic prior-art note](2026-07-28-symbolic-prior-art.md)
already lists BDI belief bases among the older work the atlas does not cover;
this note does the narrower job of laying BDI's three attitudes over what the
atlas already measures.

In BDI an agent holds **beliefs** (what it takes to be true), **desires** (states
it would like to bring about) and **intentions** (desires it has committed to,
with a plan selected). The two operations that define the model are belief
revision and deliberation: deciding which desires become intentions, and
keeping a commitment until it is achieved, impossible, or no longer wanted.

## The mapping

**Beliefs** are what nearly every system in the corpus stores. What BDI adds is
that revision is a defined operation, not an emergent one. In the atlas's terms
that is the difference between a system that stores a contradiction beside the
value it contradicts and one that resolves it, which is the
[resolve-not-just-detect](../content/patterns/resolve-not-just-detect.md) pattern,
and between a status that labels a belief and one a read path honours, which is
the `trust_state` producer test
([trust-state machine](../content/patterns/trust-state-machine.md)).

**Intentions** are the atlas's prospective-memory category, which
[families](../content/families.md) heads "the category almost nothing models".
[NOOA Memory](../content/systems/nooa-memory.md) carries an `intent` type
(trigger-based) and a `todo` with an open/done lifecycle, and
[MineContext](../content/systems/minecontext.md) also remembers that something is
to be done later. [Memento](../content/systems/memento.md) holds *content* back
until a date. None of them is an intention in BDI's sense: what is missing is the
commitment rule, that an adopted intention persists and constrains what the agent
does until it is dropped for a stated reason.

**Desires, or goals,** are not missing, and an earlier answer given in chat on
2026-10-18 said they were. A search of the reports finds goals as a stored type
in many systems, among them [Arcon](../content/systems/arcon.md) (`GOAL` beside
`FACT` and `PREFERENCE`), [Basemode](../content/systems/basemode.md) (a `Goal`
RDF class), [ctx-open](../content/systems/ctx-open.md) (goal, epic and task
records with a state enum), [Jumbo](../content/systems/jumbo.md) (a `Goal`
aggregate carrying a branch and worktree) and [Helm](../content/systems/helm.md)
(`kind=goal`). What the reports show is how goals are *used*: as ranking or
salience context (Helm adds 0.2 to an interrupt score when a goal value appears
in a thought), as task-tracking records (ctx-open, Jumbo), or as a coordination
key between concurrent agents (Jumbo's worktree field). None of the readings
records a goal that selects what the agent commits to next. The gap is
deliberation, not storage.

**The plan library** corresponds to
[skills as procedural memory](../content/patterns/skills-as-procedural-memory.md).
BDI selects a plan by its triggering event and context condition; the skill
systems read here ([MuscleMemory](../content/systems/musclememory.md),
[stella](../content/systems/stella.md), [vir](../content/systems/vir.md)) mostly
retrieve one by similarity to the task or by file overlap.

## BDI and ReAct

A ReAct loop and a BDI interpreter are both perceive–think–act cycles, and a
ReAct agent with a memory store has something like a belief base. What ReAct
lacks is the rest of BDI. Its plan lives in generated text ("Thought: first
check the config"), so the next step can drop or contradict it without anything
recording that it happened. BDI keeps intentions as objects, separates choosing
*what* to pursue from planning *how*, and has a commitment rule saying when to
reconsider. ReAct reconsiders on every step by default, which is one way to
describe why it drifts. Classic BDI also selects a stored plan by trigger and
context where ReAct has the model improvise one; agent skills are the middle
ground. This section is reasoning about the two designs, not a reading of code.

## A ReAct loop with a kanban, memory and delegation

Add a kanban and sub-agents and the shape comes close. Memory is the belief
base; the backlog holds desires; cards in progress are intentions; moving a card
into progress is deliberation; a WIP limit is a commitment policy; delegating a
card is sub-goaling, a plan step that spawns a child intention, here executed
by another agent.

Three gaps remain, and each is where such systems fail. A card leaves "doing"
without a reason, so the next agent cannot tell finished work from work
abandoned because a premise proved false. A card is not linked to the beliefs it
rests on, so when memory learns that a premise is wrong, nothing pulls the card
back for review; that is the cascade problem the atlas keeps finding for
deletion, in another form. And a delegated card arrives without the beliefs
behind it, which is the cross-agent handoff problem stated in BDI's terms.

Four readings hold pieces of this. [Jumbo](../content/systems/jumbo.md) keeps
goals as event-sourced aggregates tied to a git branch and worktree, with a
background agent that reviews an implementation before `jumbo goal approve`.
[ctx-open](../content/systems/ctx-open.md) keeps goal, epic and task records
with a state enum. [Gas Town](../content/systems/gastown.md) orchestrates
several coding agents. [Agent Mesh](../content/systems/agent-mesh.md) comes
closest to the second gap in its schema: a decision carries `assumptions`, and
a `decision_assumption_violated` event would flip one to `violated` and stamp
the event that invalidated it, but no code path in the package emits it. What
does run is the neighbouring rule: editing an accepted decision needs a
`revision_reason` and returns it to `proposed`.

## What the lens adds

It names a distinction the seven marks do not draw. Every mark is about whether
a stored claim can be trusted, scoped, corrected, audited, reviewed, or kept out
of a result. All of them apply to beliefs. None applies to a commitment: there
is no mark for "an adopted intention persists until dropped for a reason", and
the prospective-memory systems above would not obviously earn one if there were:
their readings describe an open/done lifecycle or a release date, and none of
the three reports mentions a recorded reason for an intention ending. That is a
statement about the readings, not a search of the code.

That is not a proposal for an eighth mark. The [note on the atlas as an agent
protocol](2026-08-07-the-atlas-as-an-agent-protocol.md) explains why uniform
judgements nobody has made uniformly are kept out of the machine-readable layer,
and the same reasoning applies here: the corpus holds too few intention systems
to calibrate a test against. What would change that is a system that stores an
intention with its adoption reason and its drop reason, links it to the beliefs
it assumes, and withholds or re-opens it when one of them is superseded. Agent
Mesh's unemitted `decision_assumption_violated` is the nearest schema; a system
that emits it is the one to compare against. If one arrives, this note is the
place to start.
