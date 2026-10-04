# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").
When I first ran the game, I noticed that the game state did not reset correctly when I started a new game. The attempts reset to 0, but the score stayed at -5 instead of returning to its starting value. I also noticed that the feedback hints were backwards: a guess that was too high told me to go higher, and a guess that was too low told me to go lower. While inspecting the scoring code, I also found that update_score() could add 5 points for a "Too High" result on an even-numbered attempt.
**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

| Input | Expected Behavior | Actual Behavior | Console Output / Error |
**Bug Reproduction Log**

| Input | Expected Behavior | Actual Behavior | Console Output / Error |
|-------|-------------------|-----------------|------------------------|
| Clicked "New Game" | A new game should start with attempts and score reset to their starting values. | Attempts reset to 0, but the score remained at -5. | No console error observed. |
| Guess higher than the secret number | The game should say the guess is too high and tell the player to go lower. | The result says "Too High" but the hint says "Go HIGHER!" | No console error observed. |
| Guess too high on an even-numbered attempt | A wrong guess should not increase the player's score. | The `update_score()` logic can add 5 points for a "Too High" result on an even-numbered attempt. | No console error observed. |

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion you did not accept as written (including what the AI suggested, why you rejected or changed it, and how you verified your version). It does not have to be a suggestion that was wrong: over-engineered, out of scope, harder to read, or a poor fit for this codebase all count.

I used Claude Code as an AI coding teammate during the debugging and refactoring process. One suggestion that was correct was adding st.session_state.score = 0 when the New Game button is pressed; I verified it by running the game and checking that the score reset along with the attempts. Another important suggestion was moving check_guess() from app.py into logic_utils.py, which I reviewed and then verified with pytest. When the refactor caused the existing tests to fail because they expected a string instead of the (outcome, message) tuple, I did not change the function just to satisfy the tests; I chose to update the tests to match the existing function contract and added checks for the corrected hints.


One suggestion from Claude was to add st.session_state.score = 0 when the New Game button is pressed; after a save conflict, I manually made sure that change was present and then verified the behavior by testing the game.
---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

I decided a bug was fixed only after checking both the code and the behavior of the application. I manually tested the game to verify that the high and low hints were correct and that starting a new game reset the score. I also ran the pytest tests after the refactor, and all three tests passed, including tests that verify the "Go LOWER!" and "Go HIGHER!" hints. AI helped me understand the mismatch between the tests and the function's tuple return value and helped me make the tests verify the behavior more precisely.
---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?


I learned that Streamlit reruns the Python script when the user interacts with widgets such as buttons. Session state allows values such as the secret number, score, and attempts to survive those reruns instead of being recreated from scratch every time. I would explain it as Streamlit repeatedly rebuilding the page while st.session_state acts like a small memory that keeps important game values between those rebuilds.
---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.

One habit I want to reuse is making small changes, testing them, and committing meaningful changes separately with Git. I also want to continue giving AI small, specific tasks and reviewing its changes instead of accepting everything automatically. This project showed me that AI-generated code can be useful for debugging and refactoring, but I still need to understand the code, verify its behavior, and make the final engineering decisions myself.