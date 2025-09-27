# Bankout: A Game of Great Risk and Even Greater Reward
**Copyright © 2025 VWN - All Rights Reserved**
*PROPRIETARY SOFTWARE - NON-COMMERCIAL USE ONLY*

## ⚠️ INTELLECTUAL PROPERTY NOTICE
This game and all its components are protected by multiple layers of intellectual property rights:
- **Copyright**: Source code, rules, documentation, and visual assets, "Bankout" name, logos, and distinctive elements
<!-- - **Trademark**: "Bankout" name, logos, and distinctive elements  
- **Design Patent**: Unique card layouts and interface elements -->
- **Trade Secret**: AI algorithms and game balance formulas

**UNAUTHORIZED COMMERCIAL USE STRICTLY PROHIBITED**

## 🎯 Quick Start
1. Open [`index.html`](index.html) in any modern web browser
2. Choose game format (Bankout/Blitz) and Joker settings
3. Select game mode (Player vs Player or Player vs AI)
4. Start playing - no installation required!

## 🎮 Game Formats

### Bankout Mode (Default)
- **Best-of-three rounds** - First to win 2 rounds wins the match
- **Deck reshuffles** when exhausted during a round
- **First to bank $1,000** wins the round immediately

### Blitz Mode (Winner-Takes-All)
- **Cumulative scoring** across 3 rounds with pot accumulation
- **Winner collects entire pot** - highest total bank after Round 3 wins
- **Deck depletion ends rounds** (no reshuffling during rounds)
- **Tiebreaker**: Sudden Death with Numbers + Jacks only deck

## 🃏 Jokers Setting

### Jokers ON (Enhanced Strategy)
- **Round 1**: No Jokers
- **Round 2**: 1 Joker - resolves immediately with three-choice options:
  - Swap banks (if opponent has money)
  - King effect (double & bank)
  - Queen effect (mitigate risk)
- **Round 3**: 2 Jokers - go to hand with Counter Joker rules:
  - Offensive play: Choose Swap/King/Queen
  - Defensive play: Counter opponent's swap attempts

### Jokers OFF (Pure Probability)
- Removes all Jokers from deck
- Focuses gameplay on risk management and probability
- Classic push-your-luck experience

## 🎴 The Cards & Their Functions

### Number Cards (2-10)
- **Value**: Face value added to temporary loot
- **Example**: 7 → $7 added to loot

### Jacks (J) - "Alarm" Cards
- **Alert**: Puts police on high alert
- **Third Jack**: Busts immediately (lose all loot)
- **Visibility**: Public to all players

### Aces (A) - "Bust" Cards  
- **Alert + Ace**: Bust immediately
- **No Alert**: Becomes latent (armed danger)
- **Latent Trigger**: Activates when Jack would establish alert

### Queens (Q) - "Decoy" Cards
- **Hand Management**: Save to hand (max 2 cards)
- **Effect**: Remove one Jack instance, clear alert if possible
- **Auto-Play**: Mandatory when latent Ace triggered
- **Visibility**: Hidden until played

### Kings (K) - "Insider" Cards
- **Hand Management**: Save to hand (max 2 cards)
- **Effect**: Double current loot and bank immediately
- **Turn Ends**: After playing
- **Visibility**: Hidden until played

### Jokers (X) - "Saboteur" Cards
- **Round-Specific**: Different rules per round
- **Strategic Flexibility**: Three-choice system
- **Counter Mechanics**: Defensive play options

## 🔄 Gameplay Flow

### Turn Structure
1. **Start**: Loot = $0, Alert = off, Jacks = 0, Latent = 0
2. **Actions**:
   - Draw card (resolve immediately)
   - Play from hand (Queen/King/Joker)
   - Bank loot (secure points)
   - End turn voluntarily (lose loot)
3. **Turn Ends**: On bank, bust, King play, or certain Joker effects

### Card Resolution Priority
1. Reveal card
2. Immediate bust checks (Ace on alert, third Jack)
3. Mandatory latent trigger (auto-Queen or bust)
4. Apply card's normal effect
5. Enforce hand limit (max 2 cards)
6. Check turn/round end conditions

## 🏆 Victory Conditions

### Bankout Mode
- **Round Win**: First to bank $1,000
- **Match Win**: First to win 2 rounds
- **Tie Resolution**: Higher bank total wins; if equal, first to cross wins

### Blitz Mode  
- **Pot Accumulation**: Every banking action adds to total pot
- **Final Victory**: Highest cumulative bank after Round 3 collects entire pot
- **Sudden Death**: Numbers + Jacks only, first to $1,000 wins

## ⚖️ License & Legal

### Proprietary License
This software is licensed under a **PROPRIETARY NON-COMMERCIAL LICENSE**:
- **Personal Use**: Allowed for non-commercial purposes
- **Commercial Use**: STRICTLY PROHIBITED
- **Modification**: Allowed for personal use only
- **Redistribution**: PROHIBITED

### Intellectual Property Protection
<!-- - **Copyright Registration**: Filed with US Copyright Office
- **Trademark Registration**: "Bankout"™ registered
- **Design Patents**: Card layouts and UI elements patented -->
- **Trade Secrets**: AI algorithms and balance formulas protected

### Enforcement Mechanisms
- **Digital Watermarking**: Embedded tracking in all files
- **DMCA Takedowns**: Automated monitoring and enforcement
- **Legal Action**: Pursued against unauthorized commercial use

## 🤝 Contributing

### Contributor Requirements
All contributions require:
1. **Signed CLA**: [`CONTRIBUTORS.md`](CONTRIBUTORS.md)
2. **DCO**: Developer Certificate of Origin
3. **IP Assignment**: All rights assigned to VWN

### Contribution Guidelines
- Non-commercial improvements welcome
- All contributions become property of VWN
- No expectation of compensation
- Follow existing code style and patterns

## 🛡️ Anti-Piracy Measures

### Technical Protections
- **Digital Watermarks**: Unique tracking IDs in code
- **Anti-Debugging**: Memory usage monitoring
- **Obfuscation**: AI logic protection
- **Console Logging**: IP protection status display

<!-- ### Legal Protections
 - **Multi-Jurisdictional**: US, EU, China filings via Madrid/Hague systems
- **Customs Recording**: Border enforcement against physical copies
- **App Store Registration**: Name locking on digital platforms -->

## 📋 Files Overview

- [`index.html`](index.html) - Main game file with HTML/CSS/JS
- [`Rules.md`](/doc/Rules.md) - Complete rules and regulations
- [`LICENSE.md`](LICENSE.md) - Proprietary license terms
- [`CONTRIBUTORST.md`](CONTRIBUTORS.md) - Contributor agreement
- [`Analysis.md`](/doc/Analysis.md) - Game balance and probability analysis
- [`Bankout_GDD.md`](Bankout_GDD.md) - Game design documentation

## 📞 Contact & Support

**For Inquiries**: info@bankoutgame.com
**For Licensing Inquiries**: legal@bankoutgame.com  
**Game Issues**: [Open GitHub issue](https://github.com/bellodox/Bankout/issues)  
**Commercial Licensing**: Contact for partnership opportunities

---
**WARNING: UNAUTHORIZED COMMERCIAL USE WILL RESULT IN LEGAL ACTION**
*This game represents significant investment in design, development, and intellectual property protection. Respect the creators' rights.*