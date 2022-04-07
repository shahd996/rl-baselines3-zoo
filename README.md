# ML702: Training Policy Gradient Methods using the RL Baselines3 Zoo

RL Baselines3 Zoo is a training framework for Reinforcement Learning (RL). 

Sources:
- [Stable Baselines3](https://github.com/DLR-RM/stable-baselines3)


This repository was used to train RL agents on the Pendulum-V1 environment using the following policy gradient methods:
1. TRPO
2. PPO
3. DDPG
4. SAC
5. TD3

## Objective: 

- Confirm findings and comparative analysis conducted in the Policy Gradient Methods literature review report for the ML702 course at MBZUAI.
- Several plots were generated to confirm the relative performance and convergence rates of the above methods. 
### The plots can be found in the PLOTS folder


The below details are found in the original repository's README.md file:

## Train an Agent

The hyperparameters for each environment are defined in `hyperparameters/algo_name.yml`.

If the environment exists in this file, then you can train an agent using:
```
python train.py --algo algo_name --env env_id
```

For example (with tensorboard support):
```
python train.py --algo ppo --env CartPole-v1 --tensorboard-log /tmp/stable-baselines/
```

Evaluate the agent every 10000 steps using 10 episodes for evaluation (using only one evaluation env):
```
python train.py --algo sac --env HalfCheetahBulletEnv-v0 --eval-freq 10000 --eval-episodes 10 --n-eval-envs 1
```

Save a checkpoint of the agent every 100000 steps:
```
python train.py --algo td3 --env HalfCheetahBulletEnv-v0 --save-freq 100000
```

Continue training (here, load pretrained agent for Breakout and continue training for 5000 steps):
```
python train.py --algo a2c --env BreakoutNoFrameskip-v4 -i rl-trained-agents/a2c/BreakoutNoFrameskip-v4_1/BreakoutNoFrameskip-v4.zip -n 5000
```

When using off-policy algorithms, you can also save the replay buffer after training:
```
python train.py --algo sac --env Pendulum-v0 --save-replay-buffer
```
It will be automatically loaded if present when continuing training.

## Plot Scripts

Plot scripts (to be documented, see "Results" sections in SB3 documentation):
- `scripts/all_plots.py`/`scripts/plot_from_file.py` for plotting evaluations
- `scripts/plot_train.py` for plotting training reward/success

*Examples (on the current collection)*

Plot training success (y-axis) w.r.t. timesteps (x-axis) with a moving window of 500 episodes for all the `Fetch` environment with `HER` algorithm:

```
python scripts/plot_train.py -a her -e Fetch -y success -f rl-trained-agents/ -w 500 -x steps
```

Plot evaluation reward curve for TQC, SAC and TD3 on the HalfCheetah and Ant PyBullet environments:

```
python scripts/all_plots.py -a sac td3 tqc --env HalfCheetah Ant -f rl-trained-agents/
```

## Plot with the rliable library

The RL zoo integrates some of [rliable](https://agarwl.github.io/rliable/) library features.

First, you need to install [rliable](https://github.com/google-research/rliable).

Note: Python 3.7+ is required in that case.

Then export your results to a file using the `all_plots.py` script (see above):
```
python scripts/all_plots.py -a sac td3 tqc --env Half Ant -f logs/ -o logs/offpolicy
```

You can now use the `plot_from_file.py` script with `--rliable`, `--versus` and `--iqm` arguments:
```
python scripts/plot_from_file.py -i logs/offpolicy.pkl --skip-timesteps --rliable --versus -l SAC TD3 TQC
```

Note: you may need to edit `plot_from_file.py`, in particular the `env_key_to_env_id` dictionary
and the `scripts/score_normalization.py` which stores min and max score for each environment.

Remark: plotting with the `--rliable` option is usually slow as confidence interval need to be computed using bootstrap sampling.

## Citing the Project

To cite this repository in publications:

```bibtex
@misc{rl-zoo3,
  author = {Raffin, Antonin},
  title = {RL Baselines3 Zoo},
  year = {2020},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/DLR-RM/rl-baselines3-zoo}},
}
```

## Contributing

If you trained an agent that is not present in the RL Zoo, please submit a Pull Request (containing the hyperparameters and the score too).

## Contributors

We would like to thanks our contributors: [@iandanforth](https://github.com/iandanforth), [@tatsubori](https://github.com/tatsubori) [@Shade5](https://github.com/Shade5) [@mcres](https://github.com/mcres)
