# Bankout Debug Summary

## Problem Identified
The issue was in the card discard logic when playing a Queen to clear an alert in the Bankout card game. When a player played a Queen to clear an alert caused by a Jack, the Jack was not immediately discarded. Instead, it remained in the `drawnCards` array until the turn ended, when all cards in `drawnCards` were moved to the discard pile. This caused confusion for players who expected the Jack to be immediately discarded when the Queen cleared the alert.

## Root Cause Analysis
In the original implementation of the `playQueen` function, when a Queen was played to clear an alert:
1. The alert status was correctly cleared
2. The jacks counter was correctly reduced
3. However, the Jack card that caused the alert remained in the player's `drawnCards` array
4. This Jack would only be moved to the discard pile at the end of the turn along with other drawn cards

This behavior was inconsistent with player expectations and the game's design principle that cards should be discarded immediately when their effects are resolved.

## Solution Implemented
Modified the `playQueen` function in `index.html` to immediately find and discard the Jack that caused the alert when the Queen clears it:

1. After clearing the alert (when `jacksThisTurn` reaches 0), the function now searches through the player's `drawnCards` array for the most recent Jack.
2. When found, it removes the Jack from `drawnCards` and adds it directly to the discard pile.
3. A log message confirms that the Jack was discarded.

### Code Changes
```javascript
// In the playQueen function, added this block when jacksThisTurn reaches 0:
// Find and immediately discard the Jack that caused the alert
// Search from the end of drawnCards since it was the most recent card
for(let i = P.drawnCards.length - 1; i >= 0; i--) {
  if(P.drawnCards[i].t === 'J') {
    const jack = P.drawnCards.splice(i, 1)[0];
    state.discard.push(jack);
    log(`Queen played — alert cleared. Jack discarded.`);
    break;
  }
}
```

## Testing Performed
Created comprehensive test suites to verify the fix and ensure no regressions:

1. **Basic Functionality Test** - Verified the core fix works correctly
2. **Original Scenario Test** - Reproduced and verified the exact scenario from the bug report
3. **Comprehensive Test Suite** - Tested all card interactions and edge cases:
   - Numeric card interactions
   - Queen scenarios with various card combinations
   - Ace-specific cases (latent and alert)
   - Jack-heavy scenarios
   - Complex card interactions
   - Edge cases and error conditions

All tests passed, confirming:
- The Jack is immediately discarded when a Queen clears an alert
- Only the relevant cards remain in `drawnCards` for end-of-turn discard
- The discard pile correctly contains both the Queen and the Jack
- All other game state variables are properly updated
- No regressions in other game functionality

## Verification Results
The fix successfully resolves the original issue:

**Before Fix:**
- Player plays Queen to clear alert
- Jack remains in `drawnCards` array
- At turn end, Jack is moved to discard pile with other cards
- Player sees only 2 cards discarded at turn end (confusing)

**After Fix:**
- Player plays Queen to clear alert
- Jack is immediately removed from `drawnCards` and added to discard pile
- At turn end, only remaining cards are moved to discard pile
- Player sees 3 cards discarded in total (Jack immediately, Queen when played, numbers at turn end)
- Log message clearly shows "Queen played — alert cleared. Jack discarded."

## Impact
This fix improves the user experience by making the game behavior consistent with player expectations:
- Cards are discarded immediately when their effects are resolved
- Log messages accurately reflect what's happening in the game
- Players can better understand the game state at any given time
- No impact on game balance or other functionality

The fix is minimal, targeted, and maintains full backward compatibility while improving the clarity of game mechanics.