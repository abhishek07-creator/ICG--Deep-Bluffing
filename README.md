# ICG--Deep-Bluffing
Official repository of ICG-Summer Project 2026-27-Deep Bluffing

Welcome to Project Deep-Bluffing! The goal of this project is to build an autonomous AI bot capable of playing poker. 

# Capstone Challenge: Heads-Up Limit Hold'em (HULHE)

### **The Mission**
Balancing a pole on a cart was a perfect-information environment where you could see every variable. Now, it is time to step into the real world: **Imperfect Information**. 

Your final capstone challenge is to design, engineer, and train an autonomous AI agent capable of playing **Heads-Up Limit Texas Hold'em (HULHE)**. 

To prove your software engineering fundamentals, you will not be relying on pre-built poker libraries. You must architect the underlying game engine logic from scratch using Object-Oriented Python, and then build a bot smart enough to dominate the game tree.

---

### **Game Specifications (The Scope)**
To ensure the mathematics remain discrete and the game-tree branching factors are clean and manageable, we are enforcing the following strict architectural rules:
* **Format:** Heads-Up (1-vs-1). No multi-way pots.
* **Betting Structure:** **Limit Hold'em**. You cannot bet arbitrary chip amounts. At any decision point, your bot only has three discrete choices: `FOLD`, `CALL`, or `RAISE` (by a strictly fixed limit increment). 
* **The Deck:** A standard 52-card deck.

---

### **Technical Constraints & Rules**
This is a comprehensive test of pure algorithmic logic and clean software design. 

**NOT ALLOWED:**
* External poker simulation or evaluation libraries (e.g., `RLCard`, `PyPokerEngine`, `treys`).
* Hard-coded pre-computed lookup strategy tables downloaded from the internet.

**ALLOWED:**
* Machine Learning & Numerics: `numpy`, `torch`, `math`.
* Python Standard Libraries: `itertools`, `collections`, `random`.
* **Strategy Archetype:** You are free to explore any methodology. You can adapt your Deep Q-Network (DQN) architecture to handle hidden states, build a Monte Carlo Tree Search (MCTS) rollout framework, implement Counterfactual Regret Minimization (CFR), or design deep mathematical Expected Value (EV) heuristics.

---

### **The Architecture Blueprint & API (CRITICAL)**
Your bot will be dynamically extracted and loaded into a master tournament arena to play thousands of hands against your peers. **If your bot does not inherit from the base class or changes this exact function signature, it will crash the arena and receive a 0.**

Your code structure must provide a designated main entry point file named exactly `agent.py`. Inside `agent.py`, implement your agent using this exact class template:

```python
class BasePokerBot:
    def __init__(self, name):
        self.name = name

    def get_action(self, hole_cards, community_cards, pot_size, stack_size, amount_to_call, legal_actions):
        """
        Calculates the optimal move for Limit Texas Hold'em.
        
        Parameters:
        - hole_cards (list): Your two private cards, e.g., ['Ah', 'Kd']
        - community_cards (list): Shared public cards, e.g., ['7s', '7c', '2d'] (Empty pre-flop)
        - pot_size (int): Total chips currently in the middle pot
        - stack_size (int): Your remaining chips in your stack
        - amount_to_call (int): Chips required to put in to stay in the hand
        - legal_actions (list): Available valid moves, e.g., ['FOLD', 'CALL', 'RAISE']
        
        Returns:
        - A string exactly matching ONE of the elements in legal_actions.
        """
        raise NotImplementedError("Your bot logic goes here!")

# Implement your subclass here
class CustomPokerBot(BasePokerBot):
    def get_action(self, hole_cards, community_cards, pot_size, stack_size, amount_to_call, legal_actions):
        # Your custom decision network / game theory logic goes here
        # Return string: 'FOLD', 'CALL', or 'RAISE'
        pass
```
*Note: Cards are represented as strings, where the first character is the rank (`2`-`9`, `T`, `J`, `Q`, `K`, `A`) and the second character is the suit lowercase (`h` for hearts, `d` for diamonds, `c` for clubs, `s` for spades).*

### **The Research Board (Where to Start)**
Since you are building the card evaluation and engine yourself, utilize these built-in Python paradigms to avoid writing convoluted spaghetti code:
* **Python OOP Magic:** Implement dunder methods (`__eq__`, `__lt__`, `__gt__`) on a custom `Card` object to make sorting hands and comparing absolute values trivial. You can refer cherno's cpp playlist. For python particularly, you can refer [this](https://youtu.be/Mf2RdpEiXjU?si=9yL8XBqiNFBGRVL8)  
* **Hand Combinations:** Use `itertools.combinations` to easily extract the best 5-card combinations out of the 7 available cards (2 hole cards + 5 community cards).
* **Card Frequency Counting:** Use `collections.Counter` to check for pairs, pairs of pairs, trips, full houses, and matching flush suits effortlessly.
* **Game Theory Context:** Read up on *Monte Carlo Simulations/Rollouts* (simulating random outcomes from the current state to gauge hand strength) and *Expected Value calculations* ($EV = [\text{Probability of Winning} \times \text{Pot Size}] - [\text{Probability of Losing} \times \text{Amount to Call}]$).

---

### **Evaluation & Grading Rubric**
Your project grade is split evenly between programmatic execution and your theoretical approach:
* **75% — The Master Tournament:** Your agent folder will be compiled, and your bot will run through a grueling round-robin tournament (playing 10,000 hands against baseline agents and your peers). Points are awarded based on stability, invalid action avoidance, and cumulative win-rate performance.
* **25% — The LaTeX Defense (`report.pdf`):** You must submit a professional academic report compiled strictly in LaTeX. You must mathematically justify your state representation (how you quantized cards/hand strength into numeric features), your reward shaping mechanics (if using Reinforcement Learning), or your mathematical heuristics architecture.

---

### **How to Submit Your Solution**
We are utilizing a Pull Request (PR) workflow on our central repository. Follow these exact guidelines:
1. **Fork this repository** to your personal GitHub account.
2. **Clone your fork** locally to build out your modules. You are free to use multiple files (`model.py`, `utils.py`, etc.) to keep your code clean, modular, and professional.
3. **Create your submission directory** inside the `submissions/` folder. Your folder must be named exactly according to your name and roll number:  
    `submissions/firstname_lastname_rollnumber/`
4. **Organize your deliverables** directly within your personal folder:
   ```text
   submissions/firstname_lastname_rollnumber/
   ├── agent.py        # CRITICAL: Main entry point containing your custom bot class
   ├── report.pdf      # Compulsory strategy defense compiled from LaTeX
   └── [Any additional helper scripts, custom modules, or saved PyTorch .pth weights]
   ```
5. Open a Pull Request (PR) from your completed fork back to the main repository before the announced deadline.

### ALL THE BEST!