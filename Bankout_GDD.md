# Game Design Document: Bankout

## 1. Game Concept

**Elevator Pitch:** "Bankout" is a minimalist, high-stakes, push-your-luck card game where players take on the role of elite thieves in a stylish bank heist. The goal is to be the first to accumulate $1,000 by drawing cards from a shared deck. Each turn is a gamble: draw cards to increase your loot, but pull too many "Alarm" cards or trigger a "Bust" condition, and you'll lose everything from that turn. It's a simple, elegant game of nerve and strategy, offering two distinct play formats and dynamic player interactions, all contained within a single, beautifully designed HTML file.

## 2. Core Gameplay Loop

A player's turn consists of a simple choice, repeated until they decide to stop or are forced to.

**Step-by-Step Turn:**

1.  **Start of Turn:** The player's temporary loot pile is empty, Alert is off, Jacks this turn is 0, and Latent Aces are 0. Hand and Bank persist between turns.
2.  **Player's Choice:** The player must choose one of two actions:
    *   **Draw Card:** The player draws the top card from the deck. The card is revealed and its effect is resolved immediately according to a strict priority order (see Section 3.7).
        *   **Number Cards (2-10):** Their face value is added to the temporary loot pile for the current turn.
        *   **Jacks (J):** Put the player on "Alert" and increment "Jacks this turn." If a third Jack is drawn, the player busts. If Latent Aces are present, a Queen is auto-played to mitigate, or the player busts.
        *   **Aces (A):** If drawn while on "Alert," the player busts. Otherwise, it becomes a "Latent Ace," threatening a bust if a Jack is drawn later.
        *   **Queens (Q) & Kings (K):** Added to the player's hand (subject to a 3-card hand limit).
        *   **Jokers (X):** (If Jokers ON)
            *   **Round 1:** No Jokers in the deck.
            *   **Round 2:** Resolves immediately on draw with three-choice options: Swap banks (if opponent has >$0), King effect (double & bank), or Queen effect (mitigate risk).
            *   **Round 3:** Added to the player's hand; when played, choose between Swap banks (if opponent has >$0), King effect (double & bank), or Queen effect (mitigate risk); Counter Joker rules apply.
    *   **Play from Hand:** Queens, Kings, and (Round 3) Jokers can be played at instant speed on the player's turn.
        *   **Queen:** Reduces "Jacks this turn" by 1 (min 0), potentially clearing "Alert" and disarming a "Latent Ace." Does not end the turn.
        *   **King:** Doubles current temporary "Loot" and banks it, then ends the turn.
        *   **Joker (Round 3):** Can be played offensively to swap banks (if opponent has >$0) or go wild (if opponent has $0). Offensive play ends the turn (unless wild-Queen is chosen).
    *   **Bank Loot:** The player ends their turn. All money in their temporary loot pile is added to their permanent, total score. The temporary loot pile, Alert, Jacks this turn, and Latent Aces are cleared. Play then passes to the next player.
    *   **Voluntarily End Turn:** The player ends their turn without banking. All temporary loot is lost.

3.  **Bust Condition:** A player "Busts" if:
    *   They draw a **third Jack** in the same turn.
    *   They draw an **Ace while on Alert**.
    *   A **Latent Ace is triggered by a Jack** when they have no Queen in hand to auto-play.
    *   They draw a **third Ace while already having 2 latent Aces** (off Alert).
    *   **Bust Effect:** The player's turn ends immediately. All money in their temporary loot pile for that turn is lost. Per-turn states (Loot, Alert, Jacks this turn, Latent) are reset. Play passes to the next player.

4.  **Hand Limit:** Players can hold a maximum of 2 cards (Queens, Kings, Round 3 Jokers). If drawing a card would exceed this limit, the player must immediately choose a card to discard.

5.  **Winning the Game:**
    *   **Bankout Mode:** The first player to reach or exceed a total banked score of $1,000 wins the round immediately. The first player to win 2 rounds wins the match.
    *   **Blitz Mode:** Cumulative scoring across 3 rounds with winner-takes-all pot system. Every banking action adds to a total pot that accumulates across all rounds. The player with the highest total banked score after Round 3 wins and collects the entire accumulated pot. If tied, "Sudden Death" is played.
        *   **Sudden Death:** Deck consists of only Numbers and Jacks. First player to bank $1,000 wins and collects the total pot (if accumulated from Blitz mode). If neither reaches $1,000, the higher bank in Sudden Death wins.

## 3. Card Deck Composition and Mechanics

The game uses a standard 52-card deck. Jokers are optional and their distribution varies by round.

### 3.1. Number Cards (2-10)
*   **Effect:** Add face value to Loot.
*   **Afterward:** Discard the card.

### 3.2. Jack (J) - "Alarm" / "Cop" Card
*   **When drawn:**
    *   Increments "Jacks this turn" and sets "Alert" to true.
    *   **Latent Trigger Window (Mandatory):** If "Latent Aces" > 0:
        *   If a Queen is in hand, one Queen is auto-played (reduces "Jacks this turn" by 1, potentially clearing "Alert" and "Latent Aces").
        *   Otherwise, the player busts immediately.
    *   **Third Jack:** If "Jacks this turn" reaches 3, the player busts immediately.
*   **Visibility:** Public on draw.

### 3.3. Ace (A) - "Bust" Card
*   **If Alert = true when drawn:** Bust immediately.
*   **If Alert = false when drawn:** Increments "Latent Aces" by 1 (the Ace is "armed" and threatens only when a Jack would put the player on alert).
*   **Latent Scope:** Clears on bust or when the player's turn ends.
*   **Visibility:** Public on draw.

### 3.4. Queen (Q) - "Decoy" Card
*   **Drawn:** Added to hand (subject to hand limit).
*   **Played (manually or auto via latent trigger):**
    *   Reduces "Jacks this turn" by 1 (min 0).
    *   If "Jacks this turn" becomes 0, "Alert" is set to false.
    *   Reduces "Latent Aces" by 1 (min 0).
    *   Removed from hand and discarded.
    *   Does not end the turn.
*   **Visibility:** Hidden until played.

### 3.5. King (K) - "Insider" Card
*   **Drawn:** Added to hand (subject to hand limit).
*   **Played:** Doubles current "Loot" and banks it (Bank += 2 × Loot), then resets per-turn state (Loot = 0; Alert = false; Jacks this turn = 0; Latent = 0) and ends the turn.
*   **Visibility:** Hidden until played.

### 3.6. Joker (X) - "Saboteur" Card
*   **Round 1:** No Jokers in the deck.
*   **Round 2 (Immediate Joker):** If drawn, it resolves immediately (does not go to hand):
    *   **Three-choice options:** Choose immediately:
        *   **Swap banks** (if opponent's Bank > 0): Swap Banks; reset per-turn state; end turn.
        *   **King effect:** Double current Loot and bank it; reset per-turn state; end turn.
        *   **Queen effect:** Apply Queen effect (mitigate; turn continues).
    *   Cannot be held or countered.
*   **Round 3 (Counter Joker Rules):**
    *   **Drawn:** Goes to hand (subject to hand limit).
    *   **Offense (your turn):** You may play a Joker to choose:
        *   **Swap banks** (if opponent's Bank > 0): Swap Banks; reset per-turn state; end your turn.
        *   **King effect:** Double current Loot and bank it; reset per-turn state; end your turn.
        *   **Queen effect:** Apply Queen effect (mitigate; turn continues).
    *   **Defense (opponent's turn):** If opponent plays a Joker to swap with you, you may discard a Joker from your hand to **counter**:
        *   Both Jokers are discarded.
        *   No swap occurs.
        *   Opponent's turn ends.
        *   A defensive Joker never triggers other effects.
*   **Visibility:** Public on draw (R2), Hidden until played (R3).

### 3.7. Resolution Order and Timing Windows (On Card Draw)

When a card is drawn, resolve in this order:

1.  **Reveal** the card.
2.  **Immediate bust checks:**
    *   Ace while Alert = true → bust now.
    *   Third Jack this turn → bust now (but see step 3).
3.  **Mandatory latent trigger on Jack (if Latent > 0):**
    *   Auto-play a Queen if available; otherwise bust immediately.
    *   Auto-Queen’s -1 to Jacks this turn can prevent a third-Jack bust.
4.  **Apply the card’s normal effect:**
    *   Numbers add to Loot; Queens/Kings to hand; Jokers resolve per round (R2 immediate; R3 to hand).
5.  **Enforce hand limit:** If hand > 2, choose discards to return to 2.
6.  **Check turn/round ends:**
    *   King and Joker swap/wild-King end turn; banking ends turn.
    *   Check Bankout/Blitz thresholds as applicable.

## 4. Game Formats and Setup

### 4.1. Bankout (Default)
*   **Objective:** Best-of-three rounds. Deck reshuffles when exhausted. First to win 2 rounds wins the match.
*   **Round Goal:** First to bank $1,000 wins the round immediately.
*   **First Player:** Random in Round 1; alternate thereafter.
*   **Deck Build:** Jokers are distributed per round (0 in R1, 1 in R2, 2 in R3 if Jokers ON).

### 4.2. Blitz
*   **Objective:** Cumulative scoring across 3 rounds with winner-takes-all pot system. When the deck runs out, the round ends. Highest total bank after Round 3 wins and collects the entire accumulated pot.
*   **Pot Accumulation:** Every banking action (Bank action or King play) adds to a total pot that accumulates across all 3 rounds.
*   **Winner-Takes-All:** The player with the highest cumulative bank after Round 3 receives the entire accumulated pot as their final winnings.
*   **Tie after Round 3:** Play Sudden Death.
*   **Sudden Death Deck:** Numbers (2–10) and Jacks only. First to bank $1,000 wins and collects the total pot (if accumulated from Blitz mode). If neither reaches $1,000, higher Bank in Sudden Death wins.
*   **First Player:** Random in Round 1; alternate thereafter.
*   **Deck Build:** Jokers are distributed per round (0 in R1, 1 in R2, 2 in R3 if Jokers ON).

### 4.3. Jokers Setting
*   **Jokers ON:** Use the Joker distribution and rules as described.
*   **Jokers OFF:** Remove all Jokers from the deck; ignore Joker rules.

## 5. Strategic Depth and Design Intent

The design aims to layer simple rules with deep timing decisions, creating a game that rewards both probability discipline and psychological play.

*   **Core Tension:** Convert volatile, compounding Loot into secure Bank while navigating Alert/Latent traps and swingy Jokers.
*   **Skill Expression:** Involves micro-decisions (priority windows, precise thresholds, hand limit management) and macro-decisions (tempo control, target setting, opponent denial).
*   **Risk Math:** Players can use probability estimates for bust risks and thresholds to inform their draw/bank/King decisions.
*   **Joker Metagames:** Jokers introduce significant variance and counterplay, especially in Round 3 with the Counter Joker mechanic, fostering bluffing and timing wars.
*   **Latent Ace and Auto-Queen:** This interaction creates a crucial defensive layer, rewarding careful hand management.
*   **Visibility:** The distinction between public and hidden information enables strategic bluffing and uncertainty.

## 6. Visual & UI/UX Design

The aesthetic is minimalist, elegant, and evocative of a sophisticated, high-stakes heist. The design philosophy is "less is more," focusing on clarity, atmosphere, and satisfying interactions.

*   **Layout:** Single-screen interface with clear areas for deck, discard, player states (Bank, Loot, Hand, Alerts), and action buttons.
*   **Visuals:** High-contrast color palette (charcoal, off-white, gold, subtle red for danger). Clean sans-serif typography. Minimalist card designs with strong icons.
*   **Animations & Feedback:** Smooth card flips, score ticking, subtle risk indicators (e.g., `box-shadow` glow), clear button states, and minimalist sound effects for key actions (draw, bank, bust, special card).

## 7. Single-File Implementation

The entire game is encapsulated within a single `index.html` file, utilizing HTML for structure, CSS for styling, and JavaScript for game logic and DOM manipulation.

*   **HTML (`<body>`):** Contains main game board, deck, discard, player areas, UI panel with pot display (Blitz mode), and modals for settings, Joker choices, counter Joker decisions, hand limit discards, player name inputs, and game over.
*   **CSS (`<style>` tag in `<head>`):** Uses CSS Grid/Flexbox for layout, CSS variables for theming, and animations/transitions for visual feedback. Includes responsive design and dynamic UI elements that show/hide based on game mode.
*   **JavaScript (`<script>` tag at the end of `<body>`):**
    *   **Game State Management:** A `state` object holds all game data (deck, discard, player states, round, mode, totalPot, player names, etc.).
    *   **Core Functions:** `buildStandardDeck()`, `jokerCountForRound()`, `shuffle()`, `buildDeck()`, `newMatch()`, `startGame()`, `drawCard()`, `playQueen()`, `playKing()`, `playJoker()`, `bank()`, `switchTurn()`, `maybeEndRound()`, `endRound_Bankout()`, `endRound_Blitz()`, `startSuddenDeath()`, `showDiscardChoiceModal()`, `resolveDiscard()`.
    *   **Enhanced Functions:** `trackDeckState()`, `calculateBustRisk()`, `evaluateActionEV()`, `aiDecideAction()`, `executeAIDecision()` for AI logic.
    *   **Player Management:** `getPlayerName()`, `updatePlayerNames()`, `savePlayerNames()` for name system.
    *   **DOM Manipulation:** `render()` updates all visual elements including pot display. `log()` for game events with player names.
    *   **Event Listeners:** Wired to UI elements for player actions, modal interactions, and name input handling.

## 8. Implementation Status

*   **✅ AI Opponent:** Fully implemented with risk-based decision making, probability calculations, and human-like variability
*   **✅ Enhanced Joker System:** Three-choice options implemented for both Round 2 and Round 3 Jokers
*   **✅ Player Name System:** localStorage persistence with dynamic name inputs and fallback defaults
*   **✅ Random First Player:** Fair 50/50 randomization with proper turn alternation
*   **✅ Blitz Winner-Takes-All:** Pot accumulation system fully implemented with UI display
*   **✅ All Core Features:** Comprehensive implementation of all game mechanics and rules

## 9. Optional Variants (Future Consideration)

*   Light Jokers (Blitz): Use only 1 Joker across all 3 rounds
*   Shield Token (Blitz): Each player gets one per match to cancel a Joker swap
*   Exact Bank Bonus: +$25 for banking exactly $1,000
*   Seeding Edge (Bankout): Round winner chooses first/second next round
*   Short-match Blitz: Two rounds total; if tied, Sudden Death
