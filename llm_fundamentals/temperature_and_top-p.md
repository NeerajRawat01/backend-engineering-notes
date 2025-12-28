

# 1️⃣ How an LLM generates text (1-minute recap)

At every step, the model does this:

```
Given previous tokens
→ predict probabilities for next token
→ choose ONE token
→ repeat
```

Example:

```
"I love"
```

Model predicts:

| Token         | Probability |
| ------------- | ----------- |
| " Python"     | 0.52        |
| " coding"     | 0.21        |
| " coffee"     | 0.10        |
| " pizza"      | 0.07        |
| " everything" | 0.10        |

Now the **decoding strategy** decides which token is picked.

That’s where **temperature** and **top-p** come in.

---

# 2️⃣ Temperature (controls randomness)

### Simple definition

> **Temperature controls how random or deterministic the output is**

---

## Temperature = 0 (or very close)

* Always pick **highest-probability token**
* Almost deterministic
* Same input → same output

### Example

```
Prompt: "2 + 2 ="
```

Output (temp = 0):

```
4
```

Always.

---

## Low temperature (0.1 – 0.3)

* Mostly predictable
* Slight variation possible
* Good for **facts, explanations, code**

### Example

```
Explain FastAPI in one sentence
```

Outputs will be very similar each time.

---

## Medium temperature (0.6 – 0.8)

* Balanced creativity
* Common default
* Some variation

### Example

```
Write a LinkedIn bio for a Python developer
```

Each run slightly different.

---

## High temperature (1.0+)

* Very random
* Creative, surprising
* Risk of nonsense

### Example

```
Write a poem about databases
```

Outputs vary wildly.

---

## Very high (1.5+)

* Chaotic
* Hallucinations increase
* Often useless for production

---

# 3️⃣ What temperature actually does (internals)

Temperature **reshapes the probability distribution**.

### Before (temp = 1.0)

```
Python: 0.52
coding: 0.21
coffee: 0.10
pizza: 0.07
```

### After (temp = 0.2)

```
Python: 0.85
coding: 0.10
others: ~0
```

### After (temp = 1.2)

```
Python: 0.35
coding: 0.25
coffee: 0.20
pizza: 0.20
```

👉 Lower temp = sharper
👉 Higher temp = flatter

---

# 4️⃣ Top-P (Nucleus Sampling)

### Simple definition

> **Top-p limits token selection to a probability mass**

Instead of:

> “Pick from all tokens”

It says:

> “Pick from the smallest set whose total probability ≥ p”

---

## Example distribution

| Token      | Probability |
| ---------- | ----------- |
| "Python"   | 0.50        |
| "coding"   | 0.20        |
| "coffee"   | 0.15        |
| "pizza"    | 0.10        |
| "unicorns" | 0.05        |

---

## top-p = 0.9

Include tokens until total ≥ 0.9:

```
Python (0.50)
coding (0.20) → 0.70
coffee (0.15) → 0.85
pizza (0.10) → 0.95
```

Allowed tokens:

```
Python, coding, coffee, pizza
```

---

## top-p = 0.6

```
Python (0.50)
coding (0.20) → 0.70
```

Allowed tokens:

```
Python, coding
```

---

## top-p = 1.0

All tokens allowed (no restriction).

---

# 5️⃣ Temperature vs Top-P (VERY IMPORTANT)

### Temperature

* Changes **shape** of probabilities
* Affects randomness globally

### Top-P

* Cuts off **low-probability tail**
* Prevents rare garbage tokens

They are often used **together**.

---

# 6️⃣ Combined example (realistic)

### Prompt

```
Write one sentence about FastAPI
```

---

### temp = 0.2, top-p = 1.0

```
FastAPI is a modern, fast Python framework for building APIs.
```

---

### temp = 0.7, top-p = 0.9

```
FastAPI is a high-performance Python framework designed for building APIs quickly and efficiently.
```

---

### temp = 1.2, top-p = 1.0

```
FastAPI zips through API creation like a caffeinated cheetah in a server room.
```

---

# 7️⃣ When to use what (production mindset)

### Use LOW temperature when:

* Code generation
* API responses
* Documentation
* Math / logic
* Legal / finance

```
temperature: 0 – 0.3
top-p: 0.9 – 1.0
```

---

### Use MEDIUM temperature when:

* General chat
* Explanations
* Summaries

```
temperature: 0.5 – 0.7
top-p: 0.9
```

---

### Use HIGH temperature when:

* Brainstorming
* Creative writing
* Marketing copy

```
temperature: 0.9 – 1.2
top-p: 0.8 – 0.95
```

---

# 8️⃣ Common misconceptions (important)

❌ “Temperature controls correctness”
✅ It controls **randomness**

❌ “Higher temperature = smarter”
✅ Higher temperature = **less reliable**

❌ “top-p and temperature are same”
✅ They solve **different problems**

---

# 9️⃣ One mental model to remember forever

Think of a **dice bag** 🎲

* Temperature = how many dice you shake
* Top-p = which dice are even allowed in the bag

---

# 🔟 Final crisp summary

### Temperature

* Controls randomness
* Lower = deterministic
* Higher = creative but risky

### Top-P

* Limits token choices
* Removes low-probability junk
* Improves coherence

### Together

* You tune **style vs reliability**


