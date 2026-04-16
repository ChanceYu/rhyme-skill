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

Based on the Step 1 input type (or `explicit_form`), select the corresponding template:

---

### Template A — Poetry input / Explicit format specified

**Trigger:** Input type is "Poetry", or Step 0 detected an `explicit_form`.

Output **1 group**, strictly following the detected poetic form:

- **Limerick:** 5 lines, AABBA rhyme scheme, lines 1/2/5 longer (~8 syllables), lines 3/4 shorter (~5 syllables)
- **Couplet:** 2–4 lines with paired end rhymes, last line must rhyme with at least one preceding line
- **Quatrain:** 4 lines, ABAB or ABCB rhyme scheme

Style tag: choose one from Bold / Lyrical / Fresh / Melancholic / Scenic / Reflective / Zen / Pastoral

Output format:

~~~
**[{Poetic form} · {Style}]**
> {original input}
{line 1}
{line 2}
{line 3}
{line 4} (if applicable — Limerick has 5 lines; Couplet may have 2)
~~~

---

### Template B — Ordinary sentence input

**Trigger:** Input type is "Ordinary sentence".

Output **2 groups** in order:

~~~
**[Colloquial Rhyme]**
> {original input}
{1–2 lines matching the casual register and vocabulary of the input; end word must rhyme with the input's last word or a prominent end word}

**[Poetic Version]**
> {original input}
{2–4 lines elevated to literary register; end rhyme required; richer imagery than the colloquial version}
~~~

---

### Template C — Cannot determine

**Trigger:** Input type is "Cannot determine".

Output **exactly 3 groups** in order:

~~~
**[Colloquial Rhyme]**
> {original input}
{1–2 casual rhyming lines; end word must rhyme with the input's last word or a prominent end word}

**[{Poetry style 1}]**
> {original input}
{2–4 poetic lines; style tag from: Bold / Lyrical / Fresh / Melancholic / Scenic / Reflective / Zen / Pastoral}

**[{Poetry style 2}]**
> {original input}
{2–4 poetic lines; different style tag from group 1; different imagery and vocabulary}
~~~

---

> **Note:** The examples below use the old 3-group format. Follow Templates A/B/C above for the correct output format. Examples are provided for tonal reference only.

## Complete Output Examples

### Example 1: Free verse input

**User input:** `/yayun The moon is high, the stars are bright`

**Detection (internal, not output):** Free verse, rhyme reference: `-ight` (bright)

**Output:**

**[Style 1: Lyrical]**
> The moon is high, the stars are bright
The wind sings soft through the quiet night,
And carries your name like a fading light,
I reach for you, but you're gone from sight,
Only shadows remain of what felt right.

**[Style 2: Bold]**
> The moon is high, the stars are bright
We rise before the dawn takes flight,
And forge our path with all our might,
No storm can dim this burning light,
We'll face the world and win the fight.

**[Style 3: Pastoral]**
> The moon is high, the stars are bright
The fireflies glow in the meadow at night,
The river runs soft in the silver moonlight,
A heron stands still in the last fading light,
And all of the world feels quiet and right.

---

### Example 2: Couplet input

**User input:** `/yayun Roses are red, violets are blue`

**Detection (internal, not output):** Couplet, rhyme reference: `-oo` (blue)

**Output:**

**[Style 1: Fresh]**
The morning dew clings to the grass so new,
And songbirds wake beneath the skies of blue.

**[Style 2: Melancholic]**
The letters you left are just fragments I knew,
I read them again in the cold morning dew.

**[Style 3: Reflective]**
Each path that I have walked brings me back to the view,
Where the river runs clear and the sky opens true.
