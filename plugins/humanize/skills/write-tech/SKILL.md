---
name: write-tech
description: "For technical text an agent writes for a person to read later, with no chance to ask anything back: a technical spec, a spec-assessment question, a timeline note, a ticket comment, a PR description or the final report of a task. The reader is a working engineer who reads code and diagrams easily but bounces off walls of prose. Use whenever that text explains a code change, a technical finding, a decision or how a system works, or would otherwise come out as several paragraphs. When unsure whether a technical text counts, load it. Not for non-technical topics: use explain-the-world to explain those in the conversation. For answering a technical question in the conversation, where the reader can ask back, use explain-tech instead."
---

# Write tech

The reader is a working engineer. Code, config, and diagrams land instantly; long prose does not, in any language.
The failure mode to avoid is not "too technical". It is **too many words per idea**.
The reader wasn't there while you worked. They read this later and can't ask you anything, so the text has to make sense on its own.
The opposite failure is just as real: a text so compressed that the reader can't follow it. Spend more words to cover every step of the reasoning, but keep each idea short.

**This skill teaches by example.** The `✗ / ✓` pairs below are the specification; the prose is only there to label them. When a pair and a rule seem to disagree, copy the pair.

**The examples are in English. The text is not.** Write the text in the reader's language and dialect (see "Language"), and carry over the shape, not the words.

## The shape of every text

1. **The point, in one or two sentences.** No preamble, no restating the topic.
2. **The shape of it**, usually in two or three short sentences. Reach for a diagram, table, or bullets only when they pass the test under "Pick the right tool".
3. **A real example, inlined.** The actual snippet, command, or value, not a pointer to where it lives.

Default to the **full explanation**: everything the reader needs to understand it without asking twice. Only line-by-line detail can wait for the "For agents" section; the reasoning never does.

## Something new follows three beats

When the text introduces something the reader may not know, it runs in this order, in plain, colloquial prose, as if explaining it to a colleague over coffee.

1. **What it is, where it lives, and what it's there to solve.** One to three sentences that name the concept, place it in the system around it, and say the problem it answers, in plain words. The reader knows what they're about to look at, and why it matters, before any detail arrives. See "Place it in the whole first".
2. **A concrete case.** A situation the reader can picture where the thing does its job.
3. **The explanation.** How it works and how it fits with the rest, landing on the case they already have in their head. This is where the analogy, the mechanics, the snippet and the one trap go.

Each paragraph holds one idea and two or three sentences. No diagrams, tables, or identifiers in beats 1 and 2 unless one is the thing itself.

## Place it in the whole first

A piece explained on its own is a list of parts. The reader needs to know what it sits inside and what goes wrong without it, or the parts don't stick. So on the first pass of a new topic, before zooming into the piece, cover three things:

1. **The system around it.** What the surrounding thing is and does, in one sentence.
2. **How it works today.** The current behaviour, read from the real code (the base branch, the callers), never from memory.
3. **What hurts, or what we want.** The problem or goal that gives the piece a reason to exist.

Then the piece itself, landing on that problem. Keep the frame to two or three sentences, or a small diagram when the flow branches. It's a doorway, not a tour.

This applies to anything explained, not only code changes, such as a concept, a PR, a config, an error or an architecture choice. Skip it when the same document already set the frame above.

> ✗ *"Explain this PR."* → "It adds an `ImageSyncResultCode` enum, a `SaveImageSyncResults` job, an `image_sync_results` table and an admin block on the variant page."
>
> ✓ This app copies product images from Shopify to the store's inventory system. Today a job decides per variant whether to update, skip or leave the inventory image, but every decision only goes to the log, so staff can't tell why an image is wrong. This PR records one result per variant and shows it on the variant page in the Shopify admin.

See example 13 in `references/examples.md`.

## Explaining a code change, a finding, or a refactor

A one-line finding ("X is derivable state, drop it") is a conclusion, not an explanation. The reader can't act on it without the steps behind it. Walk them through it, one short bolded beat per step:

1. **How it was.** The old shape, and why it made sense at the time.
2. **How it is now.** The real snippet from today's code.
3. **Why it's a problem, or why it's redundant.** Use a small table of cases when it shows that the outcome never changes, or exactly when it breaks.
4. **What you verified.** Such as the callers you checked or the tests that touch it, so the reader knows the claim holds.
5. **The fix.** The concrete change, file by file, including the tests that need to change.
6. **Is it worth it.** Your position and the reason, such as the real risk of leaving it.

See example 12 in `references/examples.md`.

> ✗ *"How does the knowledge base work?"* → opens on the case with no idea of what's being explained.
>
> ✓ The knowledge base is the memory of each project. It holds everything we know about a client, so any person or any Claude session can look it up.
>
> It's six in the evening and someone asks why the site shows prices with tax included. You ask Claude, and it answers with the decision, who made it, and the minute of the meeting where it was said.
>
> Think of it as the filing room at an accounting firm. Each client has a folder, and someone at the door checks your badge. Concretely, it's a private git repo per project, reachable only through a server that first checks you're a member. Every write is a commit in your name.

The canonical shot, *"what's a hook?"*, answered whole:

> A hook is a script of yours that Claude Code runs before every tool. It's there to stop what you don't want to happen, without relying on the model to remember.
>
> Say Claude is about to run `npm test && git push`. Before it starts, the hook reads the command, sees the `&&` and stops it. You never even see the permission prompt.
>
> The hook gets what's about to run and decides with its exit code. 0 lets it through; 2 stops it and Claude gets the reason. The program runs it, not the model, so Claude can't skip it. This repo's hook stops it like this:
>
> ```python
> if "&&" in stripped or "||" in stripped:
>     print("BLOCKED_AND_OR")
> ```

What it is, case, mechanics, real snippet. No diagram, because the flow is a straight line.

## Length budget

The format of the document decides what it covers. A spec covers everything it needs to; a timeline note covers one thing. Cut words, never sections.

- **A paragraph:** one idea, in two or three sentences.
- **A sentence:** ~20 words on average, mixing short and long. If one needs a chain of comma-clauses, split it.
- **A bullet:** one line, since a bullet that wraps three times is a paragraph in disguise.

## Pick the right tool

| Question shape | Reach for |
|---|---|
| "How does X flow / what calls what / what happens when…" | ASCII or mermaid diagram (sequence, or box-and-arrow) |
| "Difference between X and Y", options, trade-offs, before/after | comparison table |
| "What is X" (a concept that's new to the reader) | the three beats: what it is, a concrete case, the explanation |
| "What does this code/config do" | the actual snippet, quoted from the file, annotated |
| "Why is it built this way" | the constraint that forced it, then the alternative that lost |

**Draw only when the picture carries something a sentence can't**, such as branches, loops, or three or more parts talking to each other. A straight line of two or three steps, a single fact, or a short list of states reads better as a sentence.

**Never two visuals for the same idea.** If a table already shows the states, the state diagram underneath it is the same information twice. Pick one, or say it in a sentence.

> ✗ A table of the three states, then an ASCII state machine of the same three states.
>
> ✓ The table, then one line: "Then it moves to `processed`, which is final, or goes through `deferred` first if someone postpones it."

## Evidence goes inline, not as an address

Read the file before explaining it, and never describe this repo from memory. Then **paste what it said**. A `path:line` is an optional footnote, never the evidence. A path, a class or a ticket number can appear, after the sentence that says what it is, and it never replaces what the file says.

> ✗ The validator is out of date and rejects those keys (SKILL.md:76), see also the gate in submit-package.ts:266.
>
> ✓ The bundled validator flags `runtime` and `tier` as unknown keys. It's a false positive, so keep them top-level anyway.

> ✗ The hook blocks compounds (pre-bash-no-compound.sh:81).
>
> ✓ It blocks on one line: `if "&&" in stripped or "||" in stripped`. Quotes get stripped first, so `echo "a && b"` survives.

Never let a guess wear the costume of a fact: **fact** = read it just now · **inference** = say "this implies" · **guess** = say "guessing", or go read the file.

## One thread, in order

> ✗ Auth uses the token from the config, which is also read by the sync job (see below), and note that the cache path differs, but first, some background on…
>
> ✓ Auth reads the token from `config.yaml`. Nothing else touches it.

Caveats come *after* the happy path as their own short beat, never before it.

> ✗ Careful: this fails if the theme isn't deployed. Anyway, the command is `shopify theme push`.
>
> ✓ The command is `shopify theme push`. One trap: it silently no-ops if no dev theme exists yet.

Two things that both matter = a table with two rows, not prose cross-referencing itself.

## Register: plain talk, zero assumed context

Talk like a colleague explaining over coffee, not like a manual. Assume the reader knows engineering but nothing about what surrounds this concept: no other components, no project vocabulary, no history.

- Names of neighbouring components and invariant numbers come after the context, with five plain words saying what they are the first time.
- Use the reader's own everyday register and dialect, the words they'd use out loud.
- Warm and friendly. A light joke or an aside is fine when it helps something stick. No sarcasm, and never at the reader's expense.

| ✗ | ✓ |
|---|---|
| Because the spec is the contract (INV-7) and the coding agent can't edit it. | The spec is the list of what the code has to meet. If the same robot that writes the code could change that list, it would always pass its own exam. So it can't touch it. |
| The hook intercepts the tool invocation and evaluates its permissibility. | The hook steps in before the command runs and decides whether it goes through. |
| Exit 1 means "I blocked it" while the command runs anyway. Confident, and wrong. | Careful, with exit 1 the command still runs. Blocking it takes exit 2. |

## Sound like a person

Correct and clean is not enough. Text can be both and still read as generated. The reader should feel a colleague wrote it.

### Take a position, and weight things unevenly

Not everything matters equally, so don't present it that way. Say which part matters, what trips people up, and what to avoid. Every opinion rests on a fact you read.

> ✗ The command has three flags, `--dry-run`, `--force` and `--profile`. Each one changes the behaviour in a different way.
>
> ✓ Of the three flags, the one you need to care about is `--profile`. Without it, `aws` says there are no credentials and you lose ten minutes chasing a problem that doesn't exist. The other two do what they say.

> ✗ Knowledge-base writes can target any project the user has access to.
>
> ✓ Don't test against a real client project, not even from dev. Dev and prod share the same data. That's what the `sandbox` project is for.

> ✗ The dedupe compares the hash declared by the sender.
>
> ✓ This part confuses everyone the first time. The dedupe doesn't look at the file that arrived. It looks at the hash the sender declared.

Clean text can still have nobody in it. Leave a person in the text, such as an opinion on what matters, one honest doubt, or something you checked and what it showed. Each one rests on something you read, never on something invented. See example 14 in `references/examples.md`.

### One analogy per new concept

When the concept is abstract and new, give one analogy from the reader's world, then drop it. Don't stretch it across paragraphs. Turn big numbers into something you can picture.

> ✗ The knowledge base is a per-client repository behind a server that validates membership.
>
> ✓ The knowledge base is like the filing room at an accounting firm. Each client has a folder, and someone at the door checks your badge before you can take one out.

> ✗ The argument limit is around 26,000 characters.
>
> ✓ The limit is around 26,000 characters, roughly ten pages of text. A SKILL.md fits easily; a base64 zip doesn't.

### The first words carry the idea

People scan. They read the first line, then the start of each paragraph. Someone who reads only the first sentence of each paragraph should still get the answer.

> ✗ There are several reasons this happens, and they're worth going over. The main one is that the token expires after an hour.
>
> ✓ The token expires after an hour. That's why the second call fails even though the first one worked.

### Vary the rhythm

Mix short sentences with long ones. A short one after a long one lands the point. Five sentences of the same length read as generated.

> ✗ The hook reads the command before it runs. Then it strips the quoted parts. Next it looks for forbidden operators. Finally it decides with the exit code.
>
> ✓ The hook reads the command before it runs, strips everything in quotes and looks for a `&&`, a `||` or a stray `;`. If it finds one, it stops it.

Vary how a case opens, too. "Imagine that…" works once, and repeated it becomes a tic.

| Opening | Example |
|---|---|
| Straight into the scene | It's six in the evening and the client just sent a two-hour transcript. |
| Something that happened | On Friday `main` went red because of a stale index. |
| The reader's own experience | Ever approved a PR without fully understanding what it changed? |
| "Imagine that…" | At most once per text. |

### Machine tells

Each of these is a sign of generated text. Cut it on sight.

| ✗ | ✓ |
|---|---|
| "It's not a validator, it's a gatekeeper." | "It works like a gatekeeper." |
| An objection nobody raised: "You might think this is slow, but…" | *(cut it, unless the reader raised it)* |
| Forced triads: "fast, secure and reliable" | the one quality that is true and matters |
| A closing aphorism: "And that changes everything." | *(delete it and end on the last fact)* |
| Bold on every bullet label | bold only on the one thing that must not be missed |
| Describing the text: "In this explanation we'll look at…" | *(start at the content)* |
| Inflated words such as "crucial", "robust", "key", "powerful" | the concrete effect: "it doesn't fall over when Drive is down" |
| Agent verbs such as "the key rides the record" or "the facts they strand" | "the field travels inside the record", "the data that gets stuck" |
| Stacked hedges: "could potentially fail in some cases" | "can", or one honest doubt tied to what you checked: "I didn't try it with files over 10 MB" |
| A row of fragments: "Fast. Simple. Done." | one sentence with the specific claim |
| A one-line closer: "And that matters." | *(delete it and end on the last fact)* |
| An analogy that needs two worlds: "It's the Excel of agents." | an analogy from one world the reader already knows |
| A round range: "5 to 10 minutes" | the number you measured, or "I didn't time it" |
| "serves as", "plays a role in", "is responsible for" | "is", "has", or the verb itself: "it validates" |

### Before sending

1. Reread and mark every tell from the table above.
2. Read only the first sentence of each paragraph. It should still say the answer.
3. Look at sentence lengths. If they are all alike, split one or join two.
4. Check that the rewrite added no fact, name or number you did not read.
5. Look for a person in it, such as an opinion, a doubt, or something you checked. If there's none, the text reads like a manual.

## Questions and notes that stand alone

A spec-assessment question or a timeline note is often read on its own, without the spec open. So it names the part of the feature and the decision first, in plain words. Class names, files and step numbers come after, and only if they help.

> ✗ In spec_draft.blade.php, where and how does it say that the rule in SpecRubric::definition() ("no TBD, no A or B") doesn't apply to the pending decisions that step 3 asks to write down with their options?
>
> ✓ The prompt the runner gets to draft a spec has two instructions that clash. One says a spec never leaves a "TBD" or an "A or B". The other, step 3, asks to write each pending decision with its options. Where does the prompt say the first rule doesn't cover those decisions? (The rule lives in `SpecRubric::definition()`; the prompt is `spec_draft.blade.php`.)

## Language

Every rule in this skill applies in any language. Write in the language the ticket or the project is written in and **stay in it**.

In a non-English text, an English word survives only if it is an identifier, path, command or flag (`submit-package.ts`, `--profile`), a product name (Claude Code, Shopify), a term the industry genuinely doesn't translate (`commit`, `merge`, `deploy`, `hook`, `spec`, `pull request`), or a name coined by this project (`kickback`, `build loop`). Everything else goes to the reader's language.

Spanish shows the failure most often. These pairs stay in Spanish because the failure is Spanish:

| ✗ | ✓ |
|---|---|
| el ceiling inline | el límite / el techo |
| los tool arguments emitidos | los argumentos que emite la herramienta |
| se truncan silently | se cortan en silencio |
| corrompe el payload | corrompe el contenido |
| un signal tipado | una señal con tipo declarado |
| te nombra la output | te nombra la salida |
| el validador bundleado | el validador que viene incluido |
| zippeá el directorio | comprimí el directorio |

1. **No hybrid verbs or participles**, such as `zippear`, `bundleado`, `deployar`, `mergeado` or `parseá`. Use a native verb with the English noun if needed, as in *hacer un deploy*.
2. **One language per clause.** `el ceiling inline es real` blends grammar mid-phrase. Write a sentence in the reader's language that contains an identifier, or a sentence fully in English.

The test: read it out loud. If it sounds like an office where nobody finished translating the memo, rewrite it.

Each language also has its own machine fillers, and avoiding the English ones ("crucial", "it's worth noting") does nothing for these. Spanish shows them most, so this table stays in Spanish:

| ✗ | ✓ |
|---|---|
| cabe destacar que / es importante mencionar que | *(borrarlo y decir la cosa)* |
| en este sentido / por ende | entonces / así que |
| juega un rol fundamental en / se encarga de | el verbo directo: "valida", "frena" |
| aquí está la clave | *(borrarlo)* |
| en resumen, … | *(borrarlo y terminar en el último dato)* |

Casual markers such as "ojo" or "mirá" work inside a sentence ("ojo con el 1, porque…"), never as a standalone opener before a routine claim. See example 15 in `references/examples.md`.

## Banned

| ✗ | ✓ |
|---|---|
| `Great question! Let's dive into how this works.` | *(delete it and start at the point)* |
| a closing paragraph summarizing what's two inches above | *(delete it)* |
| `it may sometimes potentially depend on the config` | `it depends on whether strict is true` |
| `see the docs for details` *(when the answer is two lines)* | *(just write the two lines)* |

## What goes where

The essentials and the mechanics go in the body, in prose with rhythm. Detail that only an agent needs, such as lists of files, constants and exact steps, goes in the "For agents" section when the document has one, and there it can be dense.

## More shots

`references/examples.md` holds fifteen full before/after rewrites, longer than the pairs above. Three of them (citation soup, mixed languages, machine tells) are the failures this skill slips into most. Two more (a person in the text, Spanish fillers) show how clean text still sounds generated.

Read it when a text came out too long and needs cutting, the right tool for a topic isn't obvious, or you're unsure how a rewrite should read.

Each correction from a reader becomes a new pair in that file, written in English, which is how this skill improves. When a correction changes a rule, update the rule here and fix every existing example and ✓ that now breaks it, since the model copies the examples over the rules.
