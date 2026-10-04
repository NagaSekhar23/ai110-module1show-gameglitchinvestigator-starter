# AI Interactions Log

> **Stretch features only.** Only fill in the sections that apply to stretch features you attempted. If you did not attempt a stretch feature, leave its section blank or delete it. This file is not required for the core project.

---

## AI-Assisted Debugging & Refactoring Summary

I used Claude Code to help fix two bugs I had identified myself, then complete the Phase 2 refactor. I gave one scoped task at a time ("fix only this bug", "do not commit"), checked each diff, and ran the tests myself.

### Bug 1: Reversed hint messages
- **Problem:** In `check_guess()`, a guess above the secret returned "Go HIGHER!" and a guess below it returned "Go LOWER!".
- **Fix:** Swapped the hint strings so "Too High" returns "📉 Go LOWER!" and "Too Low" returns "📈 Go HIGHER!". The `(outcome, message)` return structure was kept.
- **Review:** The AI also fixed the same reversed messages in the `except TypeError` fallback branch, which I had not pointed out. I checked the diff to confirm that was correct and in scope.

### Bug 2: New Game did not reset the score
- **Problem:** Clicking "New Game" reset `attempts` and picked a new secret, but `st.session_state.score` kept its old value.
- **Fix:** Added `st.session_state.score = 0` to the `if new_game:` block. The initial score is `0`, set where the session state is first created.
- **Not changed:** The AI noted that "New Game" also does not reset `status` or `history`. That is outside this bug, so I left it alone.

### Refactor: `check_guess()` moved to `logic_utils.py`
- Moved the logic of `check_guess()` from `app.py` into `logic_utils.py`, replacing the `NotImplementedError` stub. The reversed-hint fix and the `except TypeError` fallback came with it.
- Removed the local copy from `app.py` and added `from logic_utils import check_guess`. The call site was not changed.
- The other three stubs in `logic_utils.py` (`get_range_for_difficulty`, `parse_guess`, `update_score`) were not part of this change.

### Pytest changes
- After the refactor, all 3 tests in `tests/test_game_logic.py` failed. The tests compared the whole return value to a string (`assert result == "Win"`), but `check_guess()` returns a tuple such as `("Win", "🎉 Correct!")`.
- The AI offered two options: update the tests, or change `check_guess()` to return only the outcome string. I chose to update the tests, since the second option would change the function's return structure and require changes in `app.py`.
- The tests now unpack the tuple (`outcome, message = check_guess(...)`) and assert the outcome separately. The too-high and too-low tests also check that the message contains "Go LOWER!" and "Go HIGHER!". These hint assertions would have failed on the original reversed messages.

### Result
`3 passed` in `tests/test_game_logic.py` (`test_winning_guess`, `test_guess_too_high`, `test_guess_too_low`).

### Reviewing AI suggestions
I did not accept the AI's output blindly. Each task was limited to one change, I reviewed the diffs, and I ran the tests. When the refactor made the tests fail, I decided how to resolve the mismatch instead of letting the tests be edited automatically. The tests were only changed after I chose that option.

---

## Agent Workflow (SF8)

> Document your experience using an AI agent (e.g., Cursor Agent, Claude, Copilot) to make multi-step changes autonomously.

**What task did you give the agent?**

<!-- Describe the goal you asked the agent to accomplish -->

**What did the agent do?**

<!-- List the steps the agent took (files edited, commands run, etc.) -->

**What did you have to verify or fix manually?**

<!-- Describe anything the agent got wrong or that required human review -->

---

## Test Generation (SF7)

> Document how you used AI to help generate or improve tests.

| Edge Case | Prompt Used | AI-Suggested Test | Did It Pass? | Your Reasoning |
|-----------|-------------|-------------------|--------------|----------------|
| | | | | |
| | | | | |
| | | | | |

---

## Linting & Style (SF9)

> Document your use of AI for linting or code style improvements.

**Prompt used:**

```
<!-- Paste the prompt you gave the AI -->
```

**Linting output before:**

```
<!-- Paste relevant linter warnings/errors -->
```

**Changes applied:**

<!-- Describe what you changed based on the AI's suggestions -->

---

## Model Comparison (SF11)

> Compare two AI models on the same task.

**Task given to both models:**

<!-- Describe what you asked each model to do -->

| | Model A | Model B |
|-|---------|---------|
| **Model name** | | |
| **Response summary** | | |
| **More Pythonic?** | | |
| **Clearer explanation?** | | |

**Which did you prefer and why?**

<!-- Your conclusion -->
