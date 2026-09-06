# Prompt: write a Post-It question pack

Paste everything below the line into Claude. Replace `N` with the number of rounds you want (1 to 10) and, if you like, name a theme. Save the reply as a `.json` file and import it from the host's setup screen, or drop it in `packs/` (see the README).

---

Write a trivia question pack for a living-room game called Post-It. Output **only** the JSON below, no prose before or after it, no code fences.

Format:

```json
{
  "name": "Pack name",
  "rounds": [
    {
      "title": "Round title",
      "questions": [
        { "q": "Question text?", "a": "Answer (common alternate phrasing)", "d": 1 }
      ]
    }
  ]
}
```

Requirements:

- Exactly **N rounds**. Every round has **exactly 5 questions**, no more, no fewer. Each round has a short title naming its theme.
- **Real, verifiable facts only.** Nothing that changes over time: no "current", "latest", "most recent", "newest", no record holders, no populations, no prices, no rankings, no living people's ages or job titles. Prefer facts that were settled decades ago and will still be true in twenty years.
- **One clearly stated correct answer per question.** Put common alternate phrasings or the accepted short form in parentheses, e.g. `"Marie Curie (Physics, 1903)"`, `"The piano"`, `"W, from its German name wolfram"`. Do not write questions that have several defensible answers.
- **Difficulty `d` is 1, 2 or 3**, shown only to the host. Spread within each round of roughly two 1s, two 2s and one 3. A 1 is something most adults know, a 2 needs some general knowledge, a 3 is a real test for a mixed room.
- **No trick questions.** No riddles, no wordplay, no "gotcha" phrasing, no double negatives.
- **The answer must not appear in the question**, including in a different form (do not ask "What is the capital of France, home of the Parisians?").
- **Mixed-crowd tone.** Assume a room of friends or family of different ages and countries. Avoid questions that only make sense in one country or to one generation, and avoid anything cruel or crude. Keep question text under about 160 characters.
- Vary the subjects across rounds (history, science, geography, arts, food, sport, language, everyday life) unless a theme is requested.

Worked example of one round in the required style:

```json
{
  "title": "History",
  "questions": [
    { "q": "In what year did the Berlin Wall fall?", "a": "1989", "d": 1 },
    { "q": "Who was the first woman to win a Nobel Prize?", "a": "Marie Curie (Physics, 1903)", "d": 1 },
    { "q": "The Rosetta Stone carries the same decree in three scripts. Name any two.", "a": "Hieroglyphic, Demotic, Ancient Greek", "d": 2 },
    { "q": "The 1494 Treaty of Tordesillas divided newly explored lands between which two countries?", "a": "Spain and Portugal", "d": 3 },
    { "q": "Mansa Musa, famous for his staggering wealth, ruled which West African empire?", "a": "The Mali Empire", "d": 3 }
  ]
}
```

Now write the pack: N rounds, 5 questions each, output the JSON only.
