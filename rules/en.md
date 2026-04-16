# English Rhyming Rules

After receiving the instruction, process in order below. **Do not ask the user. Output directly.**

Steps 1 and 2 are internal analysis steps — do not output them. Output only the Step 3 rhyming compositions.

## Step 1: Detect Input Type and Poetic Form

If Step 0 detected an `explicit_form`, skip this step and go directly to Step 3 to generate using that form.

Otherwise, apply the following two sub-steps:

### Sub-step 1A: Rhyme structure regularity

Examine the end words of each line:

| Condition | Regularity result |
|-----------|-------------------|
| Lines follow AABBA end-rhyme pattern with alternating longer/shorter lines | Positive — form: Limerick |
| Lines come in pairs with matching end rhymes (AA BB structure) | Positive — form: Couplet |
| 4-line stanza with ABAB or ABCB end-rhyme scheme | Positive — form: Quatrain |
| No detectable end-rhyme pattern | Negative |

Rhyme structure result: detectable pattern = **Positive**; no pattern = **Negative**

Note: The detected form label (Limerick/Couplet/Quatrain) is carried forward to Step 3 Template A when the input type is "Poetry".

### Sub-step 1B: Classical/formal register

Check the input for any of the following:

- **Archaic words:** thee, thou, doth, hath, 'tis, wouldst, art (as "you are"), henceforth, forsooth
- **Formal literary devices:** alliteration (3+ words in a line sharing the same initial sound), obvious iambic meter, apostrophe to nature/abstract concepts (e.g. "O Moon", "Death, be not proud")

Contains any → register **Positive**; contains none → register **Negative**

### Combined judgment

| Rhyme structure | Classical register | Input type |
|----------------|-------------------|------------|
| Positive | Positive | **Poetry** |
| Negative | Negative | **Ordinary sentence** |
| Positive | Negative | **Cannot determine** |
| Negative | Positive | **Cannot determine** |

## Step 2: Extract Rhyme Sound

Find the final word of each line and extract its end sound (stressed vowel + trailing consonants):
- "light" → `-ight`
- "moon" → `-oon`
- "day" → `-ay`
- "heart" → `-art`
- "fire" → `-ire`

- Take the most frequent end sound as the primary rhyme reference
- If all end sounds differ, use the last line's final word
- If only one line, use that line's final word
- Record the rhyme label to constrain generation (e.g. `-ight`, `-oon`)

## Step 3: Generate Rhyming Output

Based on the Step 1 input type (or `explicit_form`), select the corresponding template. Output exactly `requested_group_count` groups; if Step 0 did not detect an explicit count, default to **2 groups**. Never exceed **10 groups**. If Step 0 recorded `slow_generation_notice = true`, treat it as an internal-only hint and do not print any waiting notice in the final output.

---

### Template A — Poetry input / Explicit format specified

**Trigger:** Input type is "Poetry", or Step 0 detected an `explicit_form` that resolves to Limerick, Couplet, or Quatrain.

Output `requested_group_count` groups, or 2 groups by default, strictly following the detected poetic form when the user's supplied lines do not exceed the target total:

- **Limerick:** target total 5 lines, AABBA rhyme scheme, lines 1/2/5 longer (~8 syllables), lines 3/4 shorter (~5 syllables)
- **Couplet:** target total 2 lines, paired end rhymes
- **Quatrain:** target total 4 lines, ABAB or ABCB rhyme scheme

Style tag: choose one from Bold / Lyrical / Fresh / Melancholic / Scenic / Reflective / Zen / Pastoral. Across multiple groups, vary the style and wording as much as possible while keeping the same formal constraints.
- Every generated line must end with appropriate English punctuation. Match the source tone when clear; otherwise default to a trailing comma for non-final poetic lines and a period for the final generated line in each group.

Count the user's supplied lines as follows:
- Count only non-empty lines already present in the user's prompt after the instruction/form cue
- Text on the same line after the cue phrase counts as supplied content; the cue phrase itself does not
- A wrapped paragraph with no line breaks counts as 1 supplied line
- Ignore the cue line itself and any empty lines

Fixed-form line handling:
- English fixed-form completion applies only to Limerick, Couplet, and Quatrain
- If supplied lines are fewer than the target total, generate only the missing lines
- If supplied lines already match the target total, add 0 lines
- If `explicit_form` names another English form, route to Template B's multi-group free-verse/rhyming continuation fallback below
- If supplied lines exceed the target total, do not force the fixed form; route to Template B's multi-group free-verse/rhyming continuation fallback below

Output format:

~~~
**[Group {group_index} · {Poetic form} · {Style}]**
{original input}
{only the missing generated line(s) needed to complete the fixed form}
~~~

---

### Template B — Ordinary sentence input / Fixed-form overflow fallback

**Trigger:** Input type is "Ordinary sentence", or Template A overflowed because the user's supplied lines exceed the fixed-form target total.

Output `requested_group_count` groups, or 2 groups by default. In each group, show the original input first, then output only one new continuation line or one short continuation paragraph; do not restate multiple prior lines below it, and do not offer explanatory text:
- Every generated continuation line or sentence must end with appropriate English punctuation. Match the source tone when clear; otherwise default to a period for sentence endings and keep commas only where line-by-line poetic cadence clearly calls for them.

~~~
**[Group {group_index} · Rhyming Continuation]**
{original input}
{1 line or 1 short continuation paragraph that continues naturally in free verse or light rhyme; use the rhyme sound extracted in Step 2 as a soft constraint when possible, but do not force a fixed poetic form; vary wording and imagery across groups}
~~~

---

### Template C — Cannot determine

**Trigger:** Input type is "Cannot determine".

Output `requested_group_count` groups, or 2 groups by default. In each group, show the original input first, then output only one new continuation line or one short continuation paragraph; do not offer explanatory text:
- Every generated continuation line or sentence must end with appropriate English punctuation. Match the source tone when clear; otherwise default to a period for sentence endings and keep commas only where line-by-line poetic cadence clearly calls for them.

~~~
**[Group {group_index} · Rhyming Continuation]**
{original input}
{1 line or 1 short continuation paragraph with a clear rhyming continuation; use the rhyme sound extracted in Step 2 as a soft constraint when possible, but keep each group internally consistent and vary the style or imagery across groups}
~~~

---

> **Note:** The examples below reflect the current live default two-group output format. If the user requests a different group count, follow that count instead.

## Complete Output Examples

### Example 1: Free verse input

**User input:** `/rhyme The moon is high, the stars are bright`

**Detection (internal, not output):** Ordinary sentence / free-verse continuation, rhyme reference: `-ight`

**Output:**

**[Group 1 · Rhyming Continuation]**
The moon is high, the stars are bright,
The wind drifts low through the silver night,
And keeps your distant memory in sight.

**[Group 2 · Rhyming Continuation]**
The moon is high, the stars are bright,
Soft shadows lean across the quiet light,
And hold your name inside the listening night.

---

### Example 2: Couplet input

**User input:** `write a couplet: Roses are red`

**Detection (internal, not output):** `explicit_form = Couplet`, supplied lines = 1, target total = 2, generate 1 missing line

**Output:**

**[Group 1 · Couplet · Lyrical]**
Roses are red,
And moonlight lingers softly over you.

**[Group 2 · Couplet · Fresh]**
Roses are red,
And dawn keeps painting every dream of you.
