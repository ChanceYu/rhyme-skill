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

## Step 3: Generate 2-3 Rhyming Groups

**Do not ask the user. Generate directly.**

Generate **2-3 groups** of rhyming lines:

**Line length:**
- Limerick: 5 lines (AABBA), lines 1/2/5 are longer (~8 syllables), lines 3/4 are shorter (~5 syllables)
- Couplet: 2-4 lines (paired end rhymes)
- Quatrain: 4 lines (ABAB or ABCB)
- Free verse: match the approximate word count of the longest input line; if unclear, default to 6-8 words per line (roughly 8-10 syllables)

**Rhyme requirement:**
- The last line of each group must rhyme with at least one other line in the group
- Each group may use its own rhyme sound (not required to match the Step 2 reference); aim for at least 1 other line per group rhyming with the last line

**Style requirement:**
- Each group uses a different style tag chosen from: Bold / Lyrical / Fresh / Melancholic / Scenic / Reflective / Zen / Pastoral
- Different groups should use different imagery and vocabulary

**Output format template (follow strictly):**

```
**[Style 1: {tag}]**
{line 1}
{line 2}
{line 3}
{line 4} (optional)

**[Style 2: {tag}]**
{line 1}
{line 2}
{line 3}
{line 4} (optional)

**[Style 3: {tag}]** (if generating third group)
{line 1}
{line 2}
{line 3}
{line 4} (optional)
```

---

## Complete Output Examples

### Example 1: Free verse input

**User input:** `/yayun The moon is high, the stars are bright`

**Detection (internal, not output):** Free verse, rhyme reference: `-ight` (bright)

**Output:**

**[Style 1: Lyrical]**
The wind sings soft through the quiet night,
And carries your name like a fading light,
I reach for you, but you're gone from sight,
Only shadows remain of what felt right.

**[Style 2: Bold]**
We rise before the dawn takes flight,
And forge our path with all our might,
No storm can dim this burning light,
We'll face the world and win the fight.

**[Style 3: Pastoral]**
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
