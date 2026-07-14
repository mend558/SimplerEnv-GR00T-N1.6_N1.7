# Comparative Evaluation of GR00T Versions N1.6 and N1.7 in SimplerEnv

> This is the project repository used for the evaluation of GR00T versions in SimplerEnv Benchmark. We provide instructions for running the benchmark with the Google Robot and WidowX Robot and the test results in video form with the notebook used for statistical analysis.

## Setting up

Follow the instructions for instalation of the GR00T model in the repository: for GR00T N1.7 (https://github.com/NVIDIA/Isaac-GR00T) and GR00T N1.6 (https://github.com/NVIDIA/Isaac-GR00T/tree/n1d6). 
It will clone the repository and install GR00T in a uv enviroment with it's dependencies. 

Make sure the shared system libraries are installed:
```bash
sudo apt update
sudo apt install libegl1-mesa-dev libglu1-mesa
```

Run the benchmark setup script (only needed once):
```bash
bash gr00t/eval/sim/SimplerEnv/setup_SimplerEnv.sh
```

## Evaluation

The GR00T policy runs as a server with a client. During creation the checkpoint/embodiment argument must match the robot used in the task.

To create the server we use the following script:

For Google Robot:
```bash
uv run python gr00t/eval/run_gr00t_server.py \
    --model-path nvidia/GR00T-N1.7-SimplerEnv-Fractal \
    --embodiment-tag SIMPLER_ENV_GOOGLE \
    --use-sim-policy-wrapper
```

For WidowX Robot:
```bash
uv run python gr00t/eval/run_gr00t_server.py \
    --model-path nvidia/GR00T-N1.7-SimplerEnv-Bridge \
    --embodiment-tag SIMPLER_ENV_WIDOWX \
    --use-sim-policy-wrapper
```

It allows the use of a remote checkpoint for directly runnable tests.

With a server running you can start the benchmark. Select a task, the number of episodes, max episode steps, enviroments and action steps by changing the argument variables in the following script:

(The arguments provided as default were the ones used in the project)

For Google Robot: 
```bash
gr00t/eval/sim/SimplerEnv/simpler_uv/.venv/bin/python gr00t/eval/rollout_policy.py \
    --n-episodes 1000 \
    --policy-client-host 127.0.0.1 \
    --policy-client-port 5555 \
    --max-episode-steps 300 \
    --env-name simpler_env_google/google_robot_pick_coke_can \
    --n-action-steps 1 \
    --n-envs 5
```

For WidowX Robot:

```bash
gr00t/eval/sim/SimplerEnv/simpler_uv/.venv/bin/python gr00t/eval/rollout_policy.py \
    --n-episodes 1000 \
    --policy-client-host 127.0.0.1 \
    --policy-client-port 5555 \
    --max-episode-steps 300 \
    --env-name simpler_env_widowx/widowx_spoon_on_towel \
    --n-action-steps 4 \
    --n-envs 5
```


## Results

We provide the benchmark results in the respective csv. files. The statistical analysis along with the graphical analysis can be found on the notebook file.
