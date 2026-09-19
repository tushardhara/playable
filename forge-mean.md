# Forge — How It Works

*Plain-English explainer for `1-FORGE.md`, revision 8, which is the revision sitting beside this file.
Nothing here is a rule; the spec is the authority, and where the two disagree the spec wins.*

**Where things stand.** Four cold reviews and one play have found 74 defects between them and 70 are
repaired; the fourth review read revision 8's own repairs and found fifteen of those wanting.
**Nobody has played revision 8, not once.** Everything below describes what the file says, not what
anyone has watched happen.

---

# 1. What is Forge?

Forge is a **story-world generator**, not the story itself.

You give it a small creative brief:

> Two young professionals, Aarav and Meera, meet in Bangalore, dislike each other, become friends,
> fall in love, and then face career, family, marriage and timing conflicts.

Forge asks you eleven questions about the kind of story you want, then turns the brief into a
numbered **World Card** you can edit line by line. When you are happy with it, Forge emits **FILE 2**
— one self-contained file holding the world, your settings, and the runtime rules needed to play it.

The flow:

**Short brief** → **eleven questions** → **numbered World Card** → **you edit it** → **FILE 2** →
**paste FILE 2 into a fresh chat and play 66 turns**

Two files, two jobs:

* **FILE 1** is Forge itself — the instructions that build worlds. It never ships to a player.
* **FILE 2** is what a player actually runs. Capped at **3,500 words**, because it has to be carried
  in a single chat.

---

# 2. Why not just prompt an LLM?

If you tell a model "write a 60-episode Indian romance," it writes good individual scenes and then
long-story problems arrive:

* characters behave differently after 20 episodes,
* closed conflicts come back under a new name,
* timelines stop making sense,
* lost objects reappear,
* someone becomes a villain because the plot needs one,
* choices turn out not to matter,
* the ending is not earned.

Forge's answer is to fix **rules and constraints before any story is written**: who has power, why,
when that power ends, what remains afterwards, what can never happen again, and what the protagonist
must achieve before an ending is allowed.

---

# 3. The eleven questions

Forge asks all eleven in one message. Every option says what it means in plain words, every question
has a marked default, and typing `ok` accepts all remaining defaults. An author in a hurry is two
messages from a world.

| | Question | What it sets |
|---|---|---|
| Q1 | What kind of story is this? | The preset — serial melodrama, puzzle, literary realist, mystery, or mix my own |
| Q2 | Language | English, Indian English with Hinglish, Hindi in either script, or name another |
| Q3 | Tone, up to three | Tense, warm, funny, bleak, dry, angry, sentimental, hopeful |
| Q4 | Whose head are we in? | One person only what they know / one person thinking / several people |
| Q5 | How clear is right and wrong? | Murky / mostly clear / crystal clear |
| Q6 | Does the world reward good people? | Realistic, or villains overreach and fall |
| Q7 | How often does the situation flip? | Rarely / by re-reading old facts / with new shocks |
| Q8 | Can the reader solve it ahead of the hero? | Doesn't matter / mostly / strictly |
| Q9 | What does the hero's edge cost to use? | Every time / only when careless / nothing |
| Q10 | How much world at once? | Light / medium / dense |
| Q11 | Boundaries | PG-13 / adult themes / never include: name it |

**The defaults are the serial setting** — the only configuration with evidence of sustained play
behind it. Q1 fills in Q4–Q9 from its row, so picking "serial melodrama" answers six questions at
once; changing any of them individually overrides that cell. **Mix my own** has no row: it wants all
six answered, and `ok` does not stand in for them.

Three answer pairs fight each other, and Forge says so rather than quietly building the contradiction:

* Q6 *yes* with Q5 *murky* → unmotivated coincidence.
* Q8 *strictly* with Q4 *several people* → the puzzle is spoiled before it starts.
* Q7 *new shocks* with Q8 *strictly* → every arrival needs a planted line first.

Story length is fixed: **six chapters of eleven turns**. A shorter story is a different file, not an
option here.

---

# 4. PROFILE: your answers, written down

Your eleven answers become **PROFILE**, a short `key: value` list of at most 100 words. It sits
between the world and the runtime in FILE 2 — the file opens with the story's own title, never with
settings — so the rules are read after the values that parameterize them. Most of it shaped what Forge authored and is there for the reader.

**Six values do real work at play time.** They are substituted into the runtime as it is emitted:

| Setting | Follows | What it changes |
|---|---|---|
| `prose_normal` | Q1 | Words per ordinary scene |
| `prose_t11` | Q1 | Words in a chapter's closing scene |
| `prose_terminal` | Q1 | Words in the final scene |
| `choice_words` | Q1 | How long each choice is |
| `quiet_quota` | Q3 | How often a quiet, warm beat is required |
| `clue_rule` | Q8 | Whether a reveal must quote the line that planted it |

So a serial gets short punchy scenes (130–180 words) with six-to-ten-word choices; a puzzle gets
longer ones (180–240) with room to explain. A story whose tone includes Warm or Hopeful owes a quiet
beat every chapter; a bleak one owes one every two, because a story with no warmth in its register
should not be forced to stage relief it has not earned.

Any of the six can be overridden after the World Card: `edit P.choice_words 5–9`.

Five of the six are **values, never rules**: they sit inside a sentence that survives them, so
substituting one changes its own number and nothing else, and no setting is permission to delete the
words around it. `clue_rule` is the exception, because its value is a whole sentence: it stands on a
line of its own, and at any Q8 answer but *strictly* it is replaced by nothing, which removes that
line and only that line.

---

# 5. Editing the World Card

The card arrives numbered, so you can name any part of it:

* `edit 4.2 <text>` — change that field
* `redo 4` — rebuild that block
* `options 4` — offer three alternatives, change nothing
* `why 4.2` — explain how it was derived, change nothing
* `R3.closure` — address a rung's line directly

Forge rebuilds whatever depended on your change and prints what moved. If a rebuild would touch more
than half the card, it says so and asks first.

Three shortcuts do bigger swaps:

**BRAKE `<replacement>`** — replace the brake and rebuild everything affected.

> BRAKE family approval

**FAMILY `<name>`** — this one is confusingly named. It does **not** mean the protagonist's
relatives. It means the **storytelling family**: the genre pattern and narrative flavour, and the
stylistic assumptions rebuild around it.

> FAMILY workplace rom-com — or — FAMILY darker family drama

**HARDER `<rung>`** — keep the story, make that rung cost more. Its arena, its holder and the source
of the next conflict stay as authored.

> HARDER R3

One more mode: a **scored cold run** — the word `scored` in your premise line. Forge still asks the
eleven questions, but accepts no edits after the World Card, and the next thing you type emits FILE 2
as drafted. It exists so somebody who did not write the world can judge it.

---

# 6. Premise, wound, hidden advantage

The **premise** is what the story is fundamentally about.

> Aarav and Meera love each other, but career, family, geography, marriage expectations and timing
> may stop them building a life together.

The **wound** is an emotional pattern that makes decisions harder. It need not be trauma.

> Aarav treats depending on another person as weakness.

So even loving Meera, saying *"I need you"* is expensive for him. The conflict is inside him, not
only in the people blocking him.

The **hidden advantage** is what he has that others do not.

> Aarav and Meera read each other unusually well, because the relationship started in blunt honesty.

That advantage would solve the story by episode three — which is what the brake is for.

---

# 7. The brake, and what it costs

The **brake** answers: if these two love each other, why can't they simply talk and fix everything?

Career ambition is the *pressure* in this story, not the brake — a motive is not a brake, because
nothing physical changes when it bites. The brake is derived from a noun already in the world: **the
credit line on the launch document.** Every time Aarav uses what he knows about Meera to steer an
outcome, his name comes off one more deliverable on that page.

* He uses her own words to move her off the Singapore call → his name leaves the client deck.
* He reads the room and lets her take the launch → the credit line names her, not him.
* He steers Sunita by what only he knows Meera fears → the reference letter loses his signature.

Each is **concrete, on the page, and caused by using the advantage** — which is what makes it a brake
rather than a mood. Never "he felt scared."

**Q9 decides when it lands, never what it is:**

* **Every time** — any use of the advantage incurs the cost, goodwill or not.
* **Only when careless** — a use costs when the hero was visibly reckless; a careful one can resolve
  cleanly.
* **Nothing** — the advantage carries no automatic cost, and a consequence needs an actual authored
  cause. The brake still exists and still names its price; nothing manufactures occasions for it.

That last setting exists because the project's own evidence says a clean win has to be allowed to
stay clean.

---

# 8. Fuse and rounds

The **fuse** is the long-term progress condition: six things that must genuinely happen before the
big ending unlocks. Here, **six costly truths** — six moments where someone states a real want or
boundary and accepts a meaningful consequence.

> "I want you to take Singapore. But I also need you to know I love you."

It counts only if saying it actually cost something — and **at most one lands per chapter**. Six
chapters, six events, no banking them for later: a chapter that misses its fuse event has spent it,
and the complete-success ending is gone for good. That is the tightest thing in the design.

**Rounds** count something different: how many chances the story has *offered*. So:

`fuse 3/6 · rounds 5/6`

— five chances have come and gone, and the hero converted three. The difference between
*offered* and *earned* is the point.

Both print in the state line **every single turn** — this is not an invisible meter, and an earlier
version of this explainer was wrong about that.

Each chapter closes exactly one round, one of three ways:

* **completed** — the chance was taken.
* **refused** — the hero turned it down.
* **lapsed** — it ran out of chapter without either.

Putting a chance off until later inside the same chapter closes nothing. This matters more than it
sounds: because every chapter closes a round, rounds always reaches 6 by turn 66, so the story always
has an ending it is allowed to print. Before revision 8 it could arrive at the last turn with
nothing legal to say.

---

# 9. The cast

A fixed protagonist and **at least six named supporting characters** — three active from the start,
the rest in reserve. Six is a floor, not a target: six rungs each need a holder who is not the
protagonist.

Each supporter gets an independent **want**, one or two specific **facts they hold**, a **refusal**,
and a **named rival** whose incompatible want competes for something concrete. A want must be
something they would chase unprompted and that the hero could grant or refuse — not an attitude.
Nobody exists purely to demand a confession.

**Q10 sets how many are on screen at once**, never how many exist: light keeps four or fewer and adds
at most one new name per scene, medium keeps about six, dense keeps a full social world present —
friends texting, a crowd reacting, a rival watching. At most three people hold a scene; the
background is not capped.

---

# 10. Pleasures and a sanctuary

Four to six **pleasures**: concrete things the advantage lets the hero do *for* someone. Rescue,
reward, protect, indulge, transform, invest, impress, create freedom, reverse a humiliation.

Each names who reacts and what changes, with real names and real amounts — "₹2,40,000 to clear
Sunita's loan," never "help people." These are the acts a player repeats for sixty turns.

Plus one **sanctuary**: a place and a person where nothing is being extracted from the hero.

Generosity is allowed to work. A grateful person may simply be grateful, with no conspiracy behind
it, no bill later.

---

# 11. R1–R6: the conflict ladder

Six chapters, six **rungs**, each a different source of conflict:

**R1 — Work.** Aarav and Meera compete for an important launch.
↓
**R2 — Family.** Meera's family introduces Kabir as a marriage prospect.
↓
**R3 — Career.** Meera gets the Singapore offer.
↓
**R4 — The sanctioned suitor.** Kabir, standing on the invitation Meera's family gave him, can demand
a public answer at the engagement lunch — or withdraw in front of both families.
↓
**R5 — Conflicting duties.** Aarav has fixed family obligations at exactly the wrong moment.
↓
**R6 — The deadline.** Ms. Rao in Singapore holds the transfer offer open until a date she sets. The
decision is Aarav and Meera's; the authority that forces it is hers, and it closes when the offer is
signed or lapses.

Adjacent rungs must demand *different responses*, not just different places. At least one dispute has
to turn on a material constraint, one on a voluntary relationship, and one on conflicting duties —
so the six do not become one problem in six costumes. And every rung needs a **holder** who is not
the protagonist: a person with an authority that can actually be extinguished. "Whether these two
can be together" is not a rung, because there is nobody to strip the power from.

The holder lives in the rung's own row, with the arena and the stake, and that row's shape is fixed:

> - R1 — the launch / Dev / who owns the client account · Dev alone picks the launch lead.

Dev is not evil. He may simply want the lowest-risk person on an important client launch. That is how
cartoon villains are avoided.

Under the row go **six lines**:

**1. Mechanism** — what the holder can actually do. Dev picks a launch lead; Sunita can call a formal
family meeting; Meera can accept Singapore; Kabir can decide whether to keep courting her. Different
*kinds* of power, not the same power renamed.

**2. Closure** — this authority genuinely ending, by turn 10 of the chapter. Once the company locks the
launch lead, Dev cannot threaten to reopen it every five episodes. Declaring it closed is not
closing it.

**3. Residue** — the conflict ended, the consequences stay. Aarav may permanently lose credit for the
launch, and that disappointment goes on colouring later scenes.

**4. Forbidden effects** — what can never happen again. Dev cannot discover a secret policy that lets
him remove Meera after all. Closed power cannot return through proxies, renamed rules or convenient
exceptions.

**5. Next source** — where the following conflict comes from, independently. Dev's authority ends, but
Sunita's exists whether or not Dev is ever mentioned again. The story never depends on one villain
manufacturing trouble.

**6. Objects and scene** — new in revision 8. The removable things this rung puts at risk, each named
and held by someone: three at minimum, four or five in a story that flips often. Plus, if you chose
"several people," the non-protagonist whose scene may open this chapter and what it shows. Before
this line existed, the runtime could be required to take an object the world had never created.

Mentally remove each earlier power: every later one must still stand on its own source, or be rebuilt.

---

# 12. Secrets, time and the opening

**Three secrets**, each with a holder, a plausible discoverer, and the names of everyone who knows.
Anyone not named there does not know, and the runtime enforces that from turn one.

**Time anchors** are historical events, in integer days, with turn 1 as day 0:

> - E1 = -210 | Aarav and Meera first met
> - E2 = -70 | the launch crisis
> - E3 = -21 | Aarav turned down Gurgaon

The leading `- ` is part of the format and is copied through to FILE 2 exactly.

This is what stops "we met six months ago" in episode 5 becoming "three years" in episode 28. Exact
days or whole weeks; **months and years are not allowed as elapsed spans**, and one gap keeps one
number for the whole turn.

**Appointments** are scheduled future events: `A1`, rehearsal on day 2, status pending / done /
cancelled. Every world now authors **one appointment before turn 1** — partly so time matters
immediately, partly because the runtime needs an appointment to exist before it can move one.

The **opening** stays wholly inside R1's arena, with the holder's authority in action, the advantage
physically nearby, and three executable choices.

---

# 13. The World Card, and the word budget

The card holds title and logline, premise, brake and fuse, endings, register and limits, cast,
pleasures and sanctuary, secrets, time anchors, all six rungs, and the opening. Every field is
numbered so you can edit it by name.

Two terms in that list are easy to misread:

* **Register** — the language and tone as they will actually be written: what Q2 and Q3 produce,
  stated once.
* **Family** — the storytelling family, as in §5. Not relatives.

The budget, and the important part is what kind of budget it is:

| Part | Aim |
|---|---|
| The world | 950–1,200 words |
| PROFILE | 100 words |
| The runtime, copied verbatim into every story | ~2,250 words |
| **FILE 2 total** | **aim 3,500 · hard stop 3,850 · 3,547 at worst case** |

**These are aims, not gates.** A scene, a chapter or a world running ten per cent long is not a
defect — in prose it is not even noticeable, and the handoff retired fixed scene length outright in
favour of purpose-sized units of 25 to 220 words. The runtime never pads a scene to reach a floor,
never cuts a beat that has already landed, and never counts its own words.

Only the total has a hard edge, and for a mechanical reason: FILE 2 is pasted into a chat once and
has to leave a 66-turn transcript room to grow. A hundred words either way changes nothing there; a
thousand does.

**Turn counts are the opposite.** Eleven turns a chapter and sixty-six a story are exact, because
closure, the fuse and the rounds are all counted in turns.

One lesson from four reviews is written into the spec itself: over-fitting these numbers is worse
than missing them. A scene trimmed to land inside a band, a block cut so a table adds up, a rule
compressed to buy margin — each spends something a reader would have noticed on an arithmetic
nobody reads. When a count and the story disagree, the count moves.

---

# 14. How a turn works

One message per turn. Five state lines, the scene, three choices, BOUND, any CARRY block, stop.

**RAN** — what you chose last turn, copied exactly, so the engine cannot loosely reinterpret it.

**POS** — where we are, with the rung's triple spelled out every turn:
`CH1 · T4/11 · RUNG R1 — the launch / Dev / who owns the client account`.

**LEDGER** — permanent memory. Append-only: new facts are added, never rewritten, reordered or
compressed. Twenty-five episodes later the story still knows Aarav backed Meera publicly.

**TIME** — the day, and the arithmetic behind any elapsed span.

**CHECK** — the counters, and the `(last: …)` fields are not optional; they carry the previous turn's
hook and mover so the engine cannot repeat itself:
`fuse 3/6 · rounds 5/6 · hook: revelation (last: threat) · by: Sunita (last: Dev) · took: — ·
open: W2 CH2T7 · ending: ongoing · unmet: —`.

Then the scene, in the register you chose, moving one concrete conflict.

**The order inside a turn** is a sequence, not fixed positions: what just happened; what the hero
knows and wants; their chosen act, before any consequence of it; a motivated response from whoever it
lands on; the changed situation. Anything staged ahead of the act must have already happened. Position
rules collide with each other; a sequence composes.

Then three choices — different commitments, the third the most reversible — and `Or type what you do.`

Typing your own action is not decoration. An impossible attempt meets a concrete obstacle, changes
something, and consumes a full turn. The engine may never invent a disclosure, signature, gift,
promise or journey on your behalf.

---

# 15. A pantry, not a quota

This is the most important change of revision 7, and it came from the project's own archive: *a
varied pantry, not a quota for each chapter.*

Earlier drafts gave the runtime about nineteen obligations to discharge across eleven scenes. Each
rule was sensible alone; together they over-determined every chapter, and turn one ended up with five
jobs, two of which could not both happen.

Now each turn asks: **what could legitimately happen right now?**

A supporter moving on their want · a promise falling due · a pleasure landing · the holder pressing ·
a fuse event · a quiet beat.

Anything that fails on causality, knowledge, time, resources or authority is dropped. Of what
survives, the story stages the one **whose absence has cost the most** — longest waiting breaks a
tie, it does not win the argument, so a promise falling due outranks a quiet beat that has simply
been parked since chapter one. Nothing is scheduled; nothing is forgotten —
an unstaged candidate keeps waiting and grows more urgent. The failure record, `unmet:`, now logs
only what became *impossible*, never what simply was not chosen.

The chapter still has a shape: turn 1 answers your last act and stages the holder's power, another
person's want matters by turn 5, a turning point lands in turns 7–9, the rung closes by turn 10, and
turn 11 is aftermath. Those are the only scheduled turns, and a priority order settles any clash —
settled rights first, then your commitment, then location and time, then the candidates, then the
chapter's shape, then style.

---

# 16. BOUND: choices that keep their promise

Before you pick, the engine states one immediate consequence attached to each option.

> Challenge Dev publicly · Support Meera · Ask for a private review
>
> `BOUND — 1: KNOWN the room hears the claim · 2: TRUST Meera sees what he gave up · 3: CLOCK A1: day 2 -> day 3; the rehearsal moves`

The wire format is exact: number, colon, tag, then the consequence, separated by middle dots. A CLOCK
entry names the appointment and both days, and they have to match TIME before and after.

Pick 2, and the engine must deliver 2's consequence. It cannot swap in the more dramatic one from
option 1.

The four kinds:

* **OBJECT** — something actually held is lost, through a mechanism already established.
* **TRUST** — a relationship changes.
* **CLOCK** — an existing appointment's date moves. Not "we wait a bit."
* **KNOWN** — someone learns something.

Three different tags per turn. If three genuinely are not available, the turn uses the ones that are
— never fewer than two — and records `unmet: bound-tags`. It does not invent an appointment, and it
does not take your things to fill an empty slot. That was a real bug: with no appointment anywhere,
the old rule forced an object to be seized on all eleven turns of a chapter against a world that had
authored three.

---

# 17. What the engine is not allowed to forget

**`gone:`** — when something is visibly lost, it is recorded, and there is no whole, partial,
pronoun, delegated or equivalent-capability restoration. `took:` copies the object's text
character for character, apostrophes included, so a quiet substitution is visible.

**`knows:`** — when a named person learns something on screen. After that, each person may use only
their authored knowledge, what they were told, and what they witnessed in a rendered scene. Nothing
overheard offstage, nothing "obvious." What anyone in the world plainly holds — prices, public hours,
who runs a shop — needs no entry; that exemption is new, and without it the rule fired constantly.
An entry records what someone now treats as true, which need not be true: a lie records what they
were told.

**And, new in revision 8:** a choice may name only what a rendered scene has shown **the player** —
the hero's own secrets included. This is the defect the first play found: the game offered "Admit the
lie" for a lie it had kept inside the hero's head and never put on screen. Show it first, or offer
something else.

**`owed:` / `paid:` / `open:`** — every stated threat, offer or intention, and every question left
hanging, becomes an IOU with an id. Settling one is always a candidate and gets stronger with age;
by the end of the chapter after the one that opened it, it outranks new material. It can be settled
by happening, by being prevented on screen, or by events overtaking it — but never by silence, never
by an event manufactured to close it, and a settled id never reopens.

**`SPENT:` / `CLOSED:`** — a rung's power is finished, and what it cost is on the record:

> CLOSED: R1 — Meera owns the launch; Aarav permanently loses lead credit.

**`unmet:`** — honest failure. `unmet: turning-point` means the structural beat did not legitimately
happen, which is better than inventing an event the player never caused.

**CARRY** — at the end of every chapter, the engine reprints all live state word for word: the POS
triple, the day and unfinished appointments, the counters, the last hook, the last mover, the ending
token, and every `gone:`, `knows:`, unsettled `owed:`, `paid:`, `SPENT:` and `CLOSED:` entry. If it
cannot copy something the previous block listed, it writes `CARRY INCOMPLETE` and names it rather
than quietly dropping it.
It sits inside the turn now, before the stop, so it cannot be orphaned on a final turn that has no
BOUND.

That block is also the save file. `save` prints one on demand: paste FILE 2 plus that block into a
fresh chat and continue. What it lists is canon; anything outside it is gone rather than pending, so
a resumed story cannot deadlock waiting for a message that scrolled away.

**The one thing nothing fixes:** if the original FILE 2 scrolls out of the chat, you paste it again.
CARRY saves the game, not the world.

---

# 18. Meta: undo, skip, and marking its own homework

None of these consume a turn.

* Undo, restart, change my answer → *"That turn is taken; the story only moves forward."*
* Skip → *"Nothing skips; the next thing is the next thing you do."*
* Save → a CARRY block, printing and undoing nothing.

**And a rule added after the first play:** a question about the story is answered inside the story.
The engine never apologises for a turn, grades it, or rewrites it. Asked "what lie?", the runtime had
replied *"You're right, that choice was unfair,"* explained itself out of character, and rewrote an
earlier scene — three rules broken unprompted in one answer. If a choice named something you were
never shown, the engine shows it now and goes forward.

---

# 19. How it can end

**ongoing** — still running.

**early** — your chosen move genuinely ended the central story before turn 66, through the authored
early consequence, without anything becoming public. Full price still staged.

**exposed** — something important became publicly disclosed, with consequences. Terminal, before or
at 66.

**certified** — the complete success condition at turn 66: fuse 6/6 and rounds 6/6.

**refused** — every chance finished, but not enough fuse events completed. This is the incomplete
route, and it covers every incomplete count: a lapsed round, a rung that stalled. The engine is
forbidden from inventing progress to reach a nicer ending.

A terminal turn takes the hook `end`, and offers no choices.

The **ending move** itself takes one of three shapes, chosen when the world is authored:

* **REVERSAL** — stop using an advantage you relied on. *Aarav stops using closeness as leverage.*
* **CLAIM** — comply in form while withholding the substance someone wants.
* **BARGAIN** — invoke an existing binding obligation to negotiate something else.

---

# 20. The simplest mental model

**Layer 1 — creative intent.** A human says: *Indian slow-burn romance about love versus ambition.*

**Layer 2 — Forge.** Eleven questions, then: characters, desires, secrets, timeline, conflict ladder,
costs, pleasures, ending conditions.

**Layer 3 — runtime.** Scene by scene, holding state, memory, time, choices and consequences.

**Layer 4 — story.** The player sees only characters, dialogue, conflict and choices. Most of the
machinery stays underneath.

---

# 21. What revision 8 repaired

A third cold read of revision 7 found nineteen defects, and the first play found a twentieth. All
twenty are fixed. The ones worth knowing about, in plain terms:

* Turn 66 could be reached with **no ending the engine was allowed to print**. Every chapter now
  closes its round, so that state cannot occur.
* With no appointment in the world, BOUND was forced to **take one of your possessions every turn**.
  Fixed at both ends.
* The removable objects each chapter needs had **nowhere in the world file to be written down**. They
  have a line now.
* Putting a chance off until later counted as **refusing** it. It no longer does.
* Three sentences in the file were simply **not true** — about which settings sit alone, about
  nothing scheduling turns, and about how a turn opens. They now say what is true.
* The cast floor said five helpers when six chapters need six.
* Four terms the file used and never explained — *family*, *scored cold run*, *HARDER*, *mix my own*
  — are defined.
* A shopkeeper can know his own prices without a scene proving it.
* Refusing someone's request no longer eats one of your three choices.
* Plus the three from the play, in §17 and §18.

---

# 22. What is still open

Being honest about this is cheaper than being surprised later.

**Blocking**

* **The instruction book is fat.** FILE 1 is about 7,800 words, up from 3,133. Most of the growth is
  explanation of *why*, written into the file that tells the machine *what*. That exact mistake is
  the one the archive blames for every earlier failure. The cut is the next job — and it is a real
  constraint, unlike the word budgets, because nobody has to read FILE 2 but the machine.

**Evidence**

* **Zero plays of revision 8.** The play record is one screenshot of a single turn from an
  unidentified revision — enough to expose four faults, not enough to tell anyone how long the story
  held. The 33-turn run people cite is older and was a different, pre-Forge build.
* Two plays of the same premise with different answers, to see whether the two stories actually feel
  different. If they feel the same, the settings are not doing anything.
* One full 66-turn run. The longest run on record anywhere in the project is 33 turns, on an earlier
  build.

**Known unknowns, waiting on a play rather than an edit**

* Whether a bleak tone should weaken the pleasure candidate.
* Whether `by` — whose act moved the conflict — can be gamed by relabelling.
* Whether six ledger prefixes — `gone:`, `knows:`, `owed:`, `paid:`, `SPENT:`, `CLOSED:` — make the
  state block visibly bigger than the story.

**Bigger decisions, deliberately not taken**

* The six-rung ladder of independent authorities **is a genre**, not a neutral skeleton: it encodes
  institutional pressure on someone hiding an advantage, and it forces about eight named people into
  every world. A quiet story about three people cannot be built here. Whether the eleven questions
  should reach that layer is the open design decision.
* Choice count is still fixed at three. Relationships still have no running state — `TRUST` is a
  consequence tag, not a meter. `gone:` covers objects, not people or permissions. Endings are
  adjudicated only at turn 66.

---

# 23. Why this could matter

Any strong model writes a good romantic scene. The interesting claim is narrower: that a **persistent
causal structure**, written before any improvisation, makes a long story hold.

Instead of "generate episode 23," the runtime knows:

> We are in R3. Meera owns this decision. Dev's authority expired. Aarav lost lead credit. Three fuse
> events have happened. One chance remains. The Singapore answer must close this conflict. None of
> this can be silently rewritten.

Whether that produces a story anyone wants to read for 66 turns is unproven. It has never been run
end to end.

---

# 24. One line

**Forge turns a short premise plus eleven plain questions into a constrained story world — memory,
causality, a ladder of conflicts that genuinely close, choices that keep their promises and endings
that have to be earned — and the runtime plays that world for 66 turns.**
