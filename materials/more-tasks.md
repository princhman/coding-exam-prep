**General Instructions for these Tasks (as per original document):**
*   These questions require you to load the Skeleton Program and to make programming changes to it.
*   Note that any alternative or additional code changes that are deemed appropriate to make must also be evidenced, ensuring that it is clear where in the Skeleton Program those changes have been made.
*   Students are recommended to start with a clean copy of the pre-release code before attempting each of the questions in this resource. This will prevent modifications made for one question having an unintended impact on a different question.

---

**Task 20: Advanced Solver with User-Defined Constraints**

**Marks: 10**

This question extends the auto-solver functionality (similar to Task 19's `GenerateEvaluations`) by allowing the user to specify constraints on the solutions generated.

**Introduce new functionality:**
When the user opts to receive suggestions, they should first be asked if they want to apply constraints.
If yes, they should be able to:
1.  Specify a *maximum number of operands* to be used in the suggested expressions (e.g., "only show expressions using 2 or 3 numbers").
2.  Specify an *operator preference* (e.g., "prioritise expressions using multiplication" or "avoid expressions using division"). If prioritising, such expressions should appear first. If avoiding, they shouldn't be shown.

The program should then generate and display expressions that meet these criteria and evaluate to a target.

**What you need to do**

**Task 20.1**
Modify the `PlayGame` method (or create a new helper method called by `PlayGame`) to:
*   Prompt the user if they want to apply constraints to suggestions.
*   If yes, prompt for the maximum number of operands (e.g., an integer).
*   If yes, prompt for operator preference (e.g., input like "+", "-", "*", "/", "none", "avoid +", "prioritise *").

Modify the `GenerateEvaluations` method (you might want to rename it or create a new version like `GenerateConstrainedEvaluations`) to:
*   Accept these new constraints as parameters.
*   Filter the generated expressions based on the maximum number of operands.
*   Filter or sort the generated expressions based on the operator preference.
    *   "Prioritise" means matching expressions are shown first.
    *   "Avoid" means matching expressions are not shown.
    *   "None" means no operator preference.

Ensure the method still only shows expressions that evaluate to a valid target.

**Task 20.2**
Test that the changes you have made work:
*   Run the Skeleton Program.
*   Press `enter` to start a standard random game.
*   Opt to receive suggestions and apply constraints.
*   **Test 1:** Specify a maximum of 2 operands. Show the program displaying suggestions that only use two numbers from `NumbersAllowed`.
*   **Test 2:** Start a new game or continue. Opt for suggestions. Specify no operand limit, but "prioritise *" (multiplication). Show that expressions with multiplication appear, ideally at the top if any exist that hit a target.
*   **Test 3:** Start a new game or continue. Opt for suggestions. Specify no operand limit, but "avoid /" (division). Show that no suggestions containing division are displayed, even if they could hit a target.

**Evidence that you need to provide:**
*   Your PROGRAM SOURCE CODE showing the amended `PlayGame` and the new/amended `GenerateConstrainedEvaluations` (or similarly named) method, and any other methods you have modified/created. [8 marks]
*   SCREEN CAPTURE(S) showing the required tests. [2 marks]

---

**Task 21: Persistent High Score Table**

**Marks: 9**

This question extends the Skeleton Program by adding a persistent high score table that is saved to and loaded from a file.

**Introduce new functionality:**
*   At the end of any game (training or random), if the player's score is high enough, their name and score should be added to a high score table.
*   The high score table should store the top 5 scores.
*   This table should be saved to a text file (e.g., `highscores.txt`).
*   When the program starts, it should attempt to load existing high scores from this file.
*   A new option in the main menu should allow the user to view the high score table.

**What you need to do**

**Task 21.1**
Create new methods:
*   `LoadHighScores()`: This method should:
    *   Attempt to open and read `highscores.txt`.
    *   Parse the file content (e.g., "Name,Score" per line) into a suitable data structure (e.g., a list of tuples or dictionaries).
    *   If the file doesn't exist or is invalid, it should return an empty high score list.
*   `SaveHighScores(HighScoreList)`: This method should:
    *   Take a list of high scores.
    *   Write it to `highscores.txt` in a clear format.
*   `UpdateHighScores(PlayerName, Score, HighScoreList)`: This method should:
    *   Add the new score to the list.
    *   Sort the list by score in descending order.
    *   Keep only the top 5 scores.
    *   Return the updated list.
*   `DisplayHighScores(HighScoreList)`: This method should neatly display the high scores.

**Task 21.2**
Modify the `Main` method:
*   Call `LoadHighScores()` at the start.
*   Add an option to the initial menu (e.g., "Enter H to view high scores") that calls `DisplayHighScores()`.
Modify the `PlayGame` method:
*   When the game is over, prompt the user for their name.
*   Call `UpdateHighScores()` with the player's name and final score.
*   Call `SaveHighScores()` with the updated list.
*   Display the updated high scores.

**Task 21.3**
Test that the changes you have made work:
*   **Test 1 (First Run):** Run the program. Select the option to view high scores. Show that it's empty or indicates no scores yet.
*   **Test 2 (Playing and Saving):** Play a short game and achieve a score. Enter a name. Show the high scores displayed, including the new score. Close the program.
*   **Test 3 (Loading and Updating):** Re-run the program. View high scores and show the score from Test 2 is present. Play another game, achieve a different score (try to get one higher and one lower than the first to test sorting and truncation if you play multiple times to fill the top 5). Show the updated high score table. Check the content of `highscores.txt`.

**Evidence that you need to provide:**
*   Your PROGRAM SOURCE CODE showing the new methods (`LoadHighScores`, `SaveHighScores`, `UpdateHighScores`, `DisplayHighScores`) and the amended `Main` and `PlayGame` methods. [7 marks]
*   SCREEN CAPTURE(S) showing the required tests, including a capture of the `highscores.txt` file content after a few scores have been saved. [2 marks]

---

**Task 22: Strategic Target Prioritization and "Clear Streak" Bonus**

**Marks: 8**

This question extends the game by adding a strategic element to target removal when an expression evaluates to a number present multiple times in the `Targets` list and introduces a "Clear Streak" bonus.

**Introduce new functionality:**
1.  **Target Prioritization:** If an expression evaluates to a number that appears multiple times in the `Targets` list:
    *   The instance of the target closest to the *front* of the `Targets` list (lowest index) should be prioritised for removal.
    *   The player should be awarded a small bonus (e.g., +1 additional point) for clearing the front-most instance of a duplicate target.
    *   Only *one* instance of the target is removed per valid expression, even if multiple exist. (This modifies current behaviour where "all instances...are removed").
2.  **Clear Streak Bonus:**
    *   If a player successfully clears a target for 3 consecutive turns, they receive a "Clear Streak!" message and an additional bonus (e.g., +5 points). The streak counter then resets.

**What you need to do**

**Task 22.1**
Modify the `CheckIfUserInputEvaluationIsATarget` method:
*   Change its logic so that if `UserInputEvaluation` is found in `Targets`:
    *   It finds all indices where the target exists.
    *   It removes only the instance at the *lowest* index.
    *   If this removed target was part of a set of duplicates (i.e., there was more than one instance of `UserInputEvaluation` before removal), award an additional +1 point to `Score`.
    *   The standard +2 points for hitting a target should still apply.
*   Ensure the method still correctly returns `UserInputEvaluationIsATarget` and the updated `Score`.

Modify the `PlayGame` method:
*   Introduce a new variable, `StreakCounter`, initialized to 0.
*   If `CheckIfUserInputEvaluationIsATarget` indicates a target was hit, increment `StreakCounter`.
*   If a turn results in no target hit (or invalid input), reset `StreakCounter` to 0.
*   If `StreakCounter` reaches 3:
    *   Print "Clear Streak!"
    *   Add a bonus (e.g., +5) to `Score`.
    *   Reset `StreakCounter` to 0.

**Task 22.2**
Test that the changes you have made work:
*   Run the Skeleton Program for a training game (or manipulate a standard game to have duplicate targets).
*   **Test 1 (Target Prioritization & Bonus):**
    *   Ensure the `Targets` list has a duplicate target, e.g., `[..., 50, ..., 50, ...]`.
    *   Enter an expression that evaluates to this duplicate target (e.g., 50).
    *   Show that only the first instance (lowest index) of 50 is removed.
    *   Show that the score reflects the standard +2 points, plus the +1 bonus for clearing a prioritised duplicate.
*   **Test 2 (Clear Streak):**
    *   Play three consecutive turns where you successfully clear a target each time.
    *   Show the "Clear Streak!" message and the score reflecting the streak bonus after the third successful clear.
    *   On the fourth turn, deliberately enter an invalid expression or one that doesn't hit a target. Then, on the fifth turn, clear a target. Show that the streak does not continue (i.e., it correctly reset).

**Evidence that you need to provide:**
*   Your PROGRAM SOURCE CODE showing the modifications to `CheckIfUserInputEvaluationIsATarget` and `PlayGame` methods. [6 marks]
*   SCREEN CAPTURE(S) showing the required tests. [2 marks]

---

**Task 23: "Sudden Death" Mode & Target Density**

**Marks: 11**

This question introduces a new challenging game mode, "Sudden Death," and modifies target generation based on game progression to increase difficulty.

**Introduce new functionality:**
1.  **Sudden Death Mode:**
    *   A new option in the `Main` menu to select "Sudden Death" mode.
    *   In this mode, the game starts with `MaxNumberOfTargets` (e.g., 20) targets.
    *   No new targets are added to the end of the `Targets` list after a turn (`UpdateTargets` will shift, but the end will become empty, effectively shortening the list).
    *   The game ends if `Targets[0]` is not -1 (as normal) OR if the player fails to clear a target on any turn where there are still clearable targets available.
    *   The objective is to clear all targets. A bonus is awarded if all initial targets are cleared.
2.  **Dynamic Target Density (for Standard Random Game only, not Training or Sudden Death):**
    *   As the player's score increases in a standard random game, the likelihood of `-1` (empty slots) appearing in the *initial* `CreateTargets` generation decreases, making the target list denser and more challenging. For example:
        *   Score 0-5: Standard target generation (first 5 are -1).
        *   Score 6-10: Only first 3 are -1 initially.
        *   Score 11+: Only first 1 is -1 initially.
    *   This density change only applies when `CreateTargets` is called (i.e., at the start of a *new* standard game, not dynamically mid-game). It would be relevant if the game were structured to allow multiple rounds or if `CreateTargets` was called based on some condition. *For simplicity in this task, assume this logic is applied based on a hypothetical "previous game's score" or a setting chosen before starting a new standard game.*
        *Self-correction for task clarity: Since the skeleton re-creates targets only at game start, let's tie this to a *difficulty setting selected at the start of a standard random game* rather than dynamic score, as that's more directly implementable with current structure.*

**Revised Dynamic Target Density for Task 23:**
2.  **Difficulty-Based Target Density (for Standard Random Game):**
    *   When starting a standard random game, after choosing not to play training, prompt the user for a difficulty: "Easy", "Normal", "Hard".
    *   "Easy": `CreateTargets` makes the first 5 slots -1.
    *   "Normal": `CreateTargets` makes the first 3 slots -1.
    *   "Hard": `CreateTargets` makes the first 1 slot -1.
    *   The rest of the `MaxNumberOfTargets` are filled with random targets.

**What you need to do**

**Task 23.1**
Modify `Main` method:
*   Add "Sudden Death" (e.g., 's') and standard game difficulty selection.
*   Pass a `gameMode` parameter (e.g., "standard_easy", "standard_normal", "standard_hard", "sudden_death", "training") to `PlayGame`.
*   If a standard game difficulty is chosen, pass the corresponding initial empty slot count to `CreateTargets`.

Modify `CreateTargets(MaxNumberOfTargets, MaxTarget, InitialEmptySlots)`:
*   Take `InitialEmptySlots` as a new parameter.
*   Fill the first `InitialEmptySlots` with -1, and the rest with random targets up to `MaxNumberOfTargets`.

Modify `PlayGame`:
*   Accept `gameMode`.
*   If `gameMode` is "sudden_death":
    *   The game over condition becomes: `Targets[0] != -1` OR (target was not cleared AND any `T != -1` exists in `Targets`).
    *   Keep track if all initial targets were cleared. If so, award a significant bonus (e.g., +25 points) at the end.

Modify `UpdateTargets`:
*   If `gameMode` is "sudden_death", when targets are shifted, do not append a new target. The list effectively shortens from the end (or the end becomes and stays -1 if list size is fixed). One way is to append -1 and if the list consists only of -1s, and game isn't over by other means, then all targets cleared.

**Task 23.2**
Test that the changes you have made work:
*   **Test 1 (Standard Game Difficulty):**
    *   Start a standard random game, choose "Hard" difficulty. Show the initial `Targets` list having only one -1 at the start.
    *   Start another standard random game, choose "Easy" difficulty. Show the initial `Targets` list having five -1s at the start.
*   **Test 2 (Sudden Death - Win):**
    *   Start "Sudden Death" mode. Successfully clear all targets.
    *   Show the game ending, the "all clear" bonus being awarded, and the final score.
    *   Show that `UpdateTargets` does not add new random targets at the end of the list during the game.
*   **Test 3 (Sudden Death - Lose by Failing to Clear):**
    *   Start "Sudden Death" mode. Make a valid move that clears a target.
    *   On the next turn, with clearable targets still on the board, enter an expression that does NOT clear a target.
    *   Show the game ending due to failure to clear an available target.

**Evidence that you need to provide:**
*   Your PROGRAM SOURCE CODE showing modifications to `Main`, `PlayGame`, `CreateTargets`, and `UpdateTargets`. [9 marks]
*   SCREEN CAPTURE(S) showing the required tests. [2 marks]

---

**Task 24: "Operator Overload" Challenge and Smart Number Refill**

**Marks: 12**

This question introduces a recurring mini-challenge called "Operator Overload" and makes the refilling of `NumbersAllowed` smarter based on the numbers used.

**Introduce new functionality:**
1.  **Operator Overload Mini-Challenge:**
    *   Every 5 turns (in standard or training mode, not Sudden Death), an "Operator Overload" challenge is triggered.
    *   The game randomly selects one operator (+, -, \*, /).
    *   The player is informed: "Operator Overload! You MUST use the [selected operator] in your expression this turn."
    *   If the player successfully uses the specified operator AND hits a target: +3 bonus points.
    *   If the player hits a target BUT DOESN'T use the specified operator: -3 penalty points (on top of the normal -1 for the turn if it were an invalid move, or overriding the +2 if it was a valid hit without the operator).
    *   If the player fails to hit a target (regardless of operator usage): normal scoring rules apply (likely -1).
2.  **Smart Number Refill (for Standard Random Game only):**
    *   When numbers are removed from `NumbersAllowed` (after a successful hit) and need to be refilled:
        *   Instead of purely random numbers, one of the new numbers generated will be the *sum* or *product* (chosen randomly) of two of the numbers that were just *used* by the player in their successful expression, provided this result is within `MaxNumber` bounds and not excessively large (e.g. not > `MaxNumber` * 2 to keep it somewhat usable).
        *   The other numbers needed to refill `NumbersAllowed` to 5 are generated randomly as usual.
        *   This makes the next turn potentially related to the previous one.

**What you need to do**

**Task 24.1**
Modify `PlayGame` method:
*   Introduce a `turnCounter`.
*   Every 5 turns, trigger "Operator Overload":
    *   Randomly select an `enforcedOperator`.
    *   Display the challenge message to the user.
    *   Pass `enforcedOperator` (or `None` if not an overload turn) to `CheckIfUserInputEvaluationIsATarget`.
Modify `CheckIfUserInputEvaluationIsATarget`:
*   Accept `enforcedOperator` as a parameter.
*   If `enforcedOperator` is not `None`:
    *   Check if `enforcedOperator` is present in the `UserInputInRPN`.
    *   Adjust scoring based on whether a target was hit AND the `enforcedOperator` was used, as per the rules above.
Modify `RemoveNumbersUsed` (or the logic around it in `PlayGame`):
*   This function currently just removes. You'll need to capture which numbers *were* used. It might be better to get the list of used numbers directly from `UserInputInRPN` inside `PlayGame` *before* calling `RemoveNumbersUsed`.
Modify `FillNumbers`:
*   Accept `gameMode` and `usedNumbersList` (which could be empty if no numbers were used or not applicable).
*   If `gameMode` is standard random and `usedNumbersList` contains at least two numbers:
    *   Randomly pick two numbers from `usedNumbersList`.
    *   Randomly decide to calculate their sum or product.
    *   If the result is valid (positive, within `MaxNumber` (perhaps with a small margin like `MaxNumber * 2`), add this "smart" number to `NumbersAllowed`.
    *   Then, fill the rest of `NumbersAllowed` up to 5 with standard random numbers.
*   Otherwise (training, sudden death, or not enough used numbers), fill randomly as before.

**Task 24.2**
Test that the changes you have made work:
*   Run the Skeleton Program.
*   **Test 1 (Operator Overload - Success):**
    *   Play until an "Operator Overload" challenge triggers (e.g., for operator '*').
    *   Enter an expression that uses '*' and hits a target.
    *   Show the score reflecting the +3 bonus.
*   **Test 2 (Operator Overload - Partial Success/Fail):**
    *   Play until an "Operator Overload" challenge triggers (e.g., for operator '+').
    *   Enter an expression that hits a target but uses a different operator (e.g., '-').
    *   Show the score reflecting the -3 penalty.
*   **Test 3 (Smart Number Refill):**
    *   Play a standard random game.
    *   Successfully hit a target using at least two numbers (e.g., numbers 3 and 4, expression 3*4=12).
    *   Show the `NumbersAllowed` list for the *next* turn. It should contain new random numbers, and potentially 7 (3+4) or 12 (3*4) (if within `MaxNumber` bounds for the standard game) as one of the refilled numbers. You may need to run this test a few times to observe the smart refill if it's random which numbers are picked from `usedNumbersList` or whether sum/product is chosen. Make `MaxNumber` small (like 10 or 15) to make the sum/product more likely to be in range.

**Evidence that you need to provide:**
*   Your PROGRAM SOURCE CODE showing modifications to `PlayGame`, `CheckIfUserInputEvaluationIsATarget`, and `FillNumbers` (and `RemoveNumbersUsed` if you changed its role significantly or related logic). [10 marks]
*   SCREEN CAPTURE(S) showing the required tests. [2 marks]

---

These tasks incorporate more complex logic, interactions between different game components, file handling, and user-driven choices, which should provide a good challenge. Good luck with your preparation!
