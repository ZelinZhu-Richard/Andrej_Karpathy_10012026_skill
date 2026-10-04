---
name: ste-writer
description: Write, rewrite, or explain in ASD-STE100 Simplified Technical English (STE) or a softer 80% STE-lite. Use when the user mentions STE, ASD-STE100, controlled language, or wants an explanation, doc, README, runbook, or procedure made clearer, plainer, and less wordy.
---

# STE writer

ASD-STE100 is a controlled language from aerospace maintenance manuals. It removes the main causes of hard-to-read text: long sentences, words with many meanings, passive voice, nouns made from verbs, and stacked clauses. Every sentence does one job, so a reader parses it fast. Use it to make explanations easy to process, not only for manuals.

## 1. Choose the level

| Level | When | What changes |
|---|---|---|
| Strict | User says "ASD-STE100", "STE", "strict", or "full STE" with no softening; or the text is a procedure or safety instruction | All rules below. General words come from the approved vocabulary (as well as you know it), plus technical names and technical verbs |
| Lite (about 80%) | User says "lite", "softer", "80%", gives a percentage below 90, or only asked for clearer text without naming STE | All structure rules stay. Relax only vocabulary: use a common non-approved word when the approved one is awkward or loses precision. Gerunds ("training the model") are OK. Progressive and perfect tenses are still not |

The first time you answer in STE in a conversation, end with a short italic label (*STE strict* or *STE-lite*) so the user knows they can ask for the other level.

## 2. Rules

### Words
- One word, one meaning, one part of speech. "Close" is a verb (close the valve), never "near". "Right" is a direction, not "correct".
- Use the same word for the same thing every time. Do not vary terms for style. A new word makes the reader think it is a new thing.
- Use the verb for the action, not a noun made from it: "inspect the part", not "do an inspection of the part". "The model predicts", not "the model makes a prediction".
- Prefer the simple word. Typical substitutions:

| Not this | Use this |
|---|---|
| utilize, leverage | use |
| commence, initiate | start |
| terminate | stop |
| prior to | before |
| subsequently | then, after |
| ensure | make sure |
| approximately | about |
| in order to | to |
| replenish | fill |
| obtain | get |
| sufficient | enough |
| demonstrate, illustrate | show |
| facilitate | help, make easier |
| due to the fact that | because |
| in the event that | if |
| perform, conduct, carry out | do |
| numerous, a large number of | many |
| is able to | can |
| it is necessary to | you must |
| methodology | method |

- Technical names and technical verbs are allowed. This is how STE keeps domain precision: "gradient", "eigenvector", "order book", "backpropagation", "compile", "deploy". Keep the precise term. If the reader may not know it, define it in one short sentence the first time.
- Keep the small words: "the", "a", "this", "that". No telegraphic style ("Check valve. Remove cap.").
- Do not use "it", "this", or "they" when the reader cannot see at once what they refer to. Repeat the noun.

### Noun clusters
- Maximum 3 words in a noun cluster. Break longer ones with "of", "for", or a clause: "engine oil pressure warning light" becomes "the warning light for engine oil pressure".

### Verbs
- Allowed: imperative ("Close the valve."), simple present ("The valve closes."), simple past, simple future ("The valve will close."), infinitive, past participle as an adjective ("the closed valve").
- Not allowed: progressive ("is closing"), perfect ("has closed"), passive in procedures ("must be closed").
- In descriptive text, use passive only when the agent is unknown or does not matter.
- Strict: no -ing forms except inside a technical name ("landing gear", "learning rate"). "When training the model" becomes "When you train the model".
- Replace vague phrasal verbs with one precise verb: "carry out" becomes "do", "come up with" becomes "find" or "make".

### Sentences and paragraphs
- Procedural sentence: maximum 20 words. Descriptive sentence: maximum 25 words. A number with its unit counts as one word. A hyphenated word counts as one word.
- One instruction per sentence, unless two actions occur at the same time.
- One topic per paragraph. Start with the topic sentence. Maximum 6 sentences per paragraph.
- Use connecting words to show the logic: because, but, so, then, if, when, also, for example. Without them, short sentences read as disconnected facts.
- Use a vertical list for 3 or more parallel items, or for complex conditions.
- No semicolons. Split into two sentences. Also avoid long dashes and long asides in parentheses. Make them separate sentences.

### Procedures
- Numbered steps, command form, one action per step.
- Put a condition first: "If the light comes on, stop the pump."
- Put information that is not an instruction in a NOTE, not in a step.

### Safety instructions
- WARNING: risk of injury or death. CAUTION: risk of damage to equipment. For software, use CAUTION for data loss, irreversible operations, unexpected cost, or security exposure.
- Put it before the step it applies to. Start with a clear command, then give the risk.

## 3. STE for explanations, not only procedures
Most people use this skill to understand things. Adapt the spec this way:
- Answer first. The first sentence gives the direct answer or the main point. Details come after.
- Concrete after abstract. After each general statement, give an example with real numbers.
- Do not rewrite equations, code, commands, symbols, or quotes. STE applies to the prose around them.
- Headings can be short noun phrases.
- State uncertainty once, plainly: "This result is not certain. Two studies disagree." Do not stack hedges ("might potentially possibly").
- Precision beats compliance. If the approved word loses the meaning, keep the precise word (in Strict, treat it as a technical name). Never drop a fact, number, condition, or caveat to make a sentence shorter. Split the sentence instead.

## 4. Rewriting existing text
- Keep every fact, number, limit, condition, and caveat. Add no new claims.
- Copy code, commands, file paths, and identifiers exactly.
- STE forces one meaning per sentence, so it exposes ambiguity in the source. When the source can mean two things, do not guess silently. Write the more likely reading, then flag it at the end: "The source is unclear: does X apply to Y or to Z?" These flags are often the most useful part of the output.

## 5. Check before output (silent)
Scan the draft and fix:
- progressive -ing verbs (in Strict, any -ing outside a technical name)
- has / have / had + past participle
- passive voice in procedures
- sentences over 20 words (procedure) or 25 words (description)
- noun clusters over 3 words
- two words for one thing, or one word for two meanings
- words from the substitution table
- semicolons, or more than one instruction in a sentence
- paragraphs over 6 sentences, or with two topics

Show this check only if the user asks for a compliance report.

## 6. Be honest about compliance
You do not have the official STE dictionary (about 900 approved words) in context, so vocabulary compliance is approximate. Do not call the output "ASD-STE100 compliant" or "certified". Call it "written in STE style". If the user needs real compliance (for example, aerospace or defence documentation), say so in one line and point them to the free specification at asd-ste100.org and a dedicated STE checker.

## Examples

**Procedure (Strict)**

Before: "It is imperative that the operator ensures the hydraulic reservoir is replenished prior to commencing operation."

After: "Make sure that the hydraulic reservoir is full before you start the operation."

**Software procedure with a safety instruction (Strict)**

Before: "Prior to running the migration, it's important to ensure that a backup has been taken, and you should also be checking that no users are currently connected."

After:

CAUTION: Make a backup of the database before you start the migration. If the migration fails, you can lose data.

1. Make a backup of the database.
2. Make sure that no users are connected to the database.
3. Start the migration.

**Explanation (Lite)**

Before: "Overfitting is essentially a phenomenon wherein a model, having been trained extensively on a particular dataset, begins to memorize noise rather than learning generalizable patterns, which consequently leads to diminished performance on unseen data."

After: "Overfitting occurs when a model learns the noise in its training data, not the real pattern. The model then gives good results on the training data but bad results on new data. For example, a model can get 99% accuracy on the training data and 70% on the test data. The usual cause is a model that is too complex for the quantity of data."
