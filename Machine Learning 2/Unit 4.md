## Reinforcement Learning I: Detailed Notes

**1. What is Reinforcement Learning?**

* Reinforcement learning (RL) is a type of machine learning where an **agent** learns to interact with an **environment** by taking **actions** to maximize a cumulative **reward**.
* It's a trial-and-error learning paradigm. The agent receives feedback in the form of rewards or penalties for its actions, and it adjusts its behavior to achieve the highest possible long-term reward.
* Unlike supervised learning, RL doesn't rely on labeled data. Instead, the agent learns through direct interaction with the environment.
* Key components: Agent, Environment, State, Action, Reward.

**2. The Return in Reinforcement Learning:**

* The **return** (G<sub>t</sub>) at time step *t* is the total cumulative reward the agent receives from that time step onwards, often discounted by a factor gamma (γ).
* **Discounted Return:**  G<sub>t</sub> = R<sub>t+1</sub> + γR<sub>t+2</sub> + γ<sup>2</sup>R<sub>t+3</sub> + ...
* The discount factor γ (0 ≤ γ ≤ 1) determines the importance of future rewards. A value of γ close to 0 prioritizes immediate rewards, while a value close to 1 gives more weight to future rewards.

**3. Making Decisions: Policies in Reinforcement Learning:**

* A **policy** (π) defines how the agent chooses actions in each state.
* It maps states to actions (π(s) = a).
* Policies can be deterministic (always choosing the same action in a given state) or stochastic (assigning probabilities to different actions).

**4. Review of Key Concepts:**

* **State (s):** A representation of the current situation in the environment.
* **Action (a):** An action taken by the agent that can affect the environment.
* **Reward (r):** A numerical signal indicating the desirability of an outcome.
* **State-Action Pair (s, a):** A combination of the current state and the action taken.

**5. State-Action Value Function Definition (Q-function):**

* The state-action value function, denoted as Q(s, a), estimates the expected return the agent will receive if it takes action *a* in state *s* and follows its policy thereafter.
* Q(s, a) = E[G<sub>t</sub> | S<sub>t</sub> = s, A<sub>t</sub> = a]

**6. State-Action Value Function Example:**

Imagine a robot navigating a grid. The state could be the robot's location, and the actions could be moving up, down, left, or right. The Q-function would estimate the expected cumulative reward for taking each action in each location.  For example, Q("location A", "move right") might be higher if moving right leads to a reward.

**7. Bellman Equation:**

* The Bellman equation is a fundamental equation in RL that expresses the relationship between the value of a current state and the values of possible successor states.
* It decomposes the Q-function into the immediate reward and the discounted expected future reward.
* **Bellman Equation for Q(s, a):**  Q(s, a) = R(s, a) + γ * Σ P(s'|s, a) * max<sub>a'</sub> Q(s', a')
    * R(s, a): Immediate reward for taking action *a* in state *s*.
    * P(s'|s, a): Probability of transitioning to state *s'* after taking action *a* in state *s*.
    * γ: Discount factor.
    * max<sub>a'</sub> Q(s', a'): Maximum expected future reward over all possible actions *a'* in the next state *s'*.

**8. Learning the State-Value Function (V-function):**

* The state-value function, V(s), estimates the expected return the agent will receive if it starts in state *s* and follows its policy thereafter.
* V(s) = E[G<sub>t</sub> | S<sub>t</sub> = s]
* It's related to the Q-function: V(s) = max<sub>a</sub> Q(s, a) (for deterministic policies)
* Learning V(s) or Q(s, a) is a core objective in many RL algorithms.  Methods like Temporal Difference learning and Q-learning are used to estimate these functions through experience.


These notes provide a foundation for understanding key concepts in reinforcement learning.  Remember to supplement these notes with practical examples and implementations to solidify your understanding.  Consider exploring different RL algorithms and environments to gain hands-on experience.
