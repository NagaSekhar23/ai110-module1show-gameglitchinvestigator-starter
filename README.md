# 🎮 Game Glitch Investigator: The Impossible Guesser

A number-guessing game built with Streamlit. The starter app was AI-generated and contained several bugs. This project documents how those bugs were investigated and fixed, how the core guessing logic was refactored into its own module, and how it was tested.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the app: `python -m streamlit run app.py`

## 🐞 Bugs Investigated and Fixed

- **Reversed hint messages:** `check_guess()` told the player to "Go HIGHER!" when the guess was too high and "Go LOWER!" when it was too low. The hints now match the outcome.
- **New Game did not reset the score:** clicking "New Game" reset attempts but kept the old score. The score now resets to its initial value.
- **Incorrect submit button call:** the submit button used `st.menu_button`, which raised a `TypeError`. It was corrected to `st.button`.

## 🔧 Refactoring

`check_guess()` was moved from `app.py` into `logic_utils.py`, and `app.py` now imports it from there. Its return structure, `(outcome, message)`, is unchanged.

## ✅ Testing

Tests are in `tests/test_game_logic.py`. They verify the returned outcome ("Win", "Too High", "Too Low") and that the hints are correct: "Go LOWER!" for a guess that is too high and "Go HIGHER!" for a guess that is too low.

Final result: **3 passed**.

## 📸 Demo Walkthrough

1. Start the app with `python -m streamlit run app.py`.
2. Expand **Developer Debug Info** to see the secret number, attempts, and score.
3. Enter a guess in the text box and click **Submit Guess 🚀**.
4. Verify the hint: a guess above the secret should show "Go LOWER!", and a guess below it should show "Go HIGHER!".
5. Click **New Game 🔁** and check **Developer Debug Info** again: attempts and score are back to 0.

## 📝 Documentation

- `reflection.md` contains the investigation and reflection.
- `ai_interactions.md` documents the AI-assisted debugging and refactoring.

## 🔀 Git

The project was committed and pushed to GitHub.
