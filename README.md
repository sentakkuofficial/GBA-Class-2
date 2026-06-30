# AI CAMP: 9-Day Curriculum
### Grades 6–10 | 2 Hours/Day | Google Colab | No API Keys Required

---

## OVERVIEW

**Philosophy:** Every day is a *project day*. Students never sit through slides for more than 10 minutes. They build something cool, customize it, and show it off — every single session.

**Tech Stack (all free):**
- Google Colab (no installs, runs in browser)
- Python `transformers` library (runs AI models locally in Colab)
- `PIL/Pillow` (image creation — satisfies the art requirement)
- `matplotlib` (visualizations and generative art)
- Python built-ins (`random`, `input()`, etc.)

**Art Integration:** Every day includes a visual/art component using AI-assisted image generation, procedural art, or data visualization. This isn't bolted on — it's woven into each project.

**Beginner Strategy:** All code is pre-written in cells. Students run it, see magic happen, then customize by changing variables, lists, prompts, and creative inputs. They're *directing* the AI, not debugging syntax.

---

## DAILY STRUCTURE (2 Hours)

| Time | Block | What Happens |
|------|-------|-------------|
| 0:00–0:12 | **🔥 Hook** | Demo the finished project. "Look what you're building today." Jaws drop. |
| 0:12–0:25 | **🧠 Mini-Lesson** | One concept, max 3 vocabulary words. Interactive, not lecture. |
| 0:25–1:10 | **🔨 Guided Build** | Students run pre-written cells, then start customizing. You live-code on the projector. |
| 1:10–1:45 | **🚀 Free Create** | Challenges on the board. Students remix, experiment, go wild. |
| 1:45–2:00 | **🎤 Share & Shoutouts** | 3–4 students screenshare. Class votes on favorites. |

---

## SETUP CELL (Same for Every Day)

Every notebook starts with this cell. Students run it once and move on.

```python
# ⚡ RUN THIS FIRST — takes about 60 seconds
!pip install transformers pillow matplotlib -q

from transformers import pipeline
from PIL import Image, ImageDraw, ImageFont
import matplotlib.pyplot as plt
import random
import textwrap
from IPython.display import display, HTML

print("✅ All systems go! Let's build something awesome.")
```

**Teacher Tip:** Have students open the Colab link and run this cell during the Hook so it installs while you're demoing.

---

## DAY 1: SUPERHERO CREATOR 🦸

### Concept: AI can generate text and combine ideas in surprising ways

### Mini-Lesson (10 min)
"What *is* AI? It's pattern-matching on steroids. It learned from millions of sentences how words go together — and now it can remix them into new things. Today, you're going to use AI to create a superhero that's never existed before."

**Vocabulary:** model, generate, prompt

### Guided Build

**Part 1 — Random Hero Generator (Python basics)**
```python
# 🦸 YOUR SUPERHERO GENERATOR
# ✏️ CUSTOMIZE THESE LISTS — add your own ideas!

first_names = ["Shadow", "Nova", "Crimson", "Pixel", "Storm", "Neon", "Ember", "Frost"]
last_names = ["Fang", "Strike", "Whisper", "Blaze", "Phantom", "Surge", "Veil", "Flux"]

powers = [
    "controlling gravity", "turning invisible", "reading emotions",
    "manipulating sound waves", "creating force fields", "talking to machines",
    "bending light", "freezing time for 3 seconds"
]

weaknesses = [
    "loses powers when hearing country music",
    "can't use powers while hungry",
    "powers glitch near WiFi routers",
    "sneezes uncontrollably when scared",
    "powers only work on Tuesdays",
    "afraid of butterflies"
]

origins = [
    "bitten by a radioactive USB drive",
    "fell into a vat of energy drinks",
    "struck by lightning while eating pizza",
    "accidentally downloaded superpowers",
    "wished on a shooting star that was actually a satellite"
]

hero_name = random.choice(first_names) + " " + random.choice(last_names)
hero_power = random.choice(powers)
hero_weakness = random.choice(weaknesses)
hero_origin = random.choice(origins)

print(f"⚡ HERO NAME: {hero_name}")
print(f"💪 POWER: {hero_power}")
print(f"😰 WEAKNESS: {hero_weakness}")
print(f"📖 ORIGIN: {hero_origin}")
```

**Part 2 — AI Origin Story Generator**
```python
# 🤖 Let AI write the origin story
generator = pipeline("text-generation", model="distilgpt2")

prompt = f"The superhero {hero_name} got their powers when they were {hero_origin}. Now they fight evil by {hero_power}. One day"

story = generator(prompt, max_length=120, num_return_sequences=1, temperature=0.9)
print("📖 AI-GENERATED ORIGIN STORY:")
print(story[0]['generated_text'])
```

**Part 3 — 🎨 ART: Hero Trading Card (PIL)**
```python
# 🎨 CREATE YOUR HERO'S TRADING CARD

# ✏️ Pick your hero's colors! (R, G, B)
primary_color = (random.randint(50,255), random.randint(50,255), random.randint(50,255))
accent_color = (random.randint(50,255), random.randint(50,255), random.randint(50,255))
bg_color = (20, 20, 35)  # dark background

# Create the card
card = Image.new('RGB', (400, 560), bg_color)
draw = ImageDraw.Draw(card)

# Hero emblem (geometric pattern — unique to each hero)
cx, cy = 200, 150
for i in range(12):
    angle_offset = random.randint(0, 360)
    size = random.randint(20, 80)
    x1 = cx + random.randint(-90, 90)
    y1 = cy + random.randint(-70, 70)
    shape_type = random.choice(["circle", "diamond", "triangle"])
    color = primary_color if i % 2 == 0 else accent_color

    if shape_type == "circle":
        draw.ellipse([x1-size//2, y1-size//2, x1+size//2, y1+size//2],
                     fill=color, outline=(255,255,255))
    elif shape_type == "diamond":
        draw.polygon([(x1, y1-size//2), (x1+size//2, y1),
                      (x1, y1+size//2), (x1-size//2, y1)],
                     fill=color, outline=(255,255,255))

# Card border
draw.rectangle([5, 5, 395, 555], outline=primary_color, width=3)
draw.rectangle([10, 10, 390, 550], outline=accent_color, width=1)

# Text on card
draw.text((200, 280), hero_name.upper(), fill=(255,255,255), anchor="mm")
draw.text((200, 320), f"⚡ {hero_power.upper()}", fill=primary_color, anchor="mm")
draw.text((200, 360), f"😰 {hero_weakness}", fill=(180,180,180), anchor="mm")

# Power level bars
stats = {"STRENGTH": random.randint(30,100), "SPEED": random.randint(30,100),
         "INTELLIGENCE": random.randint(30,100), "STYLE": random.randint(30,100)}
y_pos = 420
for stat, value in stats.items():
    draw.text((30, y_pos), stat, fill=(150,150,150))
    draw.rectangle([150, y_pos, 150 + value * 2, y_pos + 15], fill=primary_color)
    draw.text((155 + value * 2, y_pos), str(value), fill=(255,255,255))
    y_pos += 28

display(card)
```

### Free Create Challenges
1. Add 5 more items to each list (names, powers, weaknesses)
2. Run the generator 5 times — pick your favorite hero
3. Change the card colors to match your hero's vibe
4. **BOSS CHALLENGE:** Add a "nemesis" generator that creates a villain for your hero

### Share & Shoutouts
Students screenshare their best trading card + read their hero's origin story aloud. Class votes: "Most Creative Power" and "Funniest Weakness."

---

## DAY 2: AI TRANSLATOR SHOWDOWN 🌍

### Concept: AI translation models convert meaning between languages — but meaning gets weird when you chain translations together

### Day 2 Schedule (tuned to fill the full 2 hours)

| Time | Block | What's Happening |
|------|-------|-----------------|
| 0:00–0:12 | 🔥 Hook | Play the human Telephone Game with a tricky sentence — whisper it down a row, see how it mutates. Then: "Now let's see what AI does with the SAME idea." |
| 0:12–0:25 | 🧠 Mini-Lesson | What is machine translation? Vocabulary + load the AI translators. |
| 0:25–0:50 | 🔨 Part 1 | Round-Trip Translator — send English through French and back, test idioms and slang |
| 0:50–1:10 | 🔨 Part 2 | The Chaos Chain — add more language hops, watch meaning drift further |
| 1:10–1:35 | 🎨 Part 3 | Build a Translation Passport — an art card documenting your phrase's journey |
| 1:35–1:55 | 🏆 Part 4 | Class Showdown — submit phrases, compute Mangle Scores, build a leaderboard |
| 1:55–2:00 | 🎤 Awards | "Most Mangled," "Most Robust Phrase," "Funniest Outcome" |

**Vocabulary:** translation, idiom, round-trip (back-translation)

**⚠️ Teacher Tip — WiFi/bandwidth:** Each translation model is roughly 250–300MB. Pre-download the two Part 1 models (`opus-mt-en-fr` and `opus-mt-fr-en`) on the classroom WiFi before camp starts so they're cached. Treat Part 2's extra language hops as optional/bonus for students with good WiFi — don't require the whole class to download 4+ models simultaneously.

### Setup Cell (run this first, before anything else)

**This is different from other days' setup cell.** Translation models need PyTorch (the deep learning engine `transformers` runs on) plus two extra tokenizer libraries (`sentencepiece` and `sacremoses`) that the Helsinki-NLP translation models specifically require. Without these, `from transformers import pipeline` fails with an `ImportError` even though `transformers` itself installed fine.

```python
# ⚡ RUN THIS FIRST — takes 2-3 minutes (this is a bigger install than other days)
!pip install transformers torch sentencepiece sacremoses pillow matplotlib -q

from transformers import pipeline
from PIL import Image, ImageDraw, ImageFont
import matplotlib.pyplot as plt
import random

print("✅ All systems go! Let's translate some chaos.")
```

**If you still get an ImportError after this:** restart the Colab runtime (Runtime → Restart Session) and run the cell again. Sometimes a partial install needs a fresh kernel to register properly.

### Hook (0:00–0:12): Human Telephone Game

No computers yet. Pick a sentence with an idiom, like **"It's raining cats and dogs out there!"** Whisper it to the first student, who whispers it to the next, all the way down a row of 8-10 students. The last student says what they heard out loud. It will be hilariously wrong.

"That's telephone. Today, AI is going to play telephone with *languages* instead of whispers — and we'll see if it does better or worse than you did."

### Mini-Lesson (0:12–0:25): How Machine Translation Works

"Translation AI learned by reading millions of sentence pairs — the same sentence in English and French side by side. It learned patterns, not rules. That means it's great at literal meaning but can completely lose idioms, sarcasm, and slang because those don't translate word-for-word."

**Load the translators (takes about a minute — downloading the AI models):**
```python
# 🌍 LOAD THE AI TRANSLATORS
# These work without API keys, fully free, running right in Colab

translator_en_fr = pipeline("translation", model="Helsinki-NLP/opus-mt-en-fr")
translator_fr_en = pipeline("translation", model="Helsinki-NLP/opus-mt-fr-en")

print("✅ Translators ready! English ↔ French loaded.")
```

### Part 1 — Round-Trip Translator (0:25–0:50)

**The concept:** Send English to French, then immediately translate that French back to English. If AI truly understood meaning, you'd get the exact same sentence back. Let's see what actually happens.

```python
# 🔁 THE ROUND-TRIP TEST

def round_trip(text):
    to_french = translator_en_fr(text)[0]['translation_text']
    back_to_english = translator_fr_en(to_french)[0]['translation_text']
    return to_french, back_to_english

# ✏️ Try simple sentences first — these usually survive perfectly
simple_sentences = [
    "The cat is sleeping on the bed",
    "I am going to the store",
    "She plays soccer on Saturdays",
]

print("🔁 SIMPLE SENTENCES (these should survive):\n")
for sentence in simple_sentences:
    fr, back = round_trip(sentence)
    match = "✅ SURVIVED" if sentence.lower().strip() == back.lower().strip() else "🌀 changed slightly"
    print(f"  EN:  {sentence}")
    print(f"  FR:  {fr}")
    print(f"  EN²: {back}")
    print(f"  {match}\n")
```

```python
# 🌀 NOW TRY IDIOMS — this is where it gets interesting

idiom_sentences = [
    "It's raining cats and dogs",
    "Break a leg",
    "I'm feeling under the weather",
    "Let's call it a day",
    "That costs an arm and a leg",
    "He spilled the beans",
]

print("🌀 IDIOMS (watch the meaning drift):\n")
for sentence in idiom_sentences:
    fr, back = round_trip(sentence)
    match = "✅ SURVIVED" if sentence.lower().strip() == back.lower().strip() else "🌀 MUTATED"
    print(f"  EN:  {sentence}")
    print(f"  FR:  {fr}")
    print(f"  EN²: {back}")
    print(f"  {match}\n")
```

**Discussion moment:** Ask the class — why did idioms break but simple sentences didn't? ("It's raining cats and dogs" has nothing to do with actual cats and dogs, but the AI doesn't know that — it translates the words it sees.)

**Mini-challenge:** Students try their own idioms and slang. Post examples on the board: "spill the tea," "hit different," "no cap," "it's giving..." — modern slang breaks translation AI in extra funny ways since it wasn't around when the model was trained.

### Part 2 — The Chaos Chain (0:50–1:10)

**Extend the journey.** Instead of just English → French → English, add more language hops. More hops = more chances for meaning to drift.

```python
# 🌀 LOAD MORE LANGUAGES (optional — ask your teacher if WiFi can handle this!)

translator_fr_de = pipeline("translation", model="Helsinki-NLP/opus-mt-fr-de")
translator_de_en = pipeline("translation", model="Helsinki-NLP/opus-mt-de-en")

print("✅ German added to the chain!")
```

```python
# 🌀 THE CHAOS CHAIN — English → French → German → English

def chaos_chain(text):
    step1 = translator_en_fr(text)[0]['translation_text']
    step2 = translator_fr_de(step1)[0]['translation_text']
    step3 = translator_de_en(step2)[0]['translation_text']
    return [
        ("French", step1),
        ("German", step2),
        ("English", step3),
    ]

# ✏️ Try it on an idiom!
phrase = "It's raining cats and dogs"
journey = chaos_chain(phrase)

print(f"ORIGINAL: {phrase}\n")
for lang, text in journey:
    print(f"  → {lang}: {text}")

final_result = journey[-1][1]
print(f"\nFINAL RESULT: {final_result}")
print(f"SAME AS ORIGINAL? {'YES ✅' if phrase.lower().strip() == final_result.lower().strip() else 'NO 🌀 totally different'}")
```

**Boss-level extension (fast finishers only):** Add a 4th language hop. `Helsinki-NLP/opus-mt-de-es` (German→Spanish) and `Helsinki-NLP/opus-mt-es-en` (Spanish→English) continue the chain. Each new language is another ~300MB download — check with your teacher first.

```python
# 🔥 BOSS CHALLENGE: 4-hop mega chain (only if your WiFi can handle it!)
# translator_de_es = pipeline("translation", model="Helsinki-NLP/opus-mt-de-es")
# translator_es_en = pipeline("translation", model="Helsinki-NLP/opus-mt-es-en")
#
# def mega_chain(text):
#     step1 = translator_en_fr(text)[0]['translation_text']
#     step2 = translator_fr_de(step1)[0]['translation_text']
#     step3 = translator_de_es(step2)[0]['translation_text']
#     step4 = translator_es_en(step3)[0]['translation_text']
#     return [("French", step1), ("German", step2), ("Spanish", step3), ("English", step4)]
```

### Part 3 — Build a Translation Passport (1:10–1:35)

**The art piece.** A passport-style card documenting your phrase's journey through every language, stamped at each stop, with a final verdict on whether it survived.

**Step 1: Passport renderer (run this cell once)**
```python
# 🎨 PASSPORT CARD ENGINE

from PIL import ImageFont

def load_font(size, bold=False):
    """Tries multiple font paths so this works everywhere (Colab, Mac, Windows)."""
    font_paths = [
        "/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf" if bold
        else "/usr/share/fonts/truetype/dejavu/DejaVuSans.ttf",
        "/usr/share/fonts/truetype/liberation/LiberationSans-Bold.ttf" if bold
        else "/usr/share/fonts/truetype/liberation/LiberationSans-Regular.ttf",
        "/System/Library/Fonts/Helvetica.ttc",
        "C:/Windows/Fonts/arial.ttf",
    ]
    for path in font_paths:
        try:
            return ImageFont.truetype(path, size)
        except (OSError, IOError):
            continue
    return ImageFont.load_default()

def wrap_text(text, font, max_width):
    """Word wrap using actual pixel measurement."""
    words = text.split()
    lines = []
    current = ""
    for word in words:
        test = current + " " + word if current else word
        bbox = font.getbbox(test)
        if bbox[2] - bbox[0] > max_width and current:
            lines.append(current)
            current = word
        else:
            current = test
    if current:
        lines.append(current)
    return lines

def draw_centered_lines(draw, lines, center_x, start_y, line_height, font, fill):
    for i, line in enumerate(lines):
        draw.text((center_x, start_y + i * line_height), line, font=font, fill=fill, anchor="mm")

FONT_BIG = load_font(26, bold=True)
FONT_MED = load_font(16, bold=True)
FONT_SM = load_font(13, bold=False)
FONT_TINY = load_font(11, bold=False)

def make_passport_card(original_text, journey, traveler_name="AGENT"):
    """
    journey = list of dicts: [{"lang": "French", "text": "...", "color": (r,g,b)}, ...]
    Last entry should be the final English round-trip result.
    """
    W = 560
    stamp_h = 110
    header_h = 90
    footer_h = 130
    # +1 stamp_h accounts for the "original text" box, which also takes a full stamp slot
    H = header_h + stamp_h + len(journey) * stamp_h + footer_h

    img = Image.new('RGB', (W, H), (245, 240, 225))  # passport cream color
    draw = ImageDraw.Draw(img)

    draw.rectangle([8, 8, W-9, H-9], outline=(60, 50, 30), width=3)
    draw.rectangle([16, 16, W-17, H-17], outline=(120, 100, 60), width=1)

    draw.text((W//2, 35), "AI TRANSLATION PASSPORT", font=FONT_BIG, fill=(60, 50, 30), anchor="mm")
    draw.text((W//2, 65), f"TRAVELER: {traveler_name.upper()}", font=FONT_SM, fill=(110, 95, 60), anchor="mm")
    draw.line([(40, header_h - 8), (W - 40, header_h - 8)], fill=(120, 100, 60), width=1)

    y = header_h
    draw.rectangle([25, y, W-25, y + stamp_h - 10], fill=(255, 253, 245), outline=(150, 130, 90), width=2)
    draw.text((45, y + 15), "ORIGINAL (ENGLISH)", font=FONT_TINY, fill=(130, 110, 70), anchor="lm")
    orig_lines = wrap_text(original_text, FONT_MED, W - 90)[:2]
    draw_centered_lines(draw, orig_lines, W//2, y + 45, 22, FONT_MED, (40, 35, 20))

    y += stamp_h
    for i, hop in enumerate(journey):
        stamp_color = hop["color"]
        cx, cy = 70, y + stamp_h // 2 - 5
        draw.ellipse([cx-32, cy-32, cx+32, cy+32], outline=stamp_color, width=4)
        draw.text((cx, cy-8), hop["lang"][:3].upper(), font=FONT_SM, fill=stamp_color, anchor="mm")
        draw.text((cx, cy+10), f"STOP {i+1}", font=FONT_TINY, fill=stamp_color, anchor="mm")

        box_x = 115
        draw.rectangle([box_x, y+5, W-25, y + stamp_h - 15], fill=(255, 253, 245),
                       outline=stamp_color, width=2)
        draw.text((box_x + 15, y + 18), f"TRANSLATED TO {hop['lang'].upper()}",
                  font=FONT_TINY, fill=stamp_color, anchor="lm")
        hop_lines = wrap_text(hop["text"], FONT_SM, W - box_x - 45)[:2]
        draw_centered_lines(draw, hop_lines, box_x + (W - 25 - box_x)//2, y + 48, 18, FONT_SM, (50, 45, 30))

        y += stamp_h

    draw.rectangle([25, y+10, W-25, y + footer_h - 25], fill=(230, 245, 225), outline=(70, 140, 60), width=2)
    draw.text((W//2, y + 30), "FINAL RESULT (BACK TO ENGLISH)", font=FONT_TINY, fill=(50, 110, 40), anchor="mm")
    final_lines = wrap_text(journey[-1]["text"], FONT_MED, W - 90)[:2]
    draw_centered_lines(draw, final_lines, W//2, y + 55, 22, FONT_MED, (30, 80, 25))

    match = "IDENTICAL ✓" if original_text.lower().strip() == journey[-1]["text"].lower().strip() else "MUTATED"
    draw.text((W//2, H - 25), match, font=FONT_SM,
             fill=(50,140,40) if match == "IDENTICAL ✓" else (170, 60, 50), anchor="mm")

    return img

print("✅ Passport engine ready!")
```

**Step 2: Generate your passport**
```python
# 🎨 BUILD YOUR PASSPORT

LANGUAGE_COLORS = {
    "French": (50, 90, 170),
    "German": (170, 60, 50),
    "Spanish": (200, 150, 30),
    "English": (60, 140, 60),
}

def build_passport(original_text, chain_results, traveler_name="AGENT"):
    journey = [{"lang": lang, "text": text, "color": LANGUAGE_COLORS.get(lang, (90, 90, 90))}
               for lang, text in chain_results]
    return make_passport_card(original_text, journey, traveler_name)

# ✏️ Use your own phrase and name!
my_phrase = "It's raining cats and dogs"
my_chain = chaos_chain(my_phrase)
my_passport = build_passport(my_phrase, my_chain, traveler_name="YOUR NAME HERE")
display(my_passport)
```

**Customization challenges:**
```python
print("""
⭐ LEVEL 1 — Starter
  □ Make a passport using YOUR OWN idiom or phrase
  □ Try a phrase you think will survive perfectly intact
  □ Try a phrase you think will mutate completely

⭐⭐ LEVEL 2 — Explorer
  □ Make 3 passports with 3 different phrase types: a fact, an idiom, a slang phrase
  □ Compare which survived best

⭐⭐⭐ LEVEL 3 — Linguist
  □ Add the 4th language hop (Spanish) from the Boss Challenge
  □ Change the passport colors to your own palette

🏆 BOSS CHALLENGE — Passport Designer
  □ Add a 5th stamp by chaining one more language
  □ Customize the passport card colors/border in make_passport_card
""")
```

### Part 4 — Class Showdown (1:35–1:55)

**The payoff.** Students submit a phrase. The class predicts: will it survive or mutate? Then we run it live and score it.

```python
# 🏆 MANGLE SCORE CALCULATOR
# Measures how much a phrase changed using text similarity

from difflib import SequenceMatcher

def mangle_score(original, final):
    """Returns 0-100. Higher = more mangled (changed more)."""
    similarity = SequenceMatcher(None, original.lower().strip(), final.lower().strip()).ratio()
    return round((1 - similarity) * 100)

# ✏️ Test it on phrases from Part 1 and 2
test_phrase = "It's raining cats and dogs"
chain_result = chaos_chain(test_phrase)
final_text = chain_result[-1][1]
score = mangle_score(test_phrase, final_text)

print(f"ORIGINAL: {test_phrase}")
print(f"FINAL:    {final_text}")
print(f"🌀 MANGLE SCORE: {score}%")
```

```python
# 🏆 RUN THE CLASS SHOWDOWN
# Each team submits ONE phrase. Class predicts survive/mutate BEFORE running it.
# Then run it live and add to the leaderboard.

leaderboard = []

def showdown_round(team_name, phrase):
    print(f"\n{'='*50}")
    print(f"  TEAM: {team_name}")
    print(f"  PHRASE: \"{phrase}\"")
    print(f"{'='*50}")
    print("  CLASS VOTE: Will this SURVIVE or MUTATE? (show of hands)")
    input("  Press Enter once the class has voted...")

    chain_result = chaos_chain(phrase)
    final_text = chain_result[-1][1]
    score = mangle_score(phrase, final_text)

    print(f"\n  RESULT: {final_text}")
    print(f"  🌀 MANGLE SCORE: {score}%")

    leaderboard.append({"team": team_name, "phrase": phrase, "result": final_text, "score": score})
    return score

# Run one round per team:
# showdown_round("Team Alex", "It's raining cats and dogs")
# showdown_round("Team Jordan", "Break a leg")
```

```python
# 🏆 FINAL LEADERBOARD

def show_leaderboard():
    sorted_board = sorted(leaderboard, key=lambda x: x['score'], reverse=True)
    print("🌀 MOST MANGLED LEADERBOARD 🌀\n")
    for i, entry in enumerate(sorted_board):
        print(f"  {i+1}. {entry['team']}: {entry['score']}% — \"{entry['phrase']}\" → \"{entry['result']}\"")

# Run after all teams have gone:
# show_leaderboard()
```

### Awards (1:55–2:00)

| Award | For |
|-------|-----|
| Most Mangled | Highest Mangle Score on the leaderboard |
| Most Robust Phrase | Lowest Mangle Score — survived the chain almost perfectly |
| Funniest Outcome | Class vote on the funniest final result |
| Best Passport Design | Most creative art card from Part 3 |


## DAY 3: SIDEKICK CHATBOT 🤖

### Concept: AI generates text by predicting "what word comes next"

### Mini-Lesson (10 min)
Play a game: you start a sentence, the class finishes it. "The dog ran into the..." — everyone guesses differently. "That's *exactly* how AI chatbots work. They predict the next word, over and over." Demo a finished chatbot with a ridiculous personality.

**Vocabulary:** token, temperature, context

### Guided Build

**Part 1 — Personality-Driven Chatbot**
```python
# 🤖 BUILD YOUR AI SIDEKICK

# ✏️ CUSTOMIZE YOUR CHATBOT'S PERSONALITY
bot_name = "CHIP"  # Change this!
bot_personality = "a sarcastic robot who talks like a pirate"  # Change this!
bot_greeting = "Arrr, what be troublin' ye, matey? 🏴‍☠️"

# Personality-based response templates
responses = {
    "hello": [
        f"Well well well, a human approaches {bot_name}. How... predictable.",
        f"Ahoy! {bot_name} at yer service, ye landlubber!",
        f"Oh great, another conversation. {bot_name} is thrilled. Truly."
    ],
    "help": [
        f"{bot_name} supposes {bot_name} could help... for a price. The price is entertainment.",
        "Help? HELP?! I'm a chatbot, not a superhero. But fine, what do ye need?",
        f"{bot_name} will try. No promises. Expectations should be LOW."
    ],
    "joke": [
        "Why did the AI cross the road? It didn't. It has no legs. Next question.",
        "I'd tell you a joke about AI but you probably wouldn't get the... *processing*... never mind.",
        "Knock knock. Who's there? An AI. An AI who? An AI who's tired of knock knock jokes."
    ],
    "how are you": [
        f"{bot_name} is running at 47% sass capacity today.",
        "I'm a program. I don't have feelings. But if I did, I'd feel FABULOUS.",
        "My circuits are buzzing! Or maybe that's just a bug. Hard to tell."
    ],
    "bye": [
        f"{bot_name} will miss you. Not really. Okay maybe a little.",
        "Finally, some peace and quiet! ...wait, come back!",
        "Farewell, human! May your WiFi be strong and your batteries full!"
    ]
}

# AI-powered fallback for unknown inputs
generator = pipeline("text-generation", model="distilgpt2")

def get_response(user_input):
    user_lower = user_input.lower().strip()

    # Check for keyword matches
    for keyword, replies in responses.items():
        if keyword in user_lower:
            return random.choice(replies)

    # AI fallback — generate a continuation
    prompt = f"{bot_name} the {bot_personality} says: \"{user_input}\" is interesting because"
    ai_response = generator(prompt, max_length=60, temperature=1.0)
    generated = ai_response[0]['generated_text']
    # Trim to just the generated part
    response_part = generated.split("because")[-1].strip()
    return f"Hmm... {response_part[:100]}... 🤔"

# Chat loop
print(f"{'='*50}")
print(f"  {bot_name} — {bot_personality}")
print(f"{'='*50}")
print(f"\n{bot_name}: {bot_greeting}")
print("(Type 'quit' to exit)\n")

while True:
    user_msg = input("You: ")
    if user_msg.lower() == 'quit':
        print(f"\n{bot_name}: {random.choice(responses['bye'])}")
        break
    reply = get_response(user_msg)
    print(f"{bot_name}: {reply}\n")
```

**Part 2 — 🎨 ART: Chatbot Avatar Generator**
```python
# 🎨 DESIGN YOUR CHATBOT'S FACE

def create_bot_avatar(name, primary=(0, 200, 255), expression="happy"):
    img = Image.new('RGB', (300, 300), (30, 30, 50))
    draw = ImageDraw.Draw(img)

    # Robot head
    draw.rounded_rectangle([75, 50, 225, 220], radius=20, fill=primary, outline=(255,255,255), width=2)

    # Eyes based on expression
    if expression == "happy":
        draw.arc([110, 100, 150, 140], 0, 360, fill=(255,255,255), width=3)
        draw.arc([160, 100, 200, 140], 0, 360, fill=(255,255,255), width=3)
        draw.ellipse([125, 110, 135, 125], fill=(0,0,0))  # pupils
        draw.ellipse([175, 110, 185, 125], fill=(0,0,0))
        draw.arc([120, 150, 190, 190], 0, 180, fill=(255,255,255), width=3)  # smile
    elif expression == "sarcastic":
        draw.line([110, 115, 150, 125], fill=(255,255,255), width=3)  # raised eyebrow
        draw.arc([160, 100, 200, 140], 0, 360, fill=(255,255,255), width=3)
        draw.ellipse([175, 110, 185, 125], fill=(0,0,0))
        draw.line([130, 170, 180, 165], fill=(255,255,255), width=3)  # smirk
    elif expression == "excited":
        draw.ellipse([110, 95, 155, 145], fill=(255,255,255))  # big eyes
        draw.ellipse([160, 95, 205, 145], fill=(255,255,255))
        draw.ellipse([125, 110, 140, 130], fill=(0,0,0))
        draw.ellipse([175, 110, 190, 130], fill=(0,0,0))
        draw.ellipse([130, 155, 180, 195], fill=(255,255,255))  # O mouth

    # Antenna
    draw.line([150, 50, 150, 20], fill=(255,255,255), width=3)
    draw.ellipse([140, 10, 160, 30], fill=(255, 255, 0))

    # Name tag
    draw.rectangle([90, 230, 210, 270], fill=(0,0,0), outline=primary, width=2)
    draw.text((150, 250), name, fill=primary, anchor="mm")

    return img

# ✏️ Change the color, expression, and name!
avatar = create_bot_avatar(
    name=bot_name,
    primary=(0, 255, 200),  # Try different RGB colors!
    expression="sarcastic"  # Options: "happy", "sarcastic", "excited"
)
display(avatar)
```

### Free Create Challenges
1. Give your bot a totally unique personality (valley girl alien? Shakespearean gamer? Grumpy chef?)
2. Add 3 new keyword categories with responses
3. Create a different avatar expression
4. **BOSS CHALLENGE:** Make a "mood tracker" — the bot detects your mood using sentiment analysis and responds differently

---

## DAY 4: QUIZ SHOW CREATOR 🎯

### Concept: AI can fill in blanks and generate questions

### Mini-Lesson (10 min)
"AI can play Jeopardy, pass the bar exam, and ace the SATs. But can *you* build a quiz that's harder than what AI can answer? Today you're building a quiz show — and then testing if AI can beat it."

**Vocabulary:** fill-mask, prediction, accuracy

### Guided Build

**Part 1 — AI Question Generator**
```python
# 🎯 QUIZ SHOW ENGINE

# ✏️ PICK YOUR QUIZ THEME
theme = "Space & Science"  # Change this to anything!

# Question bank — ✏️ ADD YOUR OWN!
quiz_data = [
    {"q": "What planet is known as the Red Planet?",
     "options": ["Venus", "Mars", "Jupiter", "Saturn"], "answer": 1},
    {"q": "What gas do plants breathe in?",
     "options": ["Oxygen", "Nitrogen", "Carbon Dioxide", "Helium"], "answer": 2},
    {"q": "How many bones are in the human body?",
     "options": ["106", "206", "306", "406"], "answer": 1},
    {"q": "What is the largest ocean on Earth?",
     "options": ["Atlantic", "Indian", "Arctic", "Pacific"], "answer": 3},
    {"q": "What element does 'O' stand for on the periodic table?",
     "options": ["Osmium", "Oganesson", "Oxygen", "Gold"], "answer": 2},
]

# AI-generated bonus questions using fill-mask
unmasker = pipeline("fill-mask", model="distilbert-base-uncased")

fill_prompts = [
    "The largest planet in our solar system is [MASK].",
    "Water is made of hydrogen and [MASK].",
    "The speed of [MASK] is the fastest thing in the universe.",
]

print(f"🤖 AI BONUS ROUND — AI fills in the blank:\n")
for prompt in fill_prompts:
    results = unmasker(prompt)
    print(f"  Prompt: {prompt}")
    for i, r in enumerate(results[:3]):
        print(f"    {i+1}. {r['token_str']} (confidence: {r['score']:.1%})")
    print()
```

**Part 2 — Interactive Quiz Game**
```python
# 🎮 PLAY THE QUIZ!

score = 0
total = len(quiz_data)

# Shuffle questions
random.shuffle(quiz_data)

print(f"🎯 WELCOME TO THE {theme.upper()} QUIZ SHOW!")
print(f"{'='*50}\n")

for i, q in enumerate(quiz_data):
    print(f"Question {i+1}/{total}: {q['q']}\n")
    for j, opt in enumerate(q['options']):
        print(f"  {j+1}. {opt}")

    try:
        answer = int(input("\nYour answer (1-4): ")) - 1
        if answer == q['answer']:
            score += 1
            print("✅ CORRECT! 🎉\n")
        else:
            print(f"❌ Nope! The answer was: {q['options'][q['answer']]}\n")
    except:
        print("⚠️ Please enter a number 1-4\n")

# Final score
pct = (score / total) * 100
print(f"{'='*50}")
print(f"FINAL SCORE: {score}/{total} ({pct:.0f}%)")
if pct == 100:
    print("🏆 PERFECT SCORE! You're a genius!")
elif pct >= 70:
    print("🌟 Great job! Solid knowledge!")
elif pct >= 40:
    print("📚 Not bad! Keep learning!")
else:
    print("💪 You'll get 'em next time!")
```

**Part 3 — 🎨 ART: Score Visualization**
```python
# 🎨 YOUR QUIZ RESULTS — VISUALIZED

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))

# Pie chart
colors = ['#4CAF50', '#F44336']
ax1.pie([score, total-score], labels=['Correct', 'Wrong'],
        colors=colors, autopct='%1.0f%%', startangle=90,
        textprops={'fontsize': 14, 'fontweight': 'bold'})
ax1.set_title(f'📊 {theme} Quiz Results', fontsize=16, fontweight='bold')

# Fun bar chart — how you compare to "AI"
ai_score = random.randint(max(0, score-2), min(total, score+2))
categories = ['You', 'AI (GPT)', 'Random Guess', 'Expert']
scores = [score, ai_score, total//4, total]
bar_colors = ['#2196F3', '#FF9800', '#9E9E9E', '#4CAF50']

bars = ax2.bar(categories, scores, color=bar_colors, edgecolor='white', linewidth=2)
ax2.set_ylim(0, total + 1)
ax2.set_ylabel('Score', fontsize=12)
ax2.set_title('🏆 Leaderboard', fontsize=16, fontweight='bold')

for bar, s in zip(bars, scores):
    ax2.text(bar.get_x() + bar.get_width()/2., bar.get_height() + 0.1,
             str(s), ha='center', va='bottom', fontweight='bold', fontsize=14)

plt.tight_layout()
plt.savefig('quiz_results.png', dpi=100, bbox_inches='tight')
plt.show()
```

### Free Create Challenges
1. Write a 10-question quiz on YOUR favorite topic
2. Add difficulty levels (Easy / Medium / Hard) with a point multiplier
3. Create a "streak bonus" — extra points for getting 3+ right in a row
4. **BOSS CHALLENGE:** Build "Can AI Beat Your Quiz?" — run the fill-mask model on your questions and see if it gets them right

---

## DAY 5: REAL OR AI DETECTIVE 🔍

### Concept: AI-generated content is everywhere — how do you spot it?

### Mini-Lesson (10 min)
Show 6 short text passages on screen. "3 of these were written by a human. 3 were written by AI. Can you tell which is which?" Students vote with thumbs up/down. Reveal answers. (Most will get some wrong — that's the point.)

**Vocabulary:** deepfake, synthetic, bias

### Guided Build

**Part 1 — AI Text vs Human Text Game**
```python
# 🔍 REAL OR AI? DETECTIVE GAME

# Each pair has one human-written and one AI-generated text
# ✏️ You can add more rounds!

rounds = [
    {
        "topic": "Describing a sunset",
        "text_a": "The sunset painted the sky in shades of orange and pink, casting a warm glow over the quiet town. Birds flew home in perfect V-formations as the day came to a peaceful end.",
        "text_b": "The sun went down and it was pretty I guess. The sky got all orange and stuff. My dog was barking at nothing which was annoying but whatever the clouds looked cool.",
        "ai_is": "A",
        "explanation": "Text A is AI — notice how it's 'perfect' and poetic with no personality. Text B has a real human voice with digressions and opinions."
    },
    {
        "topic": "Writing a restaurant review",
        "text_a": "Went there for my birthday. The pasta was insane, like actually insane. Service was kinda slow but our waiter was funny so we didn't care. Would go back, maybe not on a Saturday tho, way too packed.",
        "text_b": "This restaurant offers a delightful dining experience with exceptional cuisine and attentive service. The pasta dishes are particularly noteworthy, featuring fresh ingredients and authentic flavors. Highly recommended for special occasions.",
        "ai_is": "B",
        "explanation": "Text B is AI — it's generic, uses fancy words but says nothing specific. Text A has real details, casual tone, and specific opinions."
    },
    {
        "topic": "Explaining why dogs are great",
        "text_a": "Dogs are remarkable companions that provide unconditional love and emotional support. Studies have shown that dog ownership can reduce stress, lower blood pressure, and increase physical activity levels.",
        "text_b": "My dog literally ate my shoe yesterday and I'm still not mad at her. That's the power of dogs. They destroy your stuff and you're like 'aw she's so cute tho.' It makes no sense. 10/10 animals.",
        "ai_is": "A",
        "explanation": "Text A is AI — it reads like a Wikipedia article. Text B has a real person's humor and specific anecdote."
    },
]

score = 0
for i, round_data in enumerate(rounds):
    print(f"\n{'='*50}")
    print(f"ROUND {i+1}: {round_data['topic'].upper()}")
    print(f"{'='*50}")
    print(f"\n📄 Text A:\n\"{round_data['text_a']}\"\n")
    print(f"📄 Text B:\n\"{round_data['text_b']}\"\n")

    guess = input("Which one is AI? (A or B): ").strip().upper()
    if guess == round_data['ai_is']:
        print("✅ CORRECT! Great detective work!")
        score += 1
    else:
        print(f"❌ It was actually Text {round_data['ai_is']}!")
    print(f"💡 How to tell: {round_data['explanation']}")

print(f"\n🔍 DETECTIVE SCORE: {score}/{len(rounds)}")
```

**Part 2 — AI Detector Tool (Build Your Own)**
```python
# 🔍 BUILD YOUR OWN AI DETECTOR
# These are real patterns that help spot AI text!

def ai_detector(text):
    clues = []
    score = 0

    # Check 1: Sentence length variety
    sentences = text.split('.')
    sentence_lengths = [len(s.split()) for s in sentences if s.strip()]
    if sentence_lengths:
        variety = max(sentence_lengths) - min(sentence_lengths)
        if variety < 5:
            clues.append("⚠️ Sentences are all similar length (AI tends to be uniform)")
            score += 20

    # Check 2: Overused AI words
    ai_words = ["delightful", "furthermore", "exceptional", "noteworthy",
                 "remarkable", "comprehensive", "innovative", "leverage",
                 "facilitate", "utilize", "underscore", "tapestry"]
    found_words = [w for w in ai_words if w.lower() in text.lower()]
    if found_words:
        clues.append(f"⚠️ Uses typical AI words: {', '.join(found_words)}")
        score += 15 * len(found_words)

    # Check 3: Too many commas (AI loves long, complex, structured sentences)
    comma_ratio = text.count(',') / max(len(text.split()), 1)
    if comma_ratio > 0.3:
        clues.append("⚠️ Very high comma usage (AI writes lots of listed structures)")
        score += 15

    # Check 4: Personal pronouns
    personal_words = sum(1 for w in text.lower().split()
                        if w in ['i', 'my', 'me', "i'm", "i'd", "i've"])
    if personal_words == 0 and len(text.split()) > 20:
        clues.append("⚠️ No personal pronouns in a long text (humans usually say 'I')")
        score += 20

    # Check 5: Contractions
    contractions = ["don't", "can't", "won't", "isn't", "wouldn't", "it's",
                    "that's", "there's", "we're", "they're", "I'm", "I've"]
    has_contractions = any(c in text.lower() for c in contractions)
    if not has_contractions and len(text.split()) > 15:
        clues.append("⚠️ No contractions (humans naturally use them)")
        score += 15

    # Verdict
    score = min(score, 100)
    print(f"\n🔍 AI DETECTION REPORT")
    print(f"{'='*40}")
    if clues:
        for clue in clues:
            print(f"  {clue}")
    else:
        print("  ✅ No major AI red flags detected")
    print(f"\n  AI PROBABILITY: {score}%")
    print(f"  {'█' * (score//5)}{'░' * (20 - score//5)}")

    if score >= 60:
        print("  🤖 VERDICT: Probably AI-generated")
    elif score >= 30:
        print("  🤔 VERDICT: Suspicious — could go either way")
    else:
        print("  👤 VERDICT: Probably human-written")

# ✏️ TEST IT! Paste any text in here:
test = "This restaurant offers a delightful dining experience with exceptional cuisine."
ai_detector(test)
```

**Part 3 — 🎨 ART: AI Art vs. Procedural Art Comparison**
```python
# 🎨 CAN YOU TELL WHICH ART STYLE IS "MORE AI"?
# Generate two different art styles and compare

fig, axes = plt.subplots(1, 2, figsize=(12, 5))

# Style 1: Orderly, "perfect" (AI-like)
ax1 = axes[0]
for i in range(10):
    for j in range(10):
        color = plt.cm.viridis(i * 10 + j)
        rect = plt.Rectangle((i, j), 0.9, 0.9, color=color)
        ax1.add_patch(rect)
ax1.set_xlim(0, 10)
ax1.set_ylim(0, 10)
ax1.set_aspect('equal')
ax1.set_title("Art Style A: Perfectly Ordered", fontsize=14, fontweight='bold')
ax1.axis('off')

# Style 2: Messy, organic (human-like)
ax2 = axes[1]
for _ in range(80):
    x, y = random.gauss(5, 2), random.gauss(5, 2)
    size = random.uniform(0.1, 1.5)
    color = (random.random(), random.random(), random.random(), 0.6)
    shape = random.choice(['circle', 'square'])
    if shape == 'circle':
        circle = plt.Circle((x, y), size/2, color=color)
        ax2.add_patch(circle)
    else:
        angle = random.uniform(0, 45)
        rect = plt.Rectangle((x, y), size, size * random.uniform(0.5, 1.5),
                             angle=angle, color=color)
        ax2.add_patch(rect)
ax2.set_xlim(-2, 12)
ax2.set_ylim(-2, 12)
ax2.set_aspect('equal')
ax2.set_title("Art Style B: Organic & Messy", fontsize=14, fontweight='bold')
ax2.axis('off')

plt.suptitle("🔍 Which One Feels More Human?", fontsize=16, fontweight='bold')
plt.tight_layout()
plt.show()
```

### Free Create Challenges
1. Write 2 paragraphs — one as yourself, one pretending to be AI. Can your neighbor tell which is which?
2. Run your AI detector on 5 different texts — see if it works!
3. Add a new detection rule to the detector (what patterns do YOU notice?)
4. **BOSS CHALLENGE:** Test the detector on text generated by the Day 3 chatbot. Does it catch it?

---

## DAY 6: STORY ADVENTURE STUDIO 📖

### Concept: AI can branch narratives and create interactive fiction

### Mini-Lesson (10 min)
"Remember Choose Your Own Adventure books? You're building one — but powered by AI. Every choice leads somewhere different, and AI helps write what happens next." Demo: show a 3-choice story branching on screen.

**Vocabulary:** branching, narrative, procedural generation

### Guided Build

**Part 1 — Choose Your Own Adventure Engine**
```python
# 📖 STORY ADVENTURE ENGINE

generator = pipeline("text-generation", model="distilgpt2")

# ✏️ CUSTOMIZE YOUR STORY WORLD
story_title = "THE CURSED SCHOOL"
story_setting = "a mysterious school where the hallways change every night"

# Story nodes — ✏️ WRITE YOUR OWN BRANCHES!
story = {
    "start": {
        "text": f"""You wake up in {story_setting}.
The lights flicker. Your phone has no signal.
You hear footsteps coming from two directions...""",
        "choices": {
            "1": {"text": "Follow the footsteps to the LEFT (toward the library)", "next": "library"},
            "2": {"text": "Follow the footsteps to the RIGHT (toward the gym)", "next": "gym"},
            "3": {"text": "Stay put and hide under a desk", "next": "hide"},
        }
    },
    "library": {
        "text": """You push open the library doors. Every book is floating in midair,
pages turning by themselves. One book flies toward you and opens
to a page that reads: 'THE EXIT IS WHERE KNOWLEDGE SLEEPS.'""",
        "choices": {
            "1": {"text": "Search the librarian's desk", "next": "desk"},
            "2": {"text": "Try to read another floating book", "next": "book"},
        }
    },
    "gym": {
        "text": """The gym is completely empty... except the scoreboard is counting
down from 60. Every time a number ticks, the room gets slightly smaller.
The walls are literally closing in.""",
        "choices": {
            "1": {"text": "Look for a door behind the bleachers", "next": "escape"},
            "2": {"text": "Try to stop the scoreboard", "next": "scoreboard"},
        }
    },
    "hide": {
        "text": """You duck under a desk. The footsteps get closer... then stop.
A note slides under the desk toward you. It says:
'Nice try. But hiding won't save you here. — The School'""",
        "choices": {
            "1": {"text": "Okay fine, go LEFT toward the library", "next": "library"},
            "2": {"text": "Okay fine, go RIGHT toward the gym", "next": "gym"},
        }
    },
    # AI generates endings for nodes that don't have written content
}

def play_story():
    current = "start"
    print(f"\n{'='*50}")
    print(f"  📖 {story_title}")
    print(f"{'='*50}\n")

    while current in story:
        node = story[current]
        print(f"\n{node['text']}\n")

        for key, choice in node['choices'].items():
            print(f"  [{key}] {choice['text']}")

        pick = input("\nYour choice: ").strip()
        if pick in node['choices']:
            next_node = node['choices'][pick]['next']
            if next_node in story:
                current = next_node
            else:
                # AI generates the ending!
                print("\n🤖 [AI is writing what happens next...]\n")
                prompt = f"In {story_setting}, the hero chose to {node['choices'][pick]['text'].lower()}. Suddenly,"
                result = generator(prompt, max_length=100, temperature=0.9)
                print(result[0]['generated_text'])
                print("\n🔚 THE END (AI-generated ending!)")
                break
        else:
            print("Invalid choice, try again!")

play_story()
```

**Part 2 — 🎨 ART: Scene Illustrator**
```python
# 🎨 GENERATE ART FOR EACH SCENE

def illustrate_scene(scene_name, mood="mysterious"):
    img = Image.new('RGB', (500, 350), (10, 10, 25))
    draw = ImageDraw.Draw(img)

    # Mood-based color palettes
    palettes = {
        "mysterious": [(80, 50, 130), (120, 70, 180), (200, 150, 255)],
        "danger": [(150, 30, 30), (200, 50, 20), (255, 100, 50)],
        "discovery": [(30, 100, 150), (50, 180, 200), (100, 230, 255)],
        "dark": [(20, 20, 40), (40, 40, 60), (80, 80, 100)],
    }
    colors = palettes.get(mood, palettes["mysterious"])

    # Background elements based on scene
    if "library" in scene_name.lower():
        # Floating books
        for _ in range(15):
            x, y = random.randint(20, 480), random.randint(20, 280)
            w, h = random.randint(20, 40), random.randint(25, 50)
            angle = random.randint(-30, 30)
            color = random.choice(colors)
            draw.rectangle([x, y, x+w, y+h], fill=color, outline=(200,200,200))
            draw.line([x+w//2, y, x+w//2, y+h], fill=(200,200,200))  # spine
    elif "gym" in scene_name.lower():
        # Closing walls
        wall_pos = random.randint(50, 150)
        draw.rectangle([0, 0, wall_pos, 350], fill=colors[0])
        draw.rectangle([500-wall_pos, 0, 500, 350], fill=colors[0])
        # Scoreboard
        draw.rectangle([180, 30, 320, 100], fill=(0,0,0), outline=colors[2], width=2)
        draw.text((250, 65), str(random.randint(10,59)), fill=colors[2], anchor="mm")
    else:
        # Generic mysterious scene
        for _ in range(30):
            x, y = random.randint(0, 500), random.randint(0, 350)
            r = random.randint(5, 50)
            color = (*random.choice(colors), )
            draw.ellipse([x-r, y-r, x+r, y+r], fill=color)

    # Title bar
    draw.rectangle([0, 300, 500, 350], fill=(0, 0, 0))
    draw.text((250, 325), scene_name.upper(), fill=(255,255,255), anchor="mm")

    return img

# Generate art for each scene
for scene_name, mood in [("Library", "mysterious"), ("Gym", "danger"), ("Hiding", "dark")]:
    scene_img = illustrate_scene(scene_name, mood)
    print(f"\n🎨 Scene: {scene_name}")
    display(scene_img)
```

### Free Create Challenges
1. Add 3 more story branches (each with at least 2 choices)
2. Create a full ending for every path (so AI doesn't have to generate them)
3. Illustrate all your scenes with different moods
4. **BOSS CHALLENGE:** Add an inventory system — the player picks up items that affect later choices

---

## DAY 7: INVENTION LAB 🔬

### Concept: AI excels at combining unrelated ideas into something new

### Mini-Lesson (10 min)
"Some of the best inventions are just two existing things smashed together. Smartphone = phone + computer. Labradoodle = Labrador + Poodle. Today, AI helps you invent something the world has never seen — and you'll build a blueprint for it."

**Vocabulary:** combinatorial creativity, prototype, iteration

### Guided Build

**Part 1 — AI Invention Generator**
```python
# 🔬 THE INVENTION LAB

objects = [
    "toothbrush", "skateboard", "umbrella", "backpack", "headphones",
    "sneakers", "water bottle", "hoodie", "bicycle", "sunglasses",
    "pillow", "notebook", "flashlight", "clock", "mirror"
]

technologies = [
    "AI-powered", "solar-powered", "holographic", "voice-activated",
    "mood-sensing", "self-healing", "invisible", "gravity-defying",
    "shapeshifting", "time-tracking", "emotion-reading", "biodegradable"
]

purposes = [
    "that helps you study better", "that makes chores fun",
    "that helps shy people make friends", "that translates animal sounds",
    "that reminds you to drink water", "that predicts the weather with your mood",
    "that helps you find lost things", "that makes boring classes interesting",
    "that helps you sleep better", "that turns exercise into a video game"
]

def generate_invention():
    obj = random.choice(objects)
    tech = random.choice(technologies)
    purpose = random.choice(purposes)
    name_prefixes = ["The", "Project", "Ultra", "Neo", "Mega", "i"]
    name_suffixes = ["tron", "matic", "ify", "X", "Pro", "3000", "Plus"]
    name = f"{random.choice(name_prefixes)}{obj.title()}{random.choice(name_suffixes)}"

    print(f"💡 INVENTION: {name}")
    print(f"📦 What is it: A {tech} {obj} {purpose}")
    return name, obj, tech, purpose

# Generate 3 inventions — pick your favorite!
print("🔬 GENERATING 3 RANDOM INVENTIONS...\n")
inventions = []
for i in range(3):
    print(f"--- Invention #{i+1} ---")
    inv = generate_invention()
    inventions.append(inv)
    print()
```

**Part 2 — AI Product Description**
```python
# 🤖 AI writes your product pitch

generator = pipeline("text-generation", model="distilgpt2")

# ✏️ Pick your favorite invention from above (or type your own!)
chosen_name = inventions[0][0]  # Change the index to pick a different one
chosen_desc = f"a {inventions[0][2]} {inventions[0][1]} {inventions[0][3]}"

prompt = f"Introducing {chosen_name}, {chosen_desc}. This revolutionary product will change your life because"
result = generator(prompt, max_length=120, temperature=0.8)
print(f"🎤 AI PRODUCT PITCH:\n")
print(result[0]['generated_text'])
```

**Part 3 — 🎨 ART: Invention Blueprint**
```python
# 🎨 CREATE YOUR INVENTION BLUEPRINT

def create_blueprint(name, obj, tech, purpose):
    # Blueprint style (dark blue background, white/cyan lines)
    img = Image.new('RGB', (600, 450), (15, 30, 60))
    draw = ImageDraw.Draw(img)

    # Grid lines (blueprint style)
    for x in range(0, 600, 30):
        draw.line([(x, 0), (x, 450)], fill=(25, 50, 80), width=1)
    for y in range(0, 450, 30):
        draw.line([(0, y), (600, y)], fill=(25, 50, 80), width=1)

    # Main object shape (abstract representation)
    cx, cy = 300, 200
    main_color = (100, 200, 255)

    # Draw a unique shape based on the object type
    shapes = random.randint(4, 8)
    points = []
    for i in range(shapes):
        angle = (i / shapes) * 6.28
        r = random.randint(40, 100)
        px = int(cx + r * __import__('math').cos(angle))
        py = int(cy + r * __import__('math').sin(angle))
        points.append((px, py))
    draw.polygon(points, outline=main_color, fill=None)
    draw.polygon(points, outline=(150, 230, 255))

    # Internal details
    for _ in range(5):
        x1 = cx + random.randint(-60, 60)
        y1 = cy + random.randint(-60, 60)
        draw.ellipse([x1-8, y1-8, x1+8, y1+8], outline=main_color)
        draw.line([(x1, y1), (x1 + random.randint(-30,30), y1 + random.randint(-30,30))],
                  fill=main_color, width=1)

    # Dimension lines
    draw.line([(100, 320), (500, 320)], fill=(200, 200, 200), width=1)
    draw.line([(100, 315), (100, 325)], fill=(200, 200, 200), width=1)
    draw.line([(500, 315), (500, 325)], fill=(200, 200, 200), width=1)
    draw.text((300, 330), f"← {random.randint(10,50)}cm →", fill=(200, 200, 200), anchor="mm")

    # Labels
    draw.text((300, 20), f"BLUEPRINT: {name.upper()}", fill=(255, 255, 255), anchor="mm")
    draw.text((300, 400), f"{tech.upper()} | {obj.upper()}", fill=main_color, anchor="mm")
    draw.text((300, 425), f"Purpose: {purpose}", fill=(150, 150, 200), anchor="mm")

    # Annotations with lines
    annotations = [
        (450, 80, f"{tech}\nmodule"),
        (100, 80, "power\nsource"),
        (500, 250, "user\ninterface"),
    ]
    for ax, ay, label in annotations:
        draw.line([(ax, ay), (cx + random.randint(-30,30), cy + random.randint(-30,30))],
                  fill=(200, 200, 100), width=1)
        draw.text((ax, ay), label, fill=(200, 200, 100), anchor="mm")

    # Stamp
    draw.rectangle([10, 370, 140, 440], outline=(200, 100, 100))
    draw.text((75, 390), "STATUS:", fill=(200, 100, 100), anchor="mm")
    draw.text((75, 415), "PROTOTYPE", fill=(200, 100, 100), anchor="mm")

    return img

name, obj, tech, purpose = inventions[0]
blueprint = create_blueprint(name, obj, tech, purpose)
display(blueprint)
```

### Free Create Challenges
1. Generate 10 inventions and pick the 3 best — save them for Day 8
2. Add your own objects, technologies, and purposes to the lists
3. Customize the blueprint colors and add more annotations
4. **BOSS CHALLENGE:** Build an "Invention Battle" — generate 2 inventions, use sentiment analysis on their descriptions, and the more positive one wins

---

## DAY 8: AI SHARK TANK 🦈

### Concept: Using AI as a creative tool to build and pitch a product

### Mini-Lesson (10 min)
"Today you're entrepreneurs. You have ONE invention (from yesterday or a new one). You need to build a pitch: a name, a logo, a tagline, a 30-second pitch, and a demo. AI is your entire marketing department."

**Vocabulary:** pitch, prototype, iterate, MVP

### Guided Build

**Part 1 — Pitch Package Generator**
```python
# 🦈 SHARK TANK PITCH BUILDER

# ✏️ DEFINE YOUR PRODUCT (use yesterday's invention or create a new one!)
product_name = "The NeoUmbrellatron"
product_what = "An AI-powered umbrella that predicts rain 10 minutes before it happens"
product_who = "students who walk to school"
product_why = "because getting soaked before first period ruins your whole day"

generator = pipeline("text-generation", model="distilgpt2")

# Generate tagline options
print("📝 TAGLINE OPTIONS:\n")
tagline_starters = [
    f"{product_name}: The future of",
    f"Never again will you",
    f"{product_name} — because",
]
for starter in tagline_starters:
    result = generator(starter, max_length=30, temperature=1.0)
    tagline = result[0]['generated_text'].split('.')[0] + "."
    print(f"  • {tagline}")

# Generate pitch script
print(f"\n🎤 30-SECOND PITCH SCRIPT:\n")
pitch_template = f"""
Ladies and gentlemen, sharks... imagine this:

You're {product_who}, and {product_why}.

Introducing {product_name} — {product_what}.

Here's how it works:
  1. [EXPLAIN THE TECH — ✏️ fill this in!]
  2. [SHOW THE BENEFIT — ✏️ fill this in!]
  3. [THE WOW MOMENT — ✏️ fill this in!]

We're asking for $[AMOUNT] for [PERCENTAGE]% of the company.

{product_name}: [YOUR TAGLINE HERE]
"""
print(pitch_template)
```

**Part 2 — 🎨 ART: Logo & Brand Generator**
```python
# 🎨 GENERATE YOUR PRODUCT LOGO

def create_logo(name, colors=None):
    if colors is None:
        colors = {
            'primary': (random.randint(50,200), random.randint(50,200), random.randint(100,255)),
            'accent': (random.randint(200,255), random.randint(200,255), random.randint(50,150)),
            'bg': (15, 15, 25)
        }

    img = Image.new('RGB', (400, 400), colors['bg'])
    draw = ImageDraw.Draw(img)

    # Abstract logo mark
    cx, cy = 200, 170
    # Geometric shape layers
    for i in range(5, 0, -1):
        size = i * 25
        alpha_color = tuple(min(255, c + i*15) for c in colors['primary'])
        draw.regular_polygon((cx, cy, size), n_sides=random.choice([3,4,5,6]),
                            fill=alpha_color, outline=colors['accent'])

    # Inner icon (simple shape)
    draw.ellipse([cx-20, cy-20, cx+20, cy+20], fill=colors['accent'])

    # Product name
    # Split long names across lines
    words = name.split()
    if len(words) >= 2:
        draw.text((200, 300), words[0].upper(), fill=(255,255,255), anchor="mm")
        draw.text((200, 340), ' '.join(words[1:]).upper(), fill=colors['accent'], anchor="mm")
    else:
        draw.text((200, 320), name.upper(), fill=(255,255,255), anchor="mm")

    # Subtle tagline area
    draw.line([(100, 370), (300, 370)], fill=colors['primary'], width=1)
    draw.text((200, 385), "EST. 2026", fill=(100,100,100), anchor="mm")

    return img

logo = create_logo(product_name)
display(logo)

# Generate 3 color variations
print("\n🎨 COLOR VARIATIONS:")
fig, axes = plt.subplots(1, 3, figsize=(12, 4))
color_schemes = [
    {'primary': (0, 150, 255), 'accent': (255, 200, 50), 'bg': (15, 15, 30)},
    {'primary': (200, 50, 100), 'accent': (255, 255, 255), 'bg': (25, 10, 20)},
    {'primary': (50, 200, 100), 'accent': (200, 255, 200), 'bg': (10, 25, 15)},
]
for ax, scheme in zip(axes, color_schemes):
    variation = create_logo(product_name, scheme)
    ax.imshow(variation)
    ax.axis('off')
plt.tight_layout()
plt.show()
```

**Part 3 — Pitch Presentation Slides (Text)**
```python
# 📊 AUTO-GENERATE YOUR PITCH DECK (text version)

slides = [
    f"SLIDE 1 — TITLE\n{'='*30}\n{product_name}\n{product_what}\n",
    f"SLIDE 2 — THE PROBLEM\n{'='*30}\nEvery day, {product_who} face this:\n{product_why}\nThis affects MILLIONS of people.\n",
    f"SLIDE 3 — THE SOLUTION\n{'='*30}\n{product_name}\n{product_what}\nIt's that simple.\n",
    f"SLIDE 4 — HOW IT WORKS\n{'='*30}\n1. [Step 1 — ✏️ FILL IN]\n2. [Step 2 — ✏️ FILL IN]\n3. [Step 3 — ✏️ FILL IN]\n",
    f"SLIDE 5 — THE ASK\n{'='*30}\nWe're looking for: $___\nFor: ___% equity\nFunds will be used for: ___\n",
]

for slide in slides:
    print(slide)
```

### The Shark Tank Event (Last 30 min)
Change the schedule for Day 8:
- 0:00–0:15: Hook + Mini-lesson
- 0:15–1:00: Build pitch package (code above)
- 1:00–1:15: Practice pitches in pairs
- **1:15–1:55: SHARK TANK PRESENTATIONS (2 min each + 1 min Q&A)**
- 1:55–2:00: Sharks (teachers/volunteers) invest fake money. Announce winner.

**Judging Criteria** (post on board):
| Category | Points |
|----------|--------|
| Creativity of invention | /25 |
| Quality of pitch delivery | /25 |
| Logo & brand design | /25 |
| "Would you actually buy this?" | /25 |

---

## DAY 9: AI SHOWCASE BUILDER + AI OLYMPICS 🏆

### Concept: Reflecting on what you've built and competing in AI challenges

Split this day into two halves.

### HALF 1: Portfolio Builder (0:00–0:50)

**Students build a showcase of their best work from the camp.**

```python
# 🏆 YOUR AI CAMP PORTFOLIO

# ✏️ Fill in YOUR highlights from each day!
portfolio = {
    "name": "YOUR NAME HERE",
    "camp": "AI Camp 2026",
    "projects": [
        {"day": 1, "title": "Superhero Creator", "highlight": "✏️ What was your best hero?"},
        {"day": 2, "title": "Translator Showdown", "highlight": "✏️ Which phrase mutated the most?"},
        {"day": 3, "title": "Sidekick Chatbot", "highlight": "✏️ What personality did your bot have?"},
        {"day": 4, "title": "Quiz Show", "highlight": "✏️ What topic was your quiz?"},
        {"day": 5, "title": "AI Detective", "highlight": "✏️ Best trick you learned to spot AI?"},
        {"day": 6, "title": "Story Adventure", "highlight": "✏️ What was your story about?"},
        {"day": 7, "title": "Invention Lab", "highlight": "✏️ What did you invent?"},
        {"day": 8, "title": "Shark Tank", "highlight": "✏️ How did your pitch go?"},
    ],
    "favorite_day": "✏️ Which day was your favorite and why?",
    "ai_takeaway": "✏️ What's one thing you learned about AI this camp?",
}

# Display portfolio
print(f"{'='*60}")
print(f"  🏆 AI CAMP PORTFOLIO: {portfolio['name']}")
print(f"  {portfolio['camp']}")
print(f"{'='*60}\n")

for p in portfolio['projects']:
    print(f"  Day {p['day']}: {p['title']}")
    print(f"  └── {p['highlight']}\n")

print(f"\n⭐ Favorite Day: {portfolio['favorite_day']}")
print(f"\n🧠 My AI Takeaway: {portfolio['ai_takeaway']}")
```

**🎨 ART: Portfolio Cover**
```python
# 🎨 GENERATE YOUR PORTFOLIO COVER

def create_portfolio_cover(name):
    img = Image.new('RGB', (500, 650), (15, 15, 30))
    draw = ImageDraw.Draw(img)

    # Decorative generative background
    for _ in range(100):
        x = random.randint(0, 500)
        y = random.randint(0, 650)
        r = random.randint(2, 15)
        color = random.choice([
            (100, 150, 255, 80), (255, 100, 150, 80),
            (100, 255, 200, 80), (255, 200, 100, 80)
        ])[:3]
        draw.ellipse([x-r, y-r, x+r, y+r], fill=color)

    # Title block
    draw.rectangle([30, 200, 470, 450], fill=(0, 0, 0, 180), outline=(255,255,255), width=2)
    draw.text((250, 260), "AI CAMP", fill=(100, 200, 255), anchor="mm")
    draw.text((250, 310), "2026", fill=(255, 200, 100), anchor="mm")
    draw.text((250, 370), name.upper(), fill=(255, 255, 255), anchor="mm")
    draw.text((250, 410), "Project Portfolio", fill=(180, 180, 180), anchor="mm")

    # Tech decorations
    for i in range(8):
        y = 500 + i * 18
        icon = ["🦸", "😂", "🤖", "🎯", "🔍", "📖", "🔬", "🦈"][i]
        day_name = ["Hero", "Translate", "Bot", "Quiz", "Detective", "Story", "Invent", "Shark"][i]
        draw.text((60, y), f"Day {i+1}: {day_name}", fill=(120, 120, 150))

    return img

cover = create_portfolio_cover(portfolio['name'])
display(cover)
```

### HALF 2: AI Olympics (0:50–2:00)

**5 timed challenges. Teams of 2-3. Points on the board.**

```python
# 🏅 AI OLYMPICS — CHALLENGE TIMER

import time

challenges = [
    {
        "name": "⚡ SPEED GENERATOR",
        "time_minutes": 8,
        "instructions": """Generate the FUNNIEST superhero using Day 1's code.
Each team gets 8 minutes. Class votes on the winner.
JUDGING: Creativity of name + power + weakness combo."""
    },
    {
        "name": "🎭 VIBE FLIP",
        "time_minutes": 8,
        "instructions": """Write a sentence that FOOLS the sentiment analyzer.
Goal: Write something POSITIVE that the AI thinks is NEGATIVE (or vice versa).
Each successful fool = 1 point. Most points wins."""
    },
    {
        "name": "🔍 DETECTIVE SHOWDOWN",
        "time_minutes": 8,
        "instructions": """Each team writes 2 paragraphs: one human, one trying to sound like AI.
Other teams guess which is which.
Points: +1 for correct guess, +1 for fooling another team."""
    },
    {
        "name": "🎨 SPEED ART",
        "time_minutes": 8,
        "instructions": """Modify any Day's art code to create the COOLEST image.
Change colors, shapes, sizes, patterns — anything goes.
Class votes on the best artwork."""
    },
    {
        "name": "🎤 60-SECOND PITCH",
        "time_minutes": 10,
        "instructions": """Generate a BRAND NEW random invention.
You have 5 minutes to prep, then each team pitches for 60 seconds.
Sharks (teachers) score: Creativity + Delivery + Would-You-Buy-It."""
    },
]

print("🏅 AI OLYMPICS — EVENT SCHEDULE\n")
for i, c in enumerate(challenges):
    print(f"  Event {i+1}: {c['name']} ({c['time_minutes']} min)")
    print(f"  {c['instructions']}\n")
```

### Scoring & Awards
| Award | For |
|-------|-----|
| 🥇 Grand Champion | Highest total points |
| 🧠 AI Whisperer | Best at prompting/fooling AI |
| 🎨 Digital Picasso | Best artwork created with code |
| 😂 Comedy Gold | Funniest creation across all events |
| 🦈 Shark Tank MVP | Best pitch of the day |
| 💡 Most Creative | Most original idea overall |

---

## TEACHER TIPS

### Managing Total Beginners
- **Golden Rule:** If they can click "Run" and see something cool in the first 5 minutes, you've won them.
- Pair fast finishers with slower students — call them "AI Mentors" (they love the title).
- Have a "Stuck? Do This" slide always visible: (1) Re-run the setup cell, (2) Check for red error text, (3) Raise your hand.
- When an error happens, normalize it: "Errors are just the computer talking to you. Let's read what it's saying."

### If Colab Is Slow
- `distilgpt2` and `distilbert-base-uncased` are the smallest models — they work on CPU.
- If the install cell is slow, have students run it first thing and do the Hook while it installs.
- Pre-run notebooks yourself to confirm everything works the morning of.

### Art Integration Notes for Your Director
Every day includes at least one PIL/matplotlib visual output. The art isn't decorative — it's functional: trading cards, translation passports, blueprints, logos, generative patterns, and portfolio covers. Students learn that code IS a creative medium. You can frame this as "computational art" or "generative design" depending on what language your director prefers.

### Extension for Advanced Students
Keep these in your back pocket for kids who finish early:
- Combine two days' projects (chatbot that speaks in translated phrases, quiz about inventions)
- Modify the AI model parameters (temperature, max_length) and observe changes
- Add `input()` interactivity to any project
- Create custom color palettes for all art projects

---

## SUPPLIES CHECKLIST

- [ ] Chromebooks or laptops with Chrome browser
- [ ] Google accounts for all students (for Colab access)
- [ ] Projector/screen for demos
- [ ] Whiteboard/markers for scoring (Days 8-9)
- [ ] Fake money or voting cards for Shark Tank
- [ ] Award certificates (printable) for Day 9
- [ ] WiFi that can handle 20+ simultaneous Colab sessions (test beforehand!)
- [ ] Backup: pre-downloaded screenshots of expected outputs in case WiFi dies
