# Bankout: Comprehensive Guide

This guide is your all-in-one, deeply detailed reference to play, teach, judge, and master Bankout. It consolidates Bankout and Blitz formats, Jokers ON/OFF, exact timing windows, edge cases, risk math, strategy, variants, and tournament guidance.

---

## Modes, setup, and structure

- **Formats:**
  - **Bankout:** Best-of-three rounds; each round ends when a player banks $1,000; first to 2 round wins takes the match.
  - **Blitz:** Cumulative scoring across 3 rounds with winner-takes-all pot system; the round ends when the deck runs out; highest total bank after Round 3 wins and collects the entire accumulated pot. Ties go to Sudden Death (Numbers + Jacks only).

- **Jokers setting:**
  - **Jokers ON:** Use round-based distribution and Joker rules.
  - **Jokers OFF:** Remove Jokers entirely.

- **Round flow:**
  - **First player:** Random in Round 1; alternate thereafter.
  - **Deck build per round (if Jokers ON):**
    - **Round 1:** 0 Jokers.
    - **Round 2:** 1 Joker (immediate on draw; not held).
    - **Round 3:** 2 Jokers (to hand; Counter Joker enabled).
  - **Reshuffles:** When the deck empties, shuffle discard to form a new deck; continue.

- **Visibility:**
  - **Public:** Numbers, Jacks, Aces, Joker draws/effects; Loot and Bank totals.
  - **Hidden until played:** Queens, Kings, and (Round 3) Jokers in hand.

- **Quick setup checklist:**
  - **Format:** Bankout or Blitz.
  - **Jokers:** ON or OFF.
  - **First player:** Decide.
  - **Round deck:** Insert correct number of Jokers (if ON).
  - **State trackers:** Die/counters for Alert/Jacks-this-turn; token/marker for Latent Ace.

---

## Components and key states

- **Deck:** Standard 52-card deck (2–10, J, Q, K, A). Add Jokers per round if ON.
- **Player state:**
  - **Bank:** Secured points this round.
  - **Loot:** Unbanked points this turn.
  - **Hand:** Holds only Q, K, and (Round 3) X (Joker).
  - **Alert:** True after ≥1 Jack this turn; false otherwise.
  - **Jacks this turn:** Count of Jacks drawn this turn.
  - **Latent:** Count of Aces drawn off-alert; each threatens when a Jack would set Alert.

- **Hand limit:**
  - **Limit:** 2 cards (Q/K/X only).
  - **Over-limit:** When a card would push you above 2, you must immediately choose discards down to 2. If no valid choice (e.g., time), discard by pre-agreed default (commonly oldest).

---

## Turn flow and timing windows

- **Start of turn:**
  - **Reset:** Loot = 0, Alert = false, Jacks this turn = 0, Latent = 0.
  - **Persist:** Bank and Hand remain.

- **Your options (repeat until turn ends):**
  - **Draw:** Reveal top card; resolve immediately.
  - **Play from hand:** Q, K, and (Round 3) X at instant speed during your turn.
  - **Bank:** Move all Loot to Bank; end turn.
  - **Voluntarily end turn:** Lose Loot; end turn.

- **Turn ends when:**
  - **You bank,** or **bust,** or **play a King,** or **resolve a Joker that ends turn** (swap or King effect), or **voluntarily end.** On end/bust, reset per-turn states and pass play.

- **Exact timing on a draw (priority order):**
  1. **Reveal** the card.
  2. **Immediate bust checks:**
     - **Ace while Alert = true:** Bust now.
     - **Third Jack this turn:** Bust now (subject to step 3).
  3. **Mandatory latent trigger on Jack:** If Latent > 0:
     - **If you have a Queen:** Auto-play one Queen immediately (see Queen).
     - **If not:** Bust immediately.
     - Note: Auto-Queen applies before finalizing "third Jack on table," so it can prevent the 3rd-Jack bust by reducing Jacks this turn.
  4. **Apply the card’s normal effect** (Numbers add Loot, Q/K to hand, Joker per round).
  5. **Enforce hand limit** (choose discards if >2).
  6. **Check end triggers** (King/Joker end-turn effects, round/match thresholds).

- **Instant-speed windows (your turn only):**
  - **Queen window:** Play in response to a Jack reveal before consequences finalize to remove one Jack instance and possibly exit Alert/disarm latent.
  - **King window:** Play at any time; double-and-bank Loot; end turn.
  - **Joker window (Round 3):** Play on your turn for swap, King effect, or Queen effect; see Joker rules.

---

## Card effects

- **Numbers (2–10):**
  - **Effect:** Loot increases by the card’s face value.
  - **After:** Discard.

- **Jack (J):**
  - **Base:** Jacks this turn += 1; set Alert = true.
  - **Third Jack in same turn:** Bust immediately.
  - **Latent interaction:** On revealing a Jack while Latent > 0, auto-play a Queen if held; otherwise bust immediately.

- **Ace (A):**
  - **On Alert:** Immediate bust.
  - **Off Alert:** Latent += 1 (armed; no immediate effect).
  - **Latent scope:** Clears on bust or end of your turn; threatens only when a Jack would (re)establish Alert.

- **Queen (Q):**
  - **Drawn:** Goes to hand (subject to hand limit).
  - **Played (manual or auto):**
    - **Reduce Jacks this turn by 1** (min 0).
    - **If Jacks this turn becomes 0:** Set Alert = false.
    - **Reduce Latent by 1** (min 0).
    - **Discard the Queen.**
    - **Does not end turn.**

- **King (K):**
  - **Drawn:** Goes to hand.
  - **Played:** Bank 2×Temp (double-and-bank), then reset per-turn state (Loot = 0; Alert = false; Jacks this turn = 0; Latent = 0); end turn.

- **Joker (X):**
  - **Round 1:** No Jokers in the deck.
  - **Round 2 (Immediate Joker on draw):**
    - **Three-choice options:** Choose immediately:
      - **Swap banks** (if opponent's Bank > 0): Swap Banks; reset your per-turn state; end turn.
      - **King effect:** Double current Loot and bank it; reset per-turn state; end turn.
      - **Queen effect:** Apply Queen effect; turn continues.
    - **Cannot be held or countered.**
  - **Round 3 (Counter Joker rules):**
    - **Drawn:** Goes to your hand.
    - **Offense (your turn):**
      - **Three-choice options:** Choose:
        - **Swap banks** (if opponent's Bank > 0): Swap Banks; reset per-turn; end turn.
        - **King effect:** Double current Loot and bank it; reset per-turn; end turn.
        - **Queen effect:** Apply Queen effect; turn continues.
    - **Defense (opponent’s turn):**
      - When opponent plays a Joker to swap with you, you may discard a Joker from hand to **counter**:
        - **Both Jokers are discarded.**
        - **No swap occurs.**
        - **Opponent’s turn ends.**
      - Defensive Joker never produces wild effects.

---

## Banking, busts, and endings

- **Bank action:**
  - **Effect:** Bank += Loot; reset per-turn state; end turn.
  - **When:** Any time on your turn before you bust.

- **Bust conditions:**
  - **Third Jack** in a single turn.
  - **Ace drawn while Alert = true.**
  - **Latent triggered on a Jack with no Queen available to auto-play.**
  - **Three Latent Aces:** Drawing a third Ace while already having 2 latent Aces (off Alert).

- **Bust effects:**
  - **Loot:** Lost (Bank unchanged).
  - **Per-turn state:** Reset (Alert off; Jacks this turn = 0; Latent = 0).
  - **Turn:** Ends; pass play.

- **Round and match ends:**
  - **Bankout:** First to bank $1,000 wins the round immediately. First to 2 round wins takes the match.
  - **Blitz:** Highest cumulative Bank after 3 rounds wins and collects the entire accumulated pot.
  - **Sudden Death (Blitz tie):**
    - **Deck:** Numbers + Jacks only.
    - **Bust:** On third Jack on table.
    - **Win:** First to bank $1,000; if no one reaches $1,000, higher Bank wins.

---

## Edge cases and rulings

- **Auto-Queen before third-Jack bust:** On Jack with Latent > 0, the auto-Queen reduces Jacks this turn by 1 before evaluating "third Jack on table," potentially avoiding bust.
- **Multiple Jacks on Alert:** A Queen removes a single Jack instance. You remain on Alert until Jacks this turn reaches 0.
- **Latent threat timing:** Latent Aces only threaten at the instant a Jack would (re)establish Alert; clearing Alert then (via Queen or wild-Queen) prevents the bust.
- **Joker swap priority:** Swaps resolve and end the turn immediately (offense). If a counter is used (defense, Round 3), no swap occurs and the opponent’s turn ends.
- **Illegal over-hand:** If a player forgets to discard down to 2 immediately on addition, judge instructs to discard down to 2 retroactively with minimal information gain (commonly oldest or random selection per event policy).
- **Slow play:** Judges may set reasonable decision timeboxes. Repeated delays can incur warnings and time penalties per tournament policy.

---

## Probability and risk math (practical)

- **Average Number value (fresh deck):** Approximately $6 per safe draw.

- **Bust probabilities (early deck):**
  - **Ace-on-Alert risk:** If all 4 Aces remain in a deck of size \(N\), probability is
    \[
    p_\text{Ace} \approx \frac{4}{N}.
    \]
  - **Third-Jack risk:** If you have 2 Jacks already this turn and \(j\) Jacks remain in \(N\) cards,
    \[
    p_\text{thirdJ} \approx \frac{j}{N}.
    \]
  - **Three-Latent-Aces risk:** If you have 2 latent Aces already and \(a\) Aces remain in \(N\) cards,
    \[
    p_\text{threeLatentAces} \approx \frac{a}{N}.
    \]
  - **Combined next-draw bust risk (approx):**
    \[
    p_\text{bust} \approx p_\text{Ace on Alert} + p_\text{thirdJ} + p_\text{threeLatentAces}.
    \]

- **Draw-or-bank threshold (no King):**
  - Draw if expected value exceeds risked Loot \(L\):
    \[
    (1 - p_\text{bust}) \cdot 6 > p_\text{bust} \cdot L
    \quad\Rightarrow\quad
    L < \frac{1 - p_\text{bust}}{p_\text{bust}} \cdot 6.
    \]

- **Draw-or-double threshold (King in hand):**
  - Before drawing, compare new draw vs. locking the double:
    \[
    2 \cdot 6 > 2 \cdot p_\text{bust} \cdot L
    \quad\Rightarrow\quad
    L < \frac{6}{p_\text{bust}}.
    \]

- **Queen effect on risk:**
  - A Queen effectively removes the Ace component of \(p_\text{bust}\) when used to clear Alert at the latent trigger, substantially improving the safe-draw window.

> Tip: Replace “6” by your real-time expected Number value based on visible counts and known discards.

---

## Strategy for players

- **Universal principles:**
  - **Bank in windows:** Build multiple credible paths to $1,000 instead of one giant push.
  - **Pressure management:** Bank at totals that force opponent into thin, risky lines.
  - **Joker denial (ON):** Keep opponent's Bank > $0 to shut off swap Jokers (but they can still use King/Queen effects).

- **Queen mastery:**
  - **Reactive:** Guard against latent-trigger busts; auto-Queen saves runs.
  - **Proactive:** Clear Alert before big pushes when Loot is high.

- **King mastery:**
  - **Thresholding:** Fire at or above your draw-or-double threshold.
  - **Checkmate lines:** Use King to slam the door when deck danger spikes (many Aces live on Alert).

- **Jokers ON nuances:**
  - **Ahead:** Bank smaller, more often; minimize swap target size.
  - **Behind:** Embrace variance; stretch runs and value Kings.
  - **Counter Joker (Round 3):** Hold one defensively if you’re leading; offensively, time swap for max swing.

- **Format posture:**
  - **Bankout:** Round 1 calibrates; Round 2 leverages tempo; Round 3 plays the opponent — deny Joker lines and create multiple finish paths.
  - **Blitz:** With Jokers OFF, play precision banking; with Jokers ON, adapt pace to scoreboard (leaders protect, trailers push).

---

## Training tools and judge guidance

- **For teachers:**
  - **Onboarding script:** "Numbers make money. Jacks put you on Alert; third Jack on table busts. Any Ace while on Alert busts. Queens remove one Jack and can disarm latent Aces. Kings double-and-bank, ending your turn. Jokers offer three choices: swap banks (if opponent has money), double-and-bank like a King, or remove a Jack like a Queen."
  - **Table aids:** Use a die for Jacks-this-turn, a token for Alert, and a sideways card for Latent Aces.

- **For judges:**
  - **Priority on Jack:** Active player may play Queen before consequences finalize; if Latent > 0, auto-Queen is mandatory when available.
  - **Counter Joker (Round 3):** Defense requires discarding a Joker on the spot; if confirmed, both Jokers are discarded, no swap, opponent’s turn ends.
  - **Hand-limit infractions:** Remedy immediately with minimal info advantage; apply event policy for repeated errors.
  - **Time management:** Enforce consistent, fair decision windows; escalate from reminders to penalties for slow play.

---

## Worked examples

- **Example A: Disarming a latent Ace**
  - **State:** You drew an Ace off-alert earlier (Latent = 1). Now you reveal a Jack.
  - **Action:** Auto-Queen triggers if you have one; it reduces Jacks this turn by 1 and Latent by 1, and may clear Alert.
  - **Result:** No bust; you continue your run.

- **Example B: King threshold**
  - **State:** On Alert with \(N \approx 40\); all Aces live, so \(p_\text{Ace} \approx 4/40 = 0.10\). Loot \(L = \$65\).
  - **Calc:** Draw-or-double threshold \(L^* = 6/0.10 = \$60\).
  - **Action:** Fire King now; bank $130; end turn.

- **Example C: Joker swap denial**
  - **State:** Opponent Bank = $0; one Joker remains in Round 2.
  - **Action:** Bank a small amount (e.g., $12) to push opponent above $0.
  - **Result:** Their Joker swap option is disabled (but they can still use King/Queen effects).

- **Example D: Counter Joker defense (Round 3)**
  - **State:** Opponent plays Joker to swap your large Bank.
  - **Action:** Discard your Joker to counter.
  - **Result:** Both Jokers to discard; no swap; opponent’s turn ends immediately.

---

## Optional variants and tournament operations

- **Optional variants:**
  - **Light Jokers (Blitz):** Only 1 Joker across all three rounds.
  - **Shield Token (Blitz):** Each player gets one per match to cancel a single Joker swap.
  - **Exact Bank Bonus:** +$25 for banking exactly $1,000 (not used in Sudden Death).
  - **Seeding edge (Bankout):** Round winner chooses first/second next round.
  - **Short Blitz:** Two rounds total; if tied, Sudden Death.

- **Tournament operations:**
  - **Default recs:** Bankout with Jokers ON for spectacle; Blitz with Jokers OFF for competitive clarity.
  - **Time caps:** 20–25 minutes per match; in Blitz, if time is called, finish the current turn then compare totals.
  - **Record-keeping:** Track round wins (Bankout) or cumulative totals (Blitz) clearly; note Joker usage and counters.

---

## Quick reference

- **Numbers:** Loot += value.
- **Jack:** Alert on; 3rd this turn busts; if Latent > 0, auto-Queen or bust.
- **Ace:** Bust if Alert; else Latent += 1.
- **Queen:** -1 Jack; -1 Latent; clear Alert at 0; turn continues.
- **King:** Double-and-bank Loot; end turn.
- **Joker (ON):** R2 immediate on draw (swap/King/Queen); R3 to hand (offense: swap/King/Queen; defense: counter-swap).
- **Hand limit:** 2 (Q/K/X).
- **Busts:** Third Jack on table; Ace on Alert; latent-without-Queen; three latent Aces on table.
- **Ends:** Bankout: first to bank $1,000 wins round; first to 2 wins match. Blitz: highest total after 3 rounds; Sudden Death for ties.
