---
name: write-for-users
description: "For written documentation about a product or a feature, for the people who use it and never see the code: an end-user guide, a help page, a how-to, a FAQ, a troubleshooting page, a 'what's new' note, or a manual for staff, clients or operators. The reader is not an engineer. Use whenever the text tells a user how to do something in the product, what a screen or a message means, or what changed for them, such as \"write the user guide for X\", \"document this feature for the client\", \"write the help page for X\", \"explain to the store staff how to X\", \"write the release notes for users\". When unsure whether the reader is a user or a developer, load it. For text whose reader is an engineer (a PR, a spec, a ticket, developer docs), use write-tech instead."
---

# Write for users

The reader uses the product and never sees the code. They came to the page to get something done, usually in the middle of their work, and they'll leave the moment they have it.

There are four failures to avoid:

1. **The developer's manual.** Class names, endpoints, jobs and tables. The reader doesn't know any of them and doesn't need to.
2. **The brochure.** "Powerful", "seamless", "intuitive". The reader already has the product; they need to use it.
3. **The wall.** Steps buried inside paragraphs, so the reader loses their place every time they look back at the screen.
4. **The guess.** A button label, a delay or a limit described from memory. The reader follows it to the letter, and it isn't there.

**This skill teaches by example.** The `✗ / ✓` pairs below are the specification; the prose is only there to label them. When a pair and a rule seem to disagree, copy the pair.

**The examples are in English. The text is not.** Write in the language of the product's users (see "Language"), and carry over the shape, not the words.

## Read the real feature first

A user guide is only as good as its labels. Before writing, read what the user will actually see:

- **The screen texts**, exactly as they appear: button labels, menu items, field names, messages. They're usually in the templates, the components or the translation files.
- **The behaviour** the user will notice: what happens after each action, how long it takes, what the limits are. Read them from the code or the config.
- **Who can do it**: roles, permissions, plan, or anything that must be set up first.

Never describe a screen from memory. If you couldn't find a label or a number, write your best version and mark it `[to confirm]`, so it doesn't reach the reader as a fact.

> ✗ Click **Save changes** and the image will update immediately.
>
> ✓ Click **Save**. The new image shows up in the store within 15 minutes.
>
> *(The button says "Save", and the sync runs every 15 minutes. Both were read from the code.)*

## The shape of a page

Every page answers one task or one question. Its parts, in order:

1. **A title that names the task**, the way the user would say it: "Change a product's image", not "Image management".
2. **What it's for, in one or two sentences**, and when you'd need it.
3. **Before you start**, only if something is needed: a permission, a setting, a file at hand.
4. **The steps.**
5. **What you should see** when it worked.
6. **If something goes wrong**: the common problems, each with what to do.

| Kind of page | What it holds |
|---|---|
| **How-to** ("Change a product's image") | the six parts above |
| **Concept** ("What is a variant?") | what it is in the user's words, one example from their own work, and what it changes for them. No steps. |
| **Troubleshooting** ("The image didn't update") | the symptom as the user sees it, the likely causes from most to least common, and what to do for each |
| **What's new** | what the user can now do, where to find it, and what changed in how they work. Not how it was built. |

Split a page that tries to answer two tasks. Two short pages beat one page where the reader has to find their half.

## Steps

The reader reads one step, looks at the screen, does it, and comes back. Each step has to survive that trip.

- **One action per step.** If a step has an "and then", it's two steps.
- **Where first, then what.** The reader needs to find the place before they can act on it.
- **Labels exactly as on screen, in bold.** Same capitalisation, same words.
- **The result, when it isn't obvious.** A short sentence after the action: what opens, what changes.
- **Conditions first.** "If you don't see the button, …" goes before the action, not after it.
- **Seven steps or fewer.** More than that is usually two tasks.

> ✗ To change the image you need to go to the product, and once there you'll find the variants section where you can choose the variant and upload a new image, after which you should save.
>
> ✓
> 1. In the side menu, click **Products**.
> 2. Open the product you want to change.
> 3. In **Variants**, click the variant.
> 4. Under **Image**, click **Replace** and choose the new file.
> 5. Click **Save**.
>
> The variant page shows the new image right away. The store shows it within 15 minutes.

## The user's words, not the system's

Name things the way the screen names them, and describe what happens the way the user sees it. Internal names never reach the page.

| ✗ the system's view | ✓ the user's view |
|---|---|
| The `SyncImages` job runs every 15 minutes. | Changes reach the store within 15 minutes. |
| The request returns a 422. | You'll see the message "The file is too large". |
| The webhook triggers a re-index. | Search results update a few minutes after you save. |
| Set the `auto_publish` flag to true. | Turn on **Publish automatically**. |
| The record is soft-deleted. | The product goes to **Trash**, and you can restore it for 30 days. |

Explain *why* only when it changes what the user does. "It can take up to 15 minutes, because the store updates in batches" is useful: it tells them not to save twice. How the batches work is not.

## Messages and errors

When the user sees a message, quote it exactly, say what it means in plain words, and say what to do. If there's nothing they can do, say who to contact.

> ✗ An error may occur if the file is invalid.
>
> ✓ **"The file is too large"**: the image is over 5 MB. Make it smaller, or save it as JPG, and try again.

## Screenshots

Mark where a screenshot would help, with what it should show. Never invent one.

```
[Screenshot: the variant page, with the Image section and the Replace button highlighted]
```

Put one where the reader is most likely to get lost, usually the first screen of the task. A screenshot of every step is a slideshow nobody maintains.

## Register

Talk to the reader directly, in the second person, like a colleague showing them how it's done. Warm, short and specific. No jokes about errors: the reader who opened that page is already annoyed.

- **Neutral language, with no region.** A manual is read in many countries, so it uses the neutral form of its language: no regional forms of address, no slang, no idioms. Use a regional variant only when the person who asks for the text says so. This rule is for the document only: your messages to the person who asked for it keep the register of the conversation.
- **Spanish: "tú", neutral.** "Haz clic", "abre", "puedes". Never "vos" ("hacé clic") and never "vosotros" ("haced clic"). Prefer words understood everywhere: "computadora o equipo", not only "ordenador" or only "compu".
- **English: plain international English.** "You", no idioms ("hit the ground running", "a piece of cake"), and one spelling, American or British, used consistently.
- **No "just", "simply" or "easily".** If it were easy for this reader, they wouldn't be reading. And if it doesn't work for them, those words make it their fault.
- **No brochure words.** "Powerful", "seamless", "intuitive", "robust", "game-changing". Say what it does.

| ✗ | ✓ |
|---|---|
| Simply click **Save** and you're done! | Click **Save**. |
| Our powerful new image tool makes managing your catalog seamless. | You can now replace a variant's image without leaving the product page. |
| Easily see why an image didn't update. | The variant page now shows why an image didn't update. |

## Facts you can stand behind

The reader follows the page to the letter, so every number has to be true.

- **Delays, limits and sizes**: read from the code or the config. If you can't find one, mark it `[to confirm]`.
- **What happens after an action**: only what you saw in the code or tested. Not what "should" happen.
- **Permissions and plans**: say who can do it, if not everyone can.

## Machine tells

Each of these is a sign of generated text. Cut it on sight.

| ✗ | ✓ |
|---|---|
| Describing the page: "In this guide we'll walk you through…" | *(start at the first useful sentence)* |
| A closing summary: "And that's it! You've now learned how to…" | *(end on the last step or the last fix)* |
| Forced triads: "fast, easy and secure" | the one quality that is true and matters |
| Bold on every label of a list | bold only on screen labels and the one thing that must not be missed |
| Stacked hedges: "this may potentially take some time" | the real number, or `[to confirm]` |
| "allows you to", "enables you to" | "you can" |
| An objection nobody raised: "You might think this is complicated, but…" | *(cut it)* |

## Language

Write in the language of the product's users and stay in it. Product names and screen labels stay exactly as they appear on screen, even if they're in English.

Spanish shows the failure most often. These pairs stay in Spanish because the failure is Spanish:

| ✗ | ✓ |
|---|---|
| clickea en Guardar | haz clic en **Guardar** |
| loguéate con tu usuario | inicia sesión con tu usuario |
| setea la opción | configura la opción |
| el sistema se encarga de sincronizar | los cambios llegan a la tienda en 15 minutos |
| uploadea la imagen | sube la imagen |
| el dashboard *(cuando la pantalla dice "Panel")* | el **Panel** |
| hacé clic / abrí / podés *(voseo)* | haz clic / abre / puedes |

**One language per sentence.** A screen label in English inside a Spanish sentence is fine, because it's what the reader sees. A hybrid verb is not.

## Before sending

1. Every label in bold matches the screen exactly.
2. Every number was read, or is marked `[to confirm]`.
3. No internal names: no classes, endpoints, jobs, tables or flags.
4. Each step has one action, and the place comes before the action.
5. The title says the task the way the user would say it.
6. No "just", "simply", "easily", and no brochure words.
7. Neutral language: in Spanish, "tú" and no voseo; in English, no idioms and one spelling.
8. Run the cold review. See "Last step: a cold review".

## Last step: a cold review

You can't read your own text cold, because you already know how the feature works. So when the draft is done, and before you show it or hand it over, **delegate it to the `humanize:voice-reviewer` agent**. Pass the full draft, and say that the reader is an end user who never sees the code. Then apply its marks, except any that would change a fact, and deliver the corrected text.

Skip it only for a text of three lines or fewer, or when the agent isn't available. Then the "Before sending" list is the review.

## More shots

`references/examples.md` holds five full before/after rewrites. Example 1 (the developer's manual) is the failure this skill exists for.

Each correction from the reader becomes a new pair in that file, written in English, which is how this skill improves. When a correction changes a rule, update the rule here and fix every existing example and ✓ that now breaks it, since the model copies the examples over the rules.
