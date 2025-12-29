# Nim Q-Learning AI

A Python implementation of the classic Nim game featuring an AI agent that learns to play optimally through Q-learning reinforcement learning and self-play training.

## Game Rules

Nim is a two-player mathematical strategy game:

- The game starts with multiple piles of objects (default: piles of 1, 3, 5, and 7 objects)
- Players take turns removing **any number of objects** from a **single pile**
- A player must remove at least one object on their turn
- **The player who takes the last object loses**

## Implementation Details

### Q-Learning Approach

The AI uses Q-learning, a model-free reinforcement learning algorithm that learns the value of state-action pairs through experience. The agent plays against itself, updating Q-values based on game outcomes:

- **Winning moves** receive positive rewards (+1)
- **Losing moves** receive negative rewards (-1)
- **Intermediate moves** receive no immediate reward (0)

The Q-learning update formula:

```
Q(s, a) ← Q(s, a) + α × (reward + future_rewards - Q(s, a))
```

### Core AI Functions

| Function | Description |
|----------|-------------|
| `get_q_value(state, action)` | Returns the Q-value for a state-action pair (0 if not yet learned) |
| `update_q_value(state, action, old_q, reward, future_rewards)` | Updates Q-values using the Q-learning formula |
| `best_future_reward(state)` | Returns the maximum Q-value among all available actions in a state |
| `choose_action(state, epsilon)` | Selects an action using epsilon-greedy strategy |

### Epsilon-Greedy Exploration

The AI uses an epsilon-greedy strategy to balance exploration and exploitation:

- With probability **ε (epsilon)**: Choose a random action (exploration)
- With probability **1 - ε**: Choose the best known action (exploitation)

This ensures the AI explores new strategies while still leveraging learned knowledge.

## Usage Instructions

### Training the AI

Train the AI by having it play games against itself:

```python
from nim import train, play

# Train the AI with 10,000 self-play games
ai = train(10000)
```

### Playing Against the AI

After training, play against the AI:

```python
# Play a game against the trained AI
play(ai)

# Optionally specify if human plays first (0) or second (1)
play(ai, human_player=0)  # Human moves first
play(ai, human_player=1)  # AI moves first
```

### Complete Example

```python
from nim import train, play

# Train the AI
ai = train(10000)

# Play against the trained AI
play(ai)
```

During gameplay, you'll be prompted to:
1. Choose a pile (0-indexed)
2. Choose how many objects to remove

## Requirements

- **Python**: 3.6 or higher
- **Dependencies**: None (uses only Python standard library)
  - `random` - for epsilon-greedy action selection
  - `math` - for mathematical operations
  - `time` - for gameplay pacing

## Parameters

### NimAI Configuration

| Parameter | Default | Description |
|-----------|---------|-------------|
| `alpha` | 0.5 | **Learning rate** - Controls how much new information overrides old Q-values. Higher values mean faster learning but less stability. |
| `epsilon` | 0.1 | **Exploration rate** - Probability of choosing a random action during training. Higher values encourage more exploration. |

### Customizing Parameters

```python
from nim import NimAI, Nim

# Create AI with custom parameters
ai = NimAI(alpha=0.7, epsilon=0.2)

# You can also customize the initial pile configuration
game = Nim(initial=[2, 4, 6])  # Custom piles
```

### Parameter Tuning Tips

- **Higher alpha (0.7-0.9)**: Faster adaptation, good for simple environments
- **Lower alpha (0.1-0.3)**: More stable learning, better for complex scenarios
- **Higher epsilon (0.2-0.4)**: More exploration during training
- **Lower epsilon (0.05-0.1)**: More exploitation of learned strategies

## How It Works

1. **State Representation**: Game states are represented as tuples of pile sizes (e.g., `(1, 3, 5, 7)`)
2. **Action Representation**: Actions are tuples `(i, j)` where `i` is the pile index and `j` is the number to remove
3. **Q-Table**: A dictionary mapping `(state, action)` pairs to their learned Q-values
4. **Training**: The AI plays against itself, learning from wins and losses
5. **Playing**: The trained AI uses its Q-table to select optimal moves

