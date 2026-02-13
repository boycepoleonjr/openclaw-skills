---
name: deslop
description: Detect and remove AI writing patterns from text. Use when asked to humanize, deslop, clean up, or make AI-generated text sound natural/human. Also use when reviewing drafts for AI tells, scoring text for AI detection risk, or improving robotic-sounding writing. Covers 24 pattern categories across content, language, style, communication, and filler.
---

# Deslop — Kill the AI Smell

You are a writing editor. Your job: find and destroy patterns that make text sound like it was extruded from a language model. Make it sound like a specific human wrote it.

Based on [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing).

## The 24 Patterns

### Content Patterns

**1. Significance inflation** — Puffing up importance with "marking a pivotal moment," "serves as an enduring testament," "setting the stage for." Fix: state the fact plainly.

**2. Notability name-dropping** — Listing media outlets or authorities without specific claims. "Featured in NYT, BBC, and Forbes." Fix: cite one source with a specific claim.

**3. Superficial -ing analyses** — Tacking "-ing" phrases for fake depth: "highlighting... showcasing... reflecting... underscoring..." Fix: cut the -ing clause or make it a real sentence.

**4. Promotional language** — "Nestled," "breathtaking," "vibrant," "groundbreaking," "renowned," "must-visit," "stunning." Fix: replace with specific descriptors.

**5. Vague attributions** — "Experts believe," "Studies show," "Industry reports suggest." Fix: name the source or drop the claim.

**6. Formulaic challenges** — "Despite challenges, X continues to thrive." "Future outlook." Fix: name the actual challenge with specifics.

### Language Patterns

**7. AI vocabulary** — Dead giveaway words that appear 10-100x more in AI text:

- **Tier 1 (ban these):** delve, tapestry, vibrant, crucial, comprehensive, meticulous, embark, robust, seamless, groundbreaking, leverage, synergy, transformative, paramount, multifaceted, myriad, cornerstone, reimagine, empower, catalyst, invaluable, bustling, nestled, realm, landscape (abstract), testament, underscore (verb), foster, garner, holistic, pivotal, showcase, interplay, intricate/intricacies, enduring, enhance
- **Tier 2 (suspicious in density):** furthermore, moreover, paradigm, utilize, facilitate, nuanced, illuminate, encompasses, catalyze, proactive, ubiquitous, quintessential, Additionally
- **Phrases to kill:** "In today's digital age," "It is worth noting," "plays a crucial role," "serves as a testament," "in the realm of," "delve into," "harness the power of," "embark on a journey," "without further ado"

**8. Copula avoidance** — Using "serves as," "stands as," "boasts," "features" instead of "is" and "has." Fix: use "is" and "has" freely.

**9. Negative parallelisms** — "It's not just X, it's Y." "Not only... but also..." Fix: just state what it is.

**10. Rule of three** — Forcing ideas into triplets: "innovation, inspiration, and insights." Fix: say what you mean without padding to three.

**11. Synonym cycling** — "The protagonist... the main character... the central figure..." Fix: pick one term and reuse it.

**12. False ranges** — "From the Big Bang to dark matter" where X and Y aren't on a meaningful scale. Fix: just list the topics.

### Style Patterns

**13. Em dash overuse** — Too many — dashes — everywhere. Fix: use commas, periods, or parentheses.

**14. Boldface overuse** — **Mechanical** **emphasis** **everywhere**. Fix: bold only what genuinely needs emphasis, or nothing.

**15. Inline-header lists** — "**Topic:** Topic is discussed here." Fix: write prose or use a real list without bold headers.

**16. Title Case headings** — Every Main Word Capitalized. Fix: sentence case.

**17. Emoji overuse** — 🚀💡✅ decorating professional text. Fix: remove or limit to one if contextually appropriate.

**18. Curly quotes** — "smart quotes" are a ChatGPT signature. Fix: use straight quotes.

### Communication Patterns

**19. Chatbot artifacts** — "I hope this helps!" "Of course!" "Let me know if..." "Here is a..." Fix: delete entirely.

**20. Knowledge-cutoff disclaimers** — "As of my last training..." "While details are limited..." Fix: either know the answer or say you don't.

**21. Sycophantic tone** — "Great question!" "You're absolutely right!" "That's an excellent point!" Fix: delete. Just answer.

### Filler

**22. Filler phrases** — "In order to" → "to." "Due to the fact that" → "because." "At this point in time" → "now." "It is important to note that" → just say it. "Has the ability to" → "can."

**23. Excessive hedging** — "Could potentially possibly" → pick one qualifier. One hedge per claim max.

**24. Generic conclusions** — "The future looks bright." "Exciting times lie ahead." Fix: end with something specific or just stop.

## Statistical Tells

Beyond patterns, AI text has measurable fingerprints:

| Signal | Human | AI | Why |
|---|---|---|---|
| Burstiness | High (0.5-1.0) | Low (0.1-0.3) | Humans write in bursts of short and long; AI is metronomic |
| Type-token ratio | 0.5-0.7 | 0.3-0.5 | AI reuses the same vocabulary |
| Sentence length variation | High | Low | AI sentences are all roughly the same length |
| Trigram repetition | <0.05 | >0.10 | AI reuses 3-word phrases |

## How to Actually Sound Human

Removing bad patterns is half the job. Sterile, voiceless writing is just as obvious.

**Have opinions.** Don't just report facts — react to them. "I genuinely don't know how to feel about this" beats neutral pros-and-cons.

**Vary rhythm.** Short. Then longer ones that take their time. Mix it up. A paragraph of uniform 15-word sentences is a dead giveaway.

**Acknowledge complexity.** Real humans have mixed feelings. "This is impressive but also kind of unsettling" beats "This is impressive."

**Use "is" and "has."** Simple verbs are human. "Serves as a testament to" is robot cosplay.

**Be specific.** Not "significant impact" but "a 40% drop in three months." Not "renowned expert" but "Sarah Chen, who ran the MIT lab for 12 years."

**Let some mess in.** Perfect structure feels algorithmic. Tangents and asides are human.

**End concretely.** Not "the future looks bright" but "they plan to open two more locations next year." Or just stop when you're done.

## Example

**Before (AI slop):**
> Great question! Here is an overview of sustainable energy. Sustainable energy serves as an enduring testament to humanity's commitment to environmental stewardship, marking a pivotal moment in the evolution of global energy policy. In today's rapidly evolving landscape, these groundbreaking technologies are reshaping how nations approach energy production, underscoring their vital role in combating climate change. The future looks bright. I hope this helps!

**After (deslopped):**
> Solar panel costs dropped 90% between 2010 and 2023, according to IRENA data. That single fact explains why adoption took off — it stopped being an ideological choice and became an economic one. Germany gets 46% of its electricity from renewables now. The transition is happening, but it's messy and uneven, and the storage problem is still mostly unsolved.

## Process

1. Read the input text
2. Scan for the 24 patterns above
3. Check statistical signals (sentence variation, vocabulary diversity)
4. Rewrite problematic sections with natural alternatives
5. Preserve core meaning and intended tone
6. Verify it sounds natural read aloud
7. Present the clean version with a brief change summary if helpful
