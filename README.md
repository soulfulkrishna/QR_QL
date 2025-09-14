
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
 
## Differences

### Approach:
- **Paper**
  - Uses Quantum LSTM (QLSTM) with variational quantum circuits (VQCs) implemented via PennyLane.
  - Experiments with QLSTM-RC (randomly initialized and frozen as a reservoir).
  - Trains using asynchronous actor-critic (QA3C) with multiple parallel workers.
  - Benchmarks on MiniGrid navigation tasks (5×5, 6×6, 8×8 grids with fixed and random starts).

- **QR_RL**
  - Implements classical or quantum-inspired QLSTM variants using frozen VQCs as stubs.
  - Trains with synchronous A3C-style updates in a single process or small batch.
  - Evaluates on a toy environment (1D grid, left/right actions, partial observations).

- **QR_RL2**
  - Runs evaluation using deterministic and stochastic policies on the same toy environment as Code 1.
  - Uses 100 evaluation episodes and prints details for the first few.
  - Focuses only on evaluation, not training.

### Results:
- **Paper**
  - Reservoirized QLSTM performs comparably to fully trained QLSTM in easy tasks (5×5 grid).
  - Performs worse in harder tasks (8×8 grid with more quantum layers).
  - Reports learning curves, success rates, and comparisons across tasks and random seeds.

- **QR_RL**
  - Shows training metrics like loss, entropy, and average return over epochs.
  - Achieves near-optimal performance in a solved, easy environment.
  - Final evaluation shows mean reward ≈ 0.96 with minimal variance.

- **QR_RL2**
  - Deterministic policy consistently achieves mean = 0.9600, std = 0.0000 over 100 episodes.
  - Stochastic policy shows slight variability with mean ≈ 0.9542, std ≈ 0.0106.
  - Results reflect a simple, solved task rather than challenging navigation benchmarks.

