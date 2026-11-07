# Examples

Before/after rewrites. The "before" answers are all technically correct. They fail on shape, not accuracy. They are written in English so the shape is easy to copy, and the real text goes in the reader's language. The "Q" of each example is the topic the text has to explain; in the runner it comes from the ticket, not from a chat. The exceptions are examples 6 and 15, which stay in Spanish because the failures they show are Spanish.

Contents:
1. New concept → what it is, a case, then the mechanics
2. Flow question → short intro, then the diagram
3. "What does this code do" → quote the lines that matter
4. Project vocabulary → the idea, with zero outside context
5. Citation soup → inline the finding
6. Mixed languages → one language that happens to contain identifiers
7. Too many visuals → prose where the picture adds nothing
8. A flat list of facts → a position on what matters
9. Project definition → one analogy, then the mechanics
10. Machine tells → the same facts, said by a person
11. A case with no frame → what it is first, then the case, then the explanation
12. A compressed finding → the full walk-through: before, now, why, verified, fix, worth it
13. A piece with no whole → the system, how it works today, what hurts, then the piece
14. Clean but nobody in it → the same facts, with a person who read them
15. Spanish fillers → the same facts, said out loud

---


## 1. New concept → what it is, a case, then the mechanics

**Q:** "What's a kickback?"

**Before (jargon definition, project vocabulary):**
> A kickback is when a `reject_only` agent (QA or tech lead) returns the ticket to the coder with a structured failure report. There are two independent counters, and the third one escalates to a human (INV-4).

**After:**
> A kickback is when a reviewer hands a task back to the robot that did it, because something doesn't meet the spec. It's the only way a reviewer can say no.
>
> Say the robot that writes code finishes a task and another one reviews it. It finds that a button doesn't do what the spec asked. It doesn't fix it itself. It hands it back to the first one with a note on what failed.
>
> Two reviewers can do this, the one that tests the result and the one that reads the code, and each keeps its own count. If the same reviewer hands it back three times, the loop stops and a person looks at it. That way two robots don't bounce a ticket forever.

Note: the "before" is one sentence a newcomer can't parse without four other concepts. The "after" says what it is in plain words, builds the picture, and only then gives the rules.

---


## 2. Flow question → short intro, then the diagram

**Q:** "How does a ticket get from spec to merge?"

**Before:** four paragraphs naming agents in sequence.

**After:**
> A ticket starts with a spec a person approved. A robot writes the code, and then it goes through two reviewers. Either one can hand it back, and if both let it through, a person decides whether it goes in.
>
> ```
> approved spec ──▶ coder ──▶ QA ──┬─ passes ─▶ tech lead ──┬─ passes ─▶ person ─▶ merge
>                    ▲              │                        │
>                    └─ hands back ─┘                        │
>                    └────────────── hands back ─────────────┘
> ```
>
> No reviewer can approve, only hand back. The only "yes" in the drawing is the person's.

Note: the loop-back arrows are the whole point, and prose hides them. The intro still comes first, so the diagram lands on a picture the reader already has.

---


## 3. "What does this code do" → quote the lines that matter

**Q:** "What exactly does the bash hook check?"

**Rule:** read the file first. Then quote the two lines that matter, not all 200.

**After:**
> First it strips everything in quotes and the here-docs out of the command. Then it stops four shapes, which are `&&`, `||`, a stray `;`, and several commands separated by newlines.
>
> The `&&` case is two lines, in `.claude/hooks/pre-bash-no-compound.sh`:
>
> ```python
> if "&&" in stripped or "||" in stripped:
>     print("BLOCKED_AND_OR")
> ```
>
> Since the quotes are already gone, `echo "a && b"` isn't stopped. Neither are pipes (`grep x | wc -l`) or the `;` inside shell structures (`; do`, `; then`, `; fi`).
>
> One trap. It blocks with **exit 2**, not 1. With exit 1, Claude Code treats it as a non-blocking error and the command runs anyway.

---


## 4. Project vocabulary → the idea, with zero outside context

**Q:** "Why can't the build loop edit the specs?"

**Before:**
> Because the spec **is** the contract (INV-7). If the loop that builds can also rewrite what it had to meet, the contract measures the code against itself, and it always passes.

**After:**
> The spec is the list of what the code has to meet. If the same robot that writes the code could change that list, it would always pass its own exam. So it can't touch it.
>
> If the list is wrong, the robot stops and says so. A person fixes it.

Note: the "before" leaned on `INV-7` and "contract", which only make sense if you already know the project. The "after" explains the idea with zero outside context.

---


## 5. Citation soup → inline the finding

Both paragraphs are accurate. Both are unreadable.

**Before (soup + fan-out):**
> The "package" is literally the skill directory zipped. `package_skill.py` is an accelerator, not a requirement (SKILL.md:74): it produces a `.skill` that's the same zip with another extension, and it depends on PyYAML; if it doesn't run, zip the directory and move on.
>
> Watch out for the trap documented in SKILL.md:76: the bundled validator (`quick_validate.py`) is out of date and complains about `runtime`/`emits`/`reads`/`tier` as "unknown frontmatter keys". Ignore that specific complaint and keep them top-level; the library's real pre-screen requires them there, and nesting them under `metadata:` fails the real gate.

What broke:
- three `SKILL.md:NN` pointers acting as credentials for claims that needed none
- "the trap documented in" makes the reader go read a doc to trust the sentence
- the caveat arrives before the happy path, so nothing is solid yet when the exception lands
- two systems cross-referenced (the bundled validator *and* the library gate) in one breath

**After (linear, inline, zero pointers):**
> A `.skill` is the zipped directory, nothing more. If `package_skill.py` won't run because PyYAML is missing, zip the folder by hand and you're done. It's the same file.
>
> One trap. The bundled validator will flag `runtime`, `emits`, `reads` and `tier` as *unknown frontmatter keys*. It's a false positive, because the validator is out of date. Keep them at the top level:
>
> ```yaml
> name: my-skill
> runtime: ...
> tier: ...
> ```
>
> If you nest them under `metadata:` to silence the warning, you pass the local validator and fail the library's real check. Ignore the complaint.

Same facts, one thread, nothing to go look up. The `yaml` block does the work three pointers were doing.

---


## 6. Mixed languages → one language that happens to contain identifiers

This one stays in Spanish, because the failure is Spanish. Accurate, and exhausting to read.

**Before:**
> El ceiling inline es real y con razón: los tool arguments emitidos por el modelo se truncan en silencio ~26.000 chars, corrompiendo el payload sin error (submit-package.ts:266-272). Por eso el server responde con un signal tipado (`package_too_large_inline`) que te nombra la salida, no un error de tamaño pelado.

Count the untranslated words that had a perfectly good Spanish version: *ceiling, inline, tool arguments, chars, payload, server, signal tipado*. None of them is an identifier. Each one makes the reader switch languages mid-sentence.

**After:**
> El límite es real, y tiene sentido. Los argumentos que emite el modelo se cortan solos cerca de los 26.000 caracteres, sin avisar. El contenido llega roto y nadie tira un error.
>
> Por eso el servidor no devuelve un "error de tamaño" pelado. Devuelve una señal con nombre propio, `package_too_large_inline`, que ya te dice cuál es la salida.

What survived in English: `package_too_large_inline`. That's it, and it survived because it's a literal string in the response.

Note the second fix too. The "before" is one dense sentence carrying three ideas (the truncation, the silence, the corruption). The "after" gives each one its own short sentence, in the order they happen.

---


## 7. Too many visuals → prose where the picture adds nothing

The reader's correction (translated): "you make lots of diagrams, for things where they sometimes don't make much sense."

**Before** (the tail of an answer on document ingest, after a big flow diagram and a states table):
> After it lands, each `.ref` moves through these states:
>
> ```
> unprocessed ──you confirm──▶ pending ──▶ processed  (final)
>       └───────────┬────────────┘
>                   └──▶ deferred (to whom and why) ──▶ processed
> ```

Three visuals in one answer, and the third repeats states the table had just listed. The only new information is two transitions.

**After:**
> Then any of those states ends in `processed`, which is final. If someone postpones it, it goes through `deferred` first, with a note on whose turn it is and why.

The flow diagram earned its place, because two paths merge into one landing step. The state machine didn't, since a sentence says it in fewer lines.

---


## 8. A flat list of facts → a position on what matters

**Q:** "What do I need to know before running the checks script?"

**Before (every fact weighs the same):**
> There are several aspects to consider when running the checks script. The script supports different suites. The `push` suite is a fast subset. Running without arguments runs `main`'s full suite. Additionally, GNU tar is required on the PATH. Finally, the push hook validates a receipt.

**After:**
> The one thing to remember is `--suite push`:
>
> ```
> node scripts/checks.mjs --suite push
> ```
>
> Run it without the suite and you get `main`'s full battery. It takes much longer, it needs AWS access, and on top of that the push hook won't accept that receipt. It's the most common mistake.
>
> One more trap on a Mac. It needs GNU tar on the PATH, because it fails with the tar macOS ships.

Note: the "before" is a correct inventory. The "after" picks the one thing that bites, says why it bites, and leaves the rest as a single caveat.

---


## 9. Project definition → one analogy, then the mechanics

**Q:** "What's a lane claim?"

**Before:**
> A lane claim is a row in `wiki-work-in-progress.md` with owner, scope and `Expires`, reserving a server tree to prevent concurrent writers.

**After:**
> A lane claim is a notice that you're working on one of the repo's servers. It's there so two people don't edit the same code at the same time.
>
> It's like leaving your towel on a beach chair. Nobody stops you from sitting down, but you see it's taken and you ask first.
>
> In practice it's a row on a page in the project wiki, with your name, what you're touching and when it expires. It expires after 24 hours unless you renew it. If you find an expired one, don't delete it. Ask whoever put it there whether they're still on it.

Note: the analogy gets one sentence and is then dropped. The mechanics land on a picture the reader already has, and the last sentence is the one thing people get wrong.

---


## 10. Machine tells → the same facts, said by a person

**Before (correct, and obviously generated):**
> **What changes:** The dedupe system is now more robust. It's not a simple hash check, it's a verification against the declared hash. This guarantees consistency, traceability and reliability. You might think this adds latency, but the impact is minimal. Ultimately, a key step toward a more solid ingest.

What gives it away:
- bold on a label that needed no emphasis
- "more robust" and "key step", inflated words with no concrete effect
- "It's not X, it's Y"
- a forced triad, "consistency, traceability and reliability"
- an objection nobody raised, about latency
- a closing aphorism that says nothing new
- five sentences of almost the same length

**After:**
> Before, if the same document arrived twice by different routes, it went in twice. Now the dedupe compares the hash the sender declares, so the second copy is recognized and stays out.

Two sentences carry everything the five did. Nothing was added, and what was cut had no information in it.

---


## 11. A case with no frame → what it is first, then the case, then the explanation

The reader's correction (translated), after the "before" below: "before the example, I want you to give an introduction to what the concept is, then the example, then the full explanation."

**Q:** The topic to explain: how the project knowledge base works.

**Before (opens on the case):**
> It's six in the evening and someone on the team asks why the site shows prices with tax included. You ask Claude, and it answers with the decision, who made it, and the exact minute of the recording.
>
> That comes from the knowledge base. Think of it as the filing room at an accounting firm…

The case is good, but the reader spends the first paragraph without knowing what is being explained.

**After:**
> The knowledge base is the memory of each project. It holds everything we know about a client, so any person or any Claude session can look it up.
>
> It's six in the evening and someone asks why the site shows prices with tax included. You ask Claude, and it answers with the decision, who made it and, if it came out of a meeting, the exact minute of the recording.
>
> Think of it as the filing room at an accounting firm, where each client has a folder and someone at the door checks your badge. Concretely, it's a private git repo per project, reachable only through a server that first checks you're a member.
>
> There are no copies of Jira or Drive inside, since those stay the owners of their own content. What's there is the project wiki, the signed scope, the decisions and pointers to those documents. Decisions are never deleted. When a new one replaces another, the old one is marked as expired, so you can always tell what we believed in March.
>
> The server never calls a model. Your Claude session brings the intelligence, and the server only stores. Every write is a commit in your name.
>
> One trap. Dev and prod share the same data, so use the `sandbox` project for testing.

---


## 12. A compressed finding → the full walk-through: before, now, why, verified, fix, worth it

The reader's correction (translated), after the "before" below: "tell me more, I don't get it, explain a bit more in the description." After reading the "after": "this kind of longer, more explanatory description is what I want the teacher skill to do."

**Q:** The topic to explain: findings 7 and 8 *(from a code review of a REST → GraphQL migration)*

**Before (a conclusion with no steps):**
> The adapter builds a fake `discount_applications` only so a check in `SaleService` passes. `collect($order->discount_codes)` is enough, and both can go.

Every word is true, and the reader still can't tell why the field exists, why it's fake, or why removing it is safe.

**After:**
> Findings 7 and 8 are the same thing seen from two files. It's a leftover from the REST to GraphQL migration: a field we made up so old code kept working without being touched. It breaks nothing, but it's fake data nobody needs.
>
> **The case.** A customer checks out with the code `SUMMER10`. When the order webhook arrives, or the hourly `shopify:pull-sales` runs, the app has to record in `discounted_sales` how much that code took off.
>
> **How it was with REST.** Shopify sent the order with two lists. `discount_applications` held *every* discount (codes, manual and automatic), each with a `type`. `discount_codes` held only the codes and each one's amount. `SaleService` first checked that some discount was a code, and only then read the codes:
>
> ```php
> $hasDiscountCodes = $discountApplications->contains(fn($d) => $d->type === 'discount_code');
> $discountCodes = $hasDiscountCodes ? collect($order->discount_codes) : collect();
> ```
>
> **What happens now with GraphQL.** Shopify no longer sends those two lists ready-made. `toSaleOrder()` in the adapter builds them, and both come from the same `$codes` variable:
>
> ```php
> 'discount_applications' => $codes->map(fn () => (object) ['type' => 'discount_code'])->all(),
> 'discount_codes'        => $codes->map(fn ($code) => (object) ['code' => $code, 'amount' => ...])->all(),
> ```
>
> Look at the first line. For each code it creates an object that only says `type: discount_code`, with no other data. It's a blank sheet with a stamp on it, there only so the check passes.
>
> **Why it's redundant.** Both lists come from `$codes`, so they always agree:
>
> | Order | `discount_applications` | `discount_codes` | Result |
> |---|---|---|---|
> | With `SUMMER10` | 1 fake object | `SUMMER10` | saves `SUMMER10` |
> | No codes | empty | empty | saves nothing |
>
> The check never changes the result. I verified that both paths into `processSale`, the webhook and the hourly command, go through `toSaleOrder()`. No REST-shaped order is left.
>
> **The fix** is two small changes. In `SaleService`, reading the codes becomes `$discountCodes = collect($order->discount_codes);`. In the adapter, the `discount_applications` line goes. `SaleServiceTest` also builds and asserts that fake field in three places, so those go too.
>
> Is it worth it? Yes, since it's small and low risk. The real danger is that six months from now someone opens `discount_applications`, thinks it's Shopify data, and builds on a field that only holds stamps.

What changed: the same facts, plus the steps between them. It explains how it was, how it is now, why it's redundant (one table), what was checked, the fix file by file, and a position on whether it's worth it. Each paragraph still carries one idea.

---


## 13. A piece with no whole → the system, how it works today, what hurts, then the piece

The reader's correction (translated): "when I ask you to explain something, also consider some context to understand the problem within the whole, and what we want to solve."

**Q:** The topic to explain: this PR *(a PR that adds image sync results to a Shopify ↔ inventory-system bridge)*

**Before (the parts, with nothing to hang them on):**
> The PR adds an `ImageSyncResultCode` enum with six cases, refactors `UpdateProduct` to return a code per variant, adds a `SaveImageSyncResults` job that upserts into a new `image_sync_results` table, a `GET /api/admin/inventory/variants/{id}/image-sync-result` endpoint, and an admin block extension on the variant page.

Every item is accurate. The reader still doesn't know what system this is, what the job did before, or why anyone needed a table of results.

**After:**
> This repo is a bridge between the store's inventory system and Shopify. Product images flow one way: when a product changes in Shopify, a job copies each variant's image URL into the inventory system.
>
> Today that job makes a decision per variant, such as update it, skip it because the image is the same, or leave it because someone set a non-Shopify image by hand. Every decision only goes to the log, and variants missing from the inventory system vanish without a trace:
>
> ```php
> logger()->debug('Skipping variant: non-Shopify image locked', ['upc' => $variant->upc]);
> return;
> ```
>
> So when an inventory image is wrong, staff ask a developer to dig through logs. The PR fixes that. Each `return` now hands back a result code, the job saves one row per variant at the end, and a card on the variant page in the Shopify admin shows the latest one.
>
> One trap explains a design choice. Saving the result as a Shopify metafield would fire `products/update`, which queues the same job again, so the results live in the app's own table.

What changed: the same parts, after three sentences of frame (the system, how it works today, what hurts). Each part now lands as the answer to a problem the reader already holds.

---


## 14. Clean but nobody in it → the same facts, with a person who read them

Example 10 removes the tells. This one starts where 10 ends: text with no tells left that still reads like a manual.

**Before (clean, and nobody wrote it):**
> The sync job sends images in batches of 50. It retries failed uploads three times. Errors are logged to the sync channel.

Nothing here is wrong or generated-looking. Every sentence has the same weight, so the reader can't tell what matters.

**After:**
> The sync job sends images in batches of 50 and retries each failure three times. The part I'd watch is the log. Errors go to the sync channel, and that channel is so noisy nobody reads it. In last week's, two real failures were buried under 400 warnings.

What changed: an opinion on what matters, and the evidence behind it. The voice comes from something read, the channel they opened. Nothing in it is invented.

---


## 15. Spanish fillers → the same facts, said out loud

This one stays in Spanish because the fillers are Spanish. Avoiding "crucial" and "it's worth noting" doesn't touch them.

**Antes (correcto, y nadie habla así):**
> Cabe destacar que el hook juega un rol fundamental en la seguridad del flujo. En este sentido, se encarga de validar cada comando antes de su ejecución. Por ende, es importante mencionar que un exit code 2 bloquea la acción. Por otro lado, cabe señalar que el exit code 1 no bloquea la ejecución del comando. En resumen, es una pieza clave del sistema.

What gives it away:
- "cabe destacar", "es importante mencionar", announcing a point instead of making it
- "en este sentido", "por ende", "por otro lado", connectors nobody says out loud
- "juega un rol fundamental", "se encarga de", long phrases where a verb does it
- "en resumen", a closer that repeats what's two lines above

**Después:**
> El hook revisa cada comando antes de que corra. Si devuelve 2, lo frena y Claude ni lo intenta. Ojo con el 1, porque parece un bloqueo y el comando corre igual.

Three sentences carry everything the five did. The exit 1 line also gained a position: it's the trap, so it gets an "ojo" inside the sentence instead of a "cabe señalar" in front of it.
