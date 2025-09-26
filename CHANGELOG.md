
# Changelog

All notable changes to the Bankout project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- **Three Latent Aces Bust Rule**: New bust condition where drawing a third Ace while having 2 latent Aces results in immediate bust

### Fixed
- Card movement logic bug that caused premature clearing of `drawnCards` in bust scenarios
  - Fixed latent Ace bust scenario
  - Fixed third Jack bust scenario
  - Fixed Ace-on-alert bust scenario
- Latent Ace auto-resolution to properly discard Jack and Ace when Queen is auto-played

### Todo (Planned Features)
- Comprehensive game flow test suite for regression testing
- Mobile-responsive UI enhancements
- Game statistics and analytics tracking
- Multi-language support
- Sound effects and audio enhancements

## [1.0.0] - 2025-09-24

### Added
- **Production-ready release** with comprehensive implementation verification
- **Blitz Mode**: Winner-takes-all pot system with cumulative scoring across 3 rounds
- **Enhanced Joker System**: Three-choice options (Swap/King/Queen) for both Round 2 and Round 3
- **Player Name System**: Customizable player names with localStorage persistence
- **Random First Player**: Fair 50/50 starting chance randomization
- **AI Opponent**: Human-level strategic decision making with risk assessment
- **Intellectual Property Protections**: Anti-debugging, obfuscation, and console protection
- **Complete documentation suite**: Rules, Analysis, Guide, Cheatsheet, and GDD

### Changed
- **Reduced Hand Limit**: From 3 cards to 2 cards for increased strategic complexity
- **AI Decision Algorithms**: Enhanced for tighter resource management with 2-card limit
- **Queen Value**: Dramatically increased strategic importance due to hand limit reduction
- **King Timing**: More strategic usage required for optimal finishing power
- **Joker Commitment**: Higher cost for holding Jokers with limited hand space
- **Discard Modal Text**: Updated to reflect 2-card limit

### Fixed
- All edge cases and timing windows verified and resolved
- Probability calculations aligned with Analysis.md specifications
- UI/UX implementation validated against design documentation
- Documentation files updated to reflect new hand limit (`Rules.md`, `Analysis.md`, `Guide.md`, `Cheatsheet.md`)

## [0.9.0] - 2025-09-23

### Added
- **AI Human-Level Strategy**: Risk-based decision making with deck tracking
- **EV-Driven Decision Tree**: Mathematical probability calculations from Analysis.md
- **Contextual Heuristics**: Adaptive AI behavior based on lead/trail status and game format
- **Variability System**: Human-like delays (0.5-2s) and bluffing (10-20%)
- **Deck State Tracking**: Real-time card counting for probability calculations
- **Risk Assessment**: Advanced bust probability calculations with Queen safety bonuses
- **Action Evaluation**: Comprehensive EV scoring for all possible actions


### Changed
- **Banking Philosophy**: Extremely conservative - only banks when absolutely necessary
- **King Usage**: Conservative deployment based on loot value thresholds ($300+ = 1.8x, $200+ = 1.2x, $100+ = 0.8x, $50+ = 0.5x, <$50 = 0.2x)
- **Queen Priority**: Hand management prioritizes keeping Queens; auto-discard avoids discarding Queens
- **Proactive Queen Playing**: High-priority play when Alert active or latent risk exists

## [0.8.0] - 2025-09-22

### Added
- **Core Game Implementation**: Single-file HTML/CSS/JS architecture
- **Card Types**: Numbers (2-10), Jacks (Alarm), Aces (Bust), Queens (Decoy), Kings (Insider)
- **Game Formats**: Bankout (best-of-3) and Blitz (cumulative scoring) modes
- **Basic AI Opponent**: Initial risk assessment implementation
- **Fisher-Yates Shuffle**: Fair deck randomization algorithm
- **CSS Variables**: Theming system for visual consistency
- **Animations**: Smooth card transitions and state changes

### Fixed
- Initial game logic implementation for win/bust conditions
- Deck exhaustion handling with proper reshuffling
- Turn state management and switching

## [0.1.0] - 2025-09-21

### Added
- **Project Foundation**: Initial repository setup and structure
- **Game Design Documentation**: Bankout_GDD.md with comprehensive game rules
- **Intellectual Property Framework**: LICENSE.md and contributor agreements
- **Basic HTML Structure**: Initial game interface layout
- **Core JavaScript Framework**: State management and game loop foundation

---
*This changelog follows the conventions from [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).*