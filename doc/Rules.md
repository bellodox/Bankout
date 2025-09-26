# 🏦 Bankout — Complete rules set
**Copyright © 2025 VWN - All Rights Reserved**
*PROPRIETARY GAME RULES - UNAUTHORIZED REPRODUCTION PROHIBITED*

**Implementation Status: ✅ FULLY VERIFIED AND COMPLIANT**

> All rules described in this document have been comprehensively verified against the actual game implementation. The game is production-ready with no known bugs.

---

## 1) Game formats and setup

### Formats

- **Bankout (default):** Best-of-three rounds. Deck reshuffles when exhausted. First to win 2 rounds wins the match.
- **Blitz:** Cumulative scoring across 3 rounds with winner-takes-all pot system. When the deck runs out, the round ends. Highest total bank after Round 3 wins and collects the entire accumulated pot. If tied → Sudden Death.

### Jokers setting

- **Jokers ON:** Use the Joker distribution and rules below.  
- **Jokers OFF:** Remove all Jokers from the deck; ignore Joker rules.

### First player and flow

- **Round 1 start:** Random.  
- **Subsequent rounds:** Alternate the starting player.  
- **Shuffle and cut:** Shuffle well; offer a cut. Rebuild the deck between rounds with the correct Joker count (if Jokers ON).

---

## 2) Components and state

### Deck and distribution

- **Base deck:** Standard 52-card deck (four of each 2–10, J, Q, K, A).
- **Jokers (if ON):**
  - **Round 1:** 0 Jokers.
  - **Round 2:** 1 Joker (resolves immediately when drawn with three-choice options: Swap banks if opponent has money, King effect, or Queen effect).
  - **Round 3:** 2 Jokers (go to hand; when played, choose between Swap banks if opponent has money, King effect, or Queen effect; Counter Joker rules apply).
- **Sudden Death deck (for Blitz ties):** Numbers (2–10) and Jacks only.

### Per-player state

- **Bank:** Secured points this round.  
- **Loot:** Unbanked points earned this turn.  
- **Hand:** Holds only Queens, Kings, and (in Round 3) Jokers.  
- **Alert:** Active when Jacks are present on the table.
- **Jacks this turn:** Count of Jacks drawn this turn.
- **Latent Aces:** Aces drawn off-alert that remain on the table as visual cues, "arming" a pending danger.

### Hand limit

- **Limit:** Maximum of 2 cards in hand (Q, K, X only).
- **Enforcement:** If adding a card would push you above 2, immediately choose which card(s) to discard back to 2. If no choice is made (e.g., timer/invalid), discard by pre-agreed default (commally the oldest).

### Discard and reshuffle

- **Discard:** All resolved cards go to a common discard pile. Jacks and Aces remain on the table as visual cues until resolved by a Queen or bust.
- **Reshuffle:** When the deck empties, shuffle the discard to form a new deck and continue.

---

## 3) Turn structure and timing

### Start of turn

- **State:** Loot = $0; not on alert; Jacks this turn = 0; Latent = 0.  
- **Persist:** Bank and Hand carry over between turns.

### Actions during your turn

- **Draw:** Reveal top card and resolve it immediately.  
- **Play from hand:** Queens, Kings, and (Round 3) Jokers can be played at instant speed on your turn unless otherwise noted.  
- **Bank:** Move all Loot to Bank; ends your turn.  
- **Voluntarily end turn:** End with no banking; you lose Loot.

### Turn ends when

- **You bank**, or you **bust**, or you **play a King**, or you **resolve a Joker that ends the turn** (swap or wild-King), or you **voluntarily end**.
- **On end/bust:** Reset per-turn states (Loot, Jacks this turn) and pass play. Jacks and Aces remain on the table as visual cues until resolved by a Queen or bust.

---

## 4) Card effects and precise timing

### Numbers (2–10)

- **Effect:** Add face value to Loot.  
- **Afterward:** Discard the card.

### Jack (J)

- **When drawn:**
  - **Latent trigger window (mandatory):** If Latent > 0:
    - **If you have a Queen in hand:** Auto-play one Queen (see Queen below).  
    - **If you do not have a Queen:** Bust immediately.
  - **Then apply Jack:** Jacks this turn += 1; set Alert = true.
  - **Third Jack in the same turn:** Bust immediately.

> Priority note: The latent trigger (and any auto-Queen) resolves before evaluating the “third Jack” bust, so the Queen’s -1 to Jacks this turn can prevent the third-Jack bust.

### Ace (A)

- **If Alert = true when drawn:** Bust immediately.  
- **If Alert = false when drawn:** Latent += 1 (the Ace is “armed” and threatens only when a Jack would put you on alert).  
- **Latent scope:** Clears on bust or when your turn ends.

### Queen (Q)

- **Drawn:** Added to your hand (subject to the hand limit).  
- **Played (manually or auto via latent trigger):**
  - **Reduce Jacks this turn by 1** (min 0).  
  - **If Jacks this turn becomes 0:** Set Alert = false.  
  - **Reduce Latent by 1** (min 0).  
  - **Remove the Queen** from hand.  
  - **Does not end your turn.**

> A Queen can be played immediately after revealing a Jack (before consequences finalize) to remove one Jack instance, potentially exiting alert and disarming a latent Ace.

### King (K)

- **Drawn:** Added to your hand (subject to the hand limit).  
- **Played:** Double your current Loot and bank it (Bank += 2×Temp), then reset per-turn state (Loot = 0; Alert = false; Jacks this turn = 0; Latent = 0) and end your turn.

### Joker (X)

- **Round 1:** No Jokers in the deck.
- **Round 2 (Immediate Joker):** If drawn, it resolves immediately (does not go to hand):
  - **Three-choice options:** Choose immediately:
    - **Swap banks** (if opponent's Bank > 0): Swap Banks; reset your per-turn state; end your turn.
    - **King effect:** Double current Loot and bank it; reset per-turn state; end your turn.
    - **Queen effect:** Apply Queen effect (mitigate; turn continues).
  - **Cannot be held or countered.**
- **Round 3 (Counter Joker rules):**
  - **Drawn:** Goes to your hand (subject to the hand limit).
  - **Offense (your turn):** You may play a Joker to choose:
    - **Swap banks** (if opponent's Bank > 0): Swap Banks; reset per-turn state; end your turn.
    - **King effect:** Double current Loot and bank it; reset per-turn state; end your turn.
    - **Queen effect:** Apply Queen effect (mitigate; turn continues).
  - **Defense (opponent's turn):** If opponent plays a Joker to swap with you, you may discard a Joker from your hand to **counter**:
    - Both Jokers are discarded.
    - No swap occurs.
    - Opponent's turn ends.
    - A defensive Joker never triggers other effects.

---

## 5) Banking, busts, and end conditions

### Banking

- **Action:** Add Loot to Bank; reset per-turn state; end your turn.  
- **Use:** Any time during your turn before busting.

### Bust conditions

- **Third Jack** in the same turn.  
- **Any Ace drawn while on Alert.**  
- **Latent Ace triggered by a Jack when you have no Queen to auto-play.**

> Bust effect: Lose Loot (Bank unchanged), reset per-turn state, end your turn.

### Visibility

- **Public on draw:** Numbers, Jacks, Aces, Jokers (and all visible totals).
- **Persistent visual cues:** Jacks and Aces remain on the table as visual cues until resolved.
- **Hidden until played:** Queens, Kings, and (Round 3) Jokers in hand.

---

## 6) Round and match structure

### Bankout (best of three)

- **Round goal:** First to bank $1,000 wins the round immediately.  
- **Match goal:** First to 2 round wins.  
- **If neither reaches $1,000 in a sequence:** Reshuffle and continue the round until someone does.  
- **Tie edge cases:** If a rule effect would let both cross at once, the higher Bank total wins; if equal, the player who first crossed wins.

### Blitz (3-round cumulative with winner-takes-all)

- **Rounds:** Three rounds; banks accumulate into a total pot each round.
- **Match goal:** Highest cumulative total after Round 3 wins and collects the entire accumulated pot.
- **Pot accumulation:** Every time a player banks money (through Bank action or King play), that amount is immediately added to the total pot. Players keep their individual banks for scoring purposes.
- **Winner-takes-all:** The player with the highest cumulative bank after Round 3 receives the entire total pot as their final winnings.
- **Tie after Round 3:** Play Sudden Death:
  - **Deck:** Numbers and Jacks only.
  - **Bust:** Third Jack in a single turn.
  - **Win:** First to bank $1,000 wins and collects the total pot (if accumulated from Blitz mode). If neither reaches $1,000, higher Bank in Sudden Death wins.

---

## 7) Resolution order and timing windows

When you draw a card, resolve in this order:

1. **Reveal** the card.  
2. **Immediate bust checks:**  
   - Ace while Alert = true → bust now.  
   - Third Jack this turn → bust now (but see step 3).  
3. **Mandatory latent trigger on Jack (if Latent > 0):**  
   - Auto-play a Queen if available; otherwise bust immediately.  
   - Auto-Queen’s -1 to Jacks this turn can prevent a third-Jack bust.  
4. **Apply the card’s normal effect:**  
   - Numbers add to Loot; Queens/Kings to hand; Jokers resolve per round (R2 immediate; R3 to hand).  
5. **Enforce hand limit:** If hand > 2, choose discards to return to 2.
6. **Check turn/round ends:**  
   - King and Joker swap/wild-King end turn; banking ends turn.  
   - Check Bankout/Blitz thresholds as applicable.

Playing from hand on your turn:

- **Queen:** Apply reductions; turn continues; enforce hand limit if needed.  
- **King:** Double-and-bank; reset per-turn state; end turn.  
- **Joker (Round 3):** Swap (opponent > 0), King effect (double-and-bank), or Queen effect (mitigate); swap and King end turn; Queen continues turn.

---

## 8) Rulings, clarifications, and strategy-critical nuances

- **Queen removes one Jack instance:** If multiple Jacks were active, you may remain on Alert; removing the last Jack clears Alert.  
- **Latent Ace only threatens at the moment a Jack would (re)establish Alert:** If you clear Alert at that instant (via Queen or wild-Queen), you avoid the latent bust.  
- **Auto-Queen is mandatory on latent trigger:** If you have a Queen, it auto-plays; if not, you bust.  
- **Defensive Joker is cancel-only:** It cancels a swap and ends the opponent’s turn; it never produces wild effects.  
- **King ends turn immediately:** No further draws or plays after doubling-and-banking.  
- **Joker choice is immediate:** Must pick Swap (if available), King-effect, or Queen-effect; cannot be treated as a Number.
- **Priority after revealing a Jack:** Active player may respond with a Queen before Jack’s consequences finalize.

---

## 9) Optional variants (supported and play-tested)

- **Light Jokers (Blitz):** Use only 1 Joker across all 3 rounds.  
- **Shield Token (Blitz):** Each player gets one per match to cancel a Joker swap targeting them (alternative to Counter Joker).  
- **Exact Bank Bonus:** +$25 for banking exactly $1,000 (skip in Sudden Death).  
- **Seeding edge (Bankout):** Round winner chooses to go first or second in the next round.  
- **Short-match Blitz:** Two rounds total; if tied, play Sudden Death.

---

## 10) Quick-reference summary

- **Numbers (2–10):** Loot += value.  
- **Jack:** Alert on; 3rd this turn busts; latent auto-Queen or bust.  
- **Ace:** Bust if Alert; else Latent += 1.  
- **Queen:** -1 Jack; -1 Latent; clear Alert at 0; turn continues.  
- **King:** Double-and-bank Loot; end turn.  
- **Joker (ON):**  
  - R1: none.  
  - R2: on draw → swap (opp > 0), King effect, or Queen effect; swap and King end turn; Queen continues turn.
  - R3: to hand; offense = swap (opp > 0), King effect, or Queen effect; defense = counter (cancel swap; both Jokers discarded; opponent's turn ends).
- **Hand limit:** Max 2 (Q/K/X).
- **Bust:** Third Jack; Ace on Alert; latent-without-Queen.  
- **Bankout win:** First to $1,000 wins the round; first to 2 rounds wins match.  
- **Blitz win:** Highest cumulative after 3 rounds; if tied, Sudden Death (Numbers + Jacks).

---
