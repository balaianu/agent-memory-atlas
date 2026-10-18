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
intention with its adoption reason and its drop reason, and a read path that
withholds a dropped intention from the agent's plan. If one arrives, this note is
the place to start the comparison.
