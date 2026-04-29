# Introduction to Problem Solving — Teaching Plan v2 (with diagrams)

**Module:** Intermediate DSA — Lecture 1
**Duration:** 110 minutes (≈ 1h 50m)
**Audience:** Mixed cohort — beginners + experienced
**Promise:** *"By the end of today, you'll know whether your code finishes in 1 second or 317 years — without ever running it."*

> 📁 **All 12 diagrams** are in `/diagrams/` as both `.svg` (for HackMD/web) and `.png` (for slides/Notion/Word). Use whichever your delivery format prefers.

---

## 0. Session Architecture

| # | Section | Time | Diagram |
|---|---|---|---|
| 1 | Hook + agenda | 5 min | **D01** Zomato 8 PM |
| 2 | Count of Factors — brute force | 15 min | **D02** Cost ladder · Animation 1 |
| 3 | Count of Factors — optimised (√N) | 20 min | **D03** Mirror line · **D04** Rectangles · **D05** Perfect square · Animations 2 & 3 |
| 4 | Math toolbox: ranges, AP, GP | 15 min | **D11** GP tree · **D12** Gauss pairing · Animation 4 |
| 5 | What is an Iteration? | 10 min | **D06** Sequential vs Nested |
| 6 | Comparing 2 algorithms | 10 min | **D07** Algo comparison |
| 7 | Big-O: rules + hierarchy | 20 min | **D08** Big-O hierarchy · **D09** Drop lower terms · Animation 5 |
| 8 | Issues with Big-O | 10 min | **D10** Big-O lies |
| 9 | Live assignment | 15 min | — |

**Buffer:** 5 min reshuffled across Q&A.

---

## 1. The Hook (0:00 – 0:05)

> 📊 **Show Diagram 1 — Zomato 8 PM scenario** (`D01_hook_zomato.png`)

**Instructor delivery (read aloud, energetic):**

*"Imagine you're a backend engineer at Zomato. Friday, 8 PM, peak biryani hour. You ship a feature that ran fine on your laptop with 10 test users. Within 30 seconds of going live, the app is down. Why? At 8 PM, it's not 10 users — it's **18 lakh users a minute**. Your code didn't break. It just got slow. Today we'll learn the exact tool engineers use to predict this disaster **before** it happens, without ever running the code."*

**Three hook variants — pick one based on class energy:**

- **Story-driven** → the Zomato narrative above (matches the diagram).
- **Provocative** → *"How long would your laptop take to find all factors of 10¹⁸? Guess: 1 sec? 1 hour? 1 day?"* → reveal: **317 years.** Then: *"Today, we'll get it down to 10 seconds. Same problem. Same laptop."*
- **Relatable** → *"Have you ever clicked a button and waited… and waited… and the spinner kept spinning? Today you'll learn whose fault that actually is."*

---

## 2. Today's Agenda (0:05 – 0:08)

> **Frame:** *"From this lecture onward, every problem we touch will end with one question: what's the time complexity? It's not optional. It's the engineer's reflex."*

The arc of the day:
1. Count Factors — solve, then optimise via observation
2. Quick math toolbox (ranges, AP, GP)
3. Iteration counting reflex
4. The big question: how do I compare 2 algorithms?
5. The big idea: Big-O notation
6. The honest truth: when Big-O lies

---

## 3. Section 1 — Count of Factors of N (0:08 – 0:23)

### 3.1 Build intuition (90 sec — no code yet)

**Cold-call:**
> *"What is a factor of a number? Don't define it formally — give me an example."*

After 2 student answers, crystallise:
> *"`i` is a factor of `N` if `N % i == 0`."*

### 3.2 Warm-up Quizzes

**Q1:** Number of factors of 24? *(Show factor list: 1, 2, 3, 4, 6, 8, 12, 24 → 8 factors)*
**Q2:** Number of factors of 10? *(1, 2, 5, 10 → 4 factors)*

🎯 **Real-world relevance (30 sec):**
> *"Factor counting shows up in cryptography (RSA depends on factoring being HARD), inventory packing (24 boxes in equal rows), and music theory (4/4 vs 6/8 — many factors)."*

### 3.3 Brute Force Pseudocode

```cpp
function countFactors(N) {
    fac_count = 0
    for (i -> 1 to N) {
        if (N % i == 0)
            fac_count = fac_count + 1
    }
    return fac_count
}
```

> 🎬 **Play [Animation 1: Brute Force dry run for N=12]** — students watch the loop walk i = 1 → 12.

### 3.4 The cost of brute force

⏸️ **Think-Pair-Share (90 sec):**
> *"Servers do roughly 10⁸ operations per second. For N = 10⁸, this loop runs how long? N = 10⁹? N = 10¹⁸?"*

After they discuss:

> 📊 **Show Diagram 2 — Brute force cost ladder** (`D02_brute_force_cost.png`)

🎤 **Punchline:**
> *"At N = 10¹⁸ — would you be alive to see the output? Your kids? Your great-grandkids? At some point, we have to stop blaming the computer and start blaming ourselves."*

---

## 4. Section 2 — The Optimisation: Power of Observation (0:23 – 0:43)

### 4.1 The "wait, what?" moment

Build the observation interactively. Ask the class to call out factor pairs of 24:

> 📊 **Show Diagram 3 — Mirror line** (`D03_mirror_line.png`)

**Cold call:**
> *"What pattern do you see?"*
> Expected: "After row 4, the pairs repeat in reverse."

### 4.2 The mental model

> 📊 **Show Diagram 4 — Rectangles** (`D04_rectangles.png`)

🧠 **Anchor analogy:**
> *"Imagine N as a rectangle of area N. Every factor pair `(i, N/i)` is a way to draw that rectangle. 1×24, 2×12, 3×8, 4×6 — and then 6×4, 8×3 are the SAME rectangles flipped on their side. Once `i` crosses √N, you're rotating rectangles you already counted."*

### 4.3 Distil the rule

```
   Iterate while  i ≤ N/i    ⇔    i × i ≤ N    ⇔    i ≤ √N
```

#### Pseudocode V2 — sqrt(N), no edge case yet

```cpp
function countFactors(N) {
    fac_count = 0
    for (i -> 1 till i*i <= N) {
        if (N % i == 0)
            fac_count = fac_count + 2   // count i AND N/i
    }
    return fac_count
}
```

> 🎬 **Play [Animation 2: sqrt(N) dry run for N=24]**

⏸️ **Quick prediction (60 sec):**
> *"Run this mentally on N = 100. What do you get?"*

Wait. Let learners realise:
```
   N = 100  →  pairs: (1,100), (2,50), (4,25), (5,20), (10,10)
   Loop adds: +2 +2 +2 +2 +2 = 10
   But actual factors of 100 = {1,2,4,5,10,20,25,50,100} = 9 factors
   We OVERCOUNTED by 1!  Because (10,10) is the same number twice.
```

### 4.4 The Edge Case Fix — Perfect Squares

> 📊 **Show Diagram 5 — Perfect square edge case** (`D05_perfect_square.png`)

**Pattern to teach:**
> *"Whenever code has a 'pair-counting' shape, perfect squares are the boss-level edge case. Always ask: what if i and N/i collide?"*

#### Pseudocode V3 — Final, with edge case

```cpp
function countFactors(N) {
    fac_count = 0
    for (i -> 1 till i*i <= N) {
        if (N % i == 0) {
            if (i == N / i)
                fac_count = fac_count + 1   // perfect square: count once
            else
                fac_count = fac_count + 2   // distinct pair: count both
        }
    }
    return fac_count
}
```

> 🎬 **Play [Animation 3: Final version, toggleable N=24/100/36]**
> Use N=24 to show "no perfect square" path, then switch to N=100 or N=36 to show the edge-case branch lighting up.

### 4.5 Recompute the cost

| N | Iterations (√N) | Time | Verdict |
|---|---|---|---|
| 10⁸ | 10⁴ | 0.0001 sec | 🚀 instant |
| 10¹⁸ | 10⁹ | 10 sec | ✅ acceptable |

> ✨ **"317 years → 10 seconds. We didn't get a faster computer. We just MADE AN OBSERVATION."** ✨

### 4.6 Common Mistakes

| ❌ Mistake | ✅ Fix |
|---|---|
| `i <= sqrt(N)` (float call) | `i*i <= N` is faster + no precision bugs |
| Forgetting perfect-square check | Always ask: "can `i == N/i`?" |
| `i*i` overflow at INT_MAX | Use `long long` or rewrite as `i <= N/i` |

### 4.7 Follow-up (homework)

> *"Given N, check if it's prime. A prime has exactly 2 factors. Reuse today's logic. 30 seconds of work."*

---

## 5. Section 3 — Math Toolbox (0:43 – 0:58)

### 5.1 Range Counting — `[a, b]`

**Notation primer:**
- `[a, b]` → both inclusive (closed)
- `(a, b)` → both exclusive (open)

⚡ **Quiz:** Integers in `[3, 10]`?
*Answer:* 8 — {3, 4, 5, 6, 7, 8, 9, 10}

**The formula:**
```
   Count of integers in [a, b]  =  b − a + 1
                                          ▲
                                  the famous "+1" everyone forgets
```

🧠 **Anchor:** *"Fence posts on a 5-meter wall — at 1m, 2m, 3m, 4m, 5m → 5 posts, 4 gaps. The +1 is the starting post."*

### 5.2 Sum of First N Natural Numbers

**Quiz:** `1 + 2 + 3 + … + 100 = ?`

Don't reveal. Tell **Gauss's story (30 sec):**
> *"In 1786, a teacher gave 9-year-old Carl Friedrich Gauss this problem to keep him busy for an hour. He returned in 30 seconds. Here's what he saw."*

> 📊 **Show Diagram 12 — Gauss pairing** (`D12_gauss_pairing.png`)

> 🎬 **Optionally play [Animation 4: pairing animation]** — students watch columns sum to N+1 one by one.

**Generalised:**
```
   1 + 2 + … + N  =  N × (N + 1) / 2
```

### 5.3 Geometric Progression (GP)

**Build intuition:**
```
       5,  10,  20,  40,  80,  …    ← common ratio r = 2
```

**Generic GP:** `a, a·r, a·r², …, a·r^(n−1)`

**Sum formula:**
```
                  r^N − 1
      S_N = a · ─────────       (when r ≠ 1)
                  r − 1
```

### 5.4 Real-world GP

> 📊 **Show Diagram 11 — GP tree (news spreading)** (`D11_gp_tree.png`)

The diagram shows: 1 person → 4 → 16 → 64 → 256 → … → 16,384 at level 8 → **21,845 total**.

🌍 **GP shows up in:**
- 🦠 disease spread (R0 in epidemiology — COVID R0 ≈ 2-3)
- 💰 compound interest (your SIP at 12% p.a.)
- 🚗 car depreciation (~15% loss/year — GP with r=0.85)
- 🌐 viral content on Instagram/X
- 🌳 binary trees (next module)

> **Foreshadowing:** *"GP looks innocent. But when r > 1 and N is big, the numbers EXPLODE. Remember this — when we see `O(2^N)` later today, this is exactly why it's a death sentence."*

---

## 6. Section 4 — What Is an Iteration? (0:58 – 1:08)

**Definition:** An iteration = one full pass of the loop body. Count of iterations = how many times the body runs.

### 6.1 Quick-fire warmup

**Q7:**
```cpp
for (i -> 1 to N) { if (i == N) break; }
```
*Answer: N* (it breaks on the last iteration, but the body still ran N times)

**Q8:**
```cpp
for (i -> 0 to 100) { s = s + i + i*i; }
```
*Answer: **101*** (0, 1, 2, …, 100 → that's the +1 from earlier!)

**Q9:** Two sequential loops over N and M.
*Answer: **N + M*** — not N×M.

> 📊 **Show Diagram 6 — Sequential vs Nested** (`D06_loops.png`)

🧠 **Anchor for Q9:**
> *"5 apples + 7 oranges = 12 fruits. NOT 35. Sequential = ADD."*
> *"For each shirt, try each pant. 3 shirts × 4 pants = 12 outfits. Nested = MULTIPLY."*

⏸️ **Trap question (preview next class):** What if the j-loop is INSIDE the i-loop? → Now it's N × M.

---

## 7. Section 5 — How Do We Compare Two Algorithms? (1:08 – 1:18)

### 7.1 Tell the story

🎬 **Story (deliver with energy):**
> *"There's a coding contest. Sort `{3, 2, 6, 8, 1}` in ascending order. Two contestants — Gaurav and Shila — submit. Same input. Shila's runs in 10 seconds. Gaurav's runs in 15. Judge declares Shila the winner. Gaurav explodes: 'WAIT! She has a MacBook Pro M3. I'm on a 2009 Windows XP!' They re-test on the same machine. Now Gaurav wins by 2 seconds. Shila: 'WAIT! He's in C++. I used Python!' …and on, and on."*

### 7.2 The Diagram

> 📊 **Show Diagram 7 — Algorithm comparison** (`D07_algo_comparison.png`)

This is the centrepiece of the section — a side-by-side: "Why execution time fails" vs "Why iterations work." The flips in the left panel (15→8, 10→6) make the lesson visual.

### 7.3 The breakthrough

> *"Is there ONE measure that doesn't depend on ANY of these factors?"*

Wait. If nobody answers:
> ✨ **Number of iterations.**

Same algorithm + same input → SAME iteration count on a 2009 ThinkPad and a 2025 MacBook. It's a property of the **algorithm**, not the **machine**.

```
   Iterations = the algorithm's "fingerprint"
   Time-in-seconds = the runner's mood that day
```

---

## 8. Section 6 — Big-O Notation (1:18 – 1:38)

### 8.1 What it is

> **Big-O analyses how an algorithm behaves for LARGE inputs.**

Why? Because at N=5, even a terrible algorithm finishes instantly. At N=10⁹, only a good algorithm survives.

### 8.2 The Three Rules

```
   ┌─────────────────────────────────────────────────┐
   │   BIG-O: THE 3 RULES                            │
   │                                                 │
   │   1.  Count iterations as a function of N       │
   │   2.  Drop lower-order terms                    │
   │   3.  Drop constant coefficients                │
   └─────────────────────────────────────────────────┘
```

### 8.3 Worked example

`Iterations: 4N² + 3N + 1`

```
   Step 1:  4N² + 3N + 1
   Step 2:  Drop lower-order:  4N²
   Step 3:  Drop the 4:        N²

   Big-O = O(N²)
```

### 8.4 The Hierarchy

> 📊 **Show Diagram 8 — Big-O hierarchy** (`D08_bigo_hierarchy.png`)

The diagram shows: O(log N) → O(√N) → O(N) → O(N log N) → O(N²) → O(N³) → O(2ᴺ)/O(N!), with iteration counts at N=36 and side panels for "TARGET ZONE" (aim for these) and "DANGER ZONE" (avoid these).

> 🎬 **Play [Animation 5: Big-O Growth slider]** — students drag N from 10 → 10⁸ and watch each row's iteration count and execution time update. Best done as a 2-minute interactive moment where students predict where each row will land.

### 8.5 Quick-fire Big-O Quizzes

**Q10:** `F(N) = 4N + 3N·log(N) + 1` → **O(N·log N)**
**Q11:** `F(N) = 4N·log(N) + 3N·√N + 10⁶` → **O(N·√N)**

> 🛑 **Reminder:** From now on, after EVERY quiz, ask "What's the Big-O?" Build the muscle.

### 8.6 Why drop lower-order terms? (PROOF, not assertion)

> 📊 **Show Diagram 9 — Drop lower terms** (`D09_drop_lower.png`)

The diagram shows `f(N) = N² + 10N` — the contribution of `10N` shrinks from **50%** at N=10 → **0.001%** at N=10⁶. Visual bars make it instantly obvious.

### 8.7 Why drop constants?

| Algo 1 | Algo 2 | Winner at large N |
|---|---|---|
| 10·log₂N | N | Algo 1 |
| 100·log₂N | N | Algo 1 |
| 9·N | N² | Algo 1 |
| 10·N | N²/10 | Algo 1 |
| N·log₂N | 100·N | **Algo 2** |

> *"Even when Algo 2's constant is 100× bigger, if her ASYMPTOTIC class is better, she wins for large N."*

---

## 9. Section 7 — The Honest Truth: Issues with Big-O (1:38 – 1:48)

> *"I've spent 30 minutes selling Big-O. Now let me tell you when it lies."*

### 9.1 Issue 1: Big-O is for LARGE inputs only

> 📊 **Show Diagram 10 — Big-O lies for small N** (`D10_bigo_lies.png`)

The diagram shows two curves crossing at N≈1000. **Below crossover, Algo 2 (N²) wins. Above crossover, Algo 1 (1000·N) wins.** The side panel makes the industry connection — Python's `sort()` uses Insertion Sort O(N²) for arrays under 64 elements precisely for this reason.

### 9.2 Issue 2: Big-O can't distinguish within the same class

```
   Code 1 — counts odd numbers from 1 to N:
   for (i -> 1 to N)
       if (i % 2 != 0) c++;        ← N iterations

   Code 2:
   for (i -> 1 to N step 2)         ← N/2 iterations
       c++;
```

Both are **O(N)**, but Code 2 is literally 2× faster. Big-O can't tell them apart.

### 9.3 Common Big-O Mistakes

| ❌ Mistake | ✅ Reality |
|---|---|
| "O(2N) is worse than O(N)" | They're identical — drop the 2 |
| "O(N + M) = O(N)" when M is huge | NO. Both vars matter. Keep both. |
| "Constants don't matter ever" | They do — for small N or real-time systems |
| Confusing best/worst/average case | Big-O usually = worst-case unless stated |

---

## 10. Wrap-up + Live Assignment (1:48 – 2:00)

### 10.1 30-second recap (chorus answers)

> *"Today we learned…"* — let students fill in:

```
   1.  Brute force factors:        O(N)
   2.  Optimised with observation: O(√N)        ← FIRST WIN
   3.  Compare algos using:        iterations, not seconds
   4.  Big-O rules:                drop constants, drop lower order
   5.  Big-O lies when:            inputs are small, OR same class
```

### 10.2 The takeaway

> ✨ **"In DSA, the most powerful tool isn't the algorithm. It's the OBSERVATION you make BEFORE writing the algorithm."** ✨

### 10.3 Live Assignment — "Count Factors – 2"

1. Unlock the assignment via the **"?"** button.
2. Ask: *"Who wants to share their screen and solve live?"*
3. Grant Audio + Video + Screen permission.
4. As they code, the class predicts:
   - "What's the time complexity of what they're writing?"
   - "Will it pass for N = 10¹⁸?"
5. After solution, ask: *"Where's the perfect-square edge case?"*

### 10.4 Send-off

> *"Next class: arrays. We'll see why scanning is O(N), nested scanning is O(N²), and meet our first GAME-CHANGING technique — **prefix sums** — that turns repeated O(N) queries into O(1). See you Wednesday."*

---

## 11. Instructor Cheat Sheet — When Things Go Wrong

| Where it goes wrong | How to recover |
|---|---|
| Students stuck on `i*i <= N` pattern | Walk through D03 mirror line table on board |
| Beginners confused by GP formula | Skip derivation, give the formula, do 1 plug-in example |
| Class is silent during cold calls | Switch to think-pair-share for 60 sec |
| Advanced students bored during math | Throw the prime-check follow-up |
| "Why drop constants?" debate | Show D09 N=10⁴ row — 0.1% — debate over |
| Time running out at Section 8 | Skip Issue 2 (assign as reading); NEVER skip Section 7 (story is where the "aha" lives) |

---

## 12. Asset Index

### Diagrams (12)

| File | Section | Purpose |
|---|---|---|
| `D01_hook_zomato` | 1. Hook | 3-stage flow: dev → prod → crash |
| `D02_brute_force_cost` | 3.4 Brute force cost | N vs iterations vs time ladder |
| `D03_mirror_line` | 4.1 Optimisation | Factor pairs collapsing past √N |
| `D04_rectangles` | 4.2 Mental model | Factors as rectangles of area N |
| `D05_perfect_square` | 4.4 Edge case | Distinct pair vs i==N/i |
| `D06_loops` | 6. Iterations | Sequential (ADD) vs Nested (MULTIPLY) |
| `D07_algo_comparison` | 7. Comparing algos | Why iterations beat execution time |
| `D08_bigo_hierarchy` | 8.4 Big-O hierarchy | Visual ladder of complexity classes |
| `D09_drop_lower` | 8.6 Drop lower terms | Bar chart proof of % shrinkage |
| `D10_bigo_lies` | 9.1 Issue 1 | Crossover graph showing Big-O lies |
| `D11_gp_tree` | 5.4 GP examples | News-spreading branching tree |
| `D12_gauss_pairing` | 5.2 Sum of N | Gauss's column-pairing trick |

Each diagram is provided as both `.svg` (scalable, for HackMD/web/Notion) and `.png` (1400px wide, for slides/Word/PowerPoint).

### Animations (5)

| File | When to play | Duration |
|---|---|---|
| `Animation_1_BruteForce_DryRun.html` | After pseudocode V1 (Section 3.3) | ~45 sec |
| `Animation_2_SqrtOptimised_DryRun.html` | After pseudocode V2 (Section 4.3) | ~45 sec |
| `Animation_3_FinalCountFactors_DryRun.html` | After pseudocode V3 (Section 4.4) | ~60 sec |
| `Animation_4_SumOfN_Pairing.html` | During Gauss story (Section 5.2) | ~30 sec |
| `Animation_5_BigO_Growth.html` | During Big-O hierarchy (Section 8.4) | ~60 sec |

All animations are self-contained HTML — open directly in a browser, no server needed.
