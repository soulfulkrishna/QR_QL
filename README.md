
## Project goal
This notebook implements and experiments with reinforcement-learning agents that reason about value **distributions** (not just expected values). It contains multiple network architectures and trainers, including recurrent Q-networks, actor-critic components, and hybrid models, and explores quantile-based (distributional) learning techniques. The code is a compact experimental playground rather than a single polished demo.

## Key features and components
- **Environment(s)**: Contains a simple environment class `SimpleGridEnv` used for experiments. The environment implements `reset()` and `step()` and provides observations and discrete actions.
- **Model architectures**:
  - `QLSTMCell`, `QLSTM`: recurrent Q-network implementations (LSTM-based) for partially observable tasks.
  - `ActorCritic`: an on-policy actor-critic model for policy-based experiments.
  - `HybridModel`: combines components (used for experimental comparisons).
  - Quantile/distributional output variants: models that output a set of quantiles per action rather than a single Q-value.
- **Training infrastructure**:
  - Replay buffer for off-policy learning (experience storage and sampling).
  - `OnPolicyTrainer` class for on-policy algorithms and rollout management.
  - Standard training loop(s): collect transitions, sample batches, compute losses, perform optimizer steps, and periodically update a target network.
  - Epsilon-greedy exploration and epsilon scheduling are implemented (several `eps`/`epsilon` references).
- **Utilities**:
  - `set_seed` helper to fix RNG for reproducibility.
  - `main` entry point to run training / evaluation experiments.
  - Logging and evaluation routines to measure episodic return and to produce plots.
