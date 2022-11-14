# Playing Punish with Reinforcement Learning
### Jim Shepich III
### Updated: 14 November 2022
### Time Required: 56 hours 42 minutes

This repository contains the data, models, results, and analysis of my efforts to use reinforcement learning to train artificial agents to play PUNISH

## About Punish
PUNISH a fast-paced card duelling game developed by Happy Slaying Studios. More information about PUNISH can be found at [happyslaying.gg/articles/punish-instructions](https://happyslaying.gg/articles/punish-instructions). A virtual version of PUNISH can be downloaded for free for Windows or Android devices from [thecometcloud.itch.io/punish](https://thecometcloud.itch.io/punish) (note: downloads from Itch do not always work on Chrome).

To play against artificial agents developed in this project:

1. Download the desired agent's Q-function JSON file from the `agents/` directory of this repository
2. Drop it into the `agents/` directory of the folder containing your local copy of the game
3. Set the `path` option in the `config.txt` file to the agents's filename; set the `name` to whatever you want displayed in the logs
4. Host a game, and then ready up in the empty room; your opponent will be the agent

Feel free to send us your data (the `saves.json` file in the root directory of your game folder) to include in our analyses!

## How to Run Reinforcement Learning Project
The code used in this project was developed using version 1.8.0 of Julia. In order to run Julia in a Jupyter Notebook environment, you will need to add and build the `IJulia` package. The additional required packages are:

```
Combinatorics
StatsBase
Random
JSON
BenchmarkTools
Plots
PlotlyJS
WebIO
StatsPlots
SQLite
DataFrames
OrderedCollections
Flux
```

Add JSON data formatted by the PUNISH app to the database `data/punish_data.db` by running the following command-line command:

```cmd
julia json2db.jl saves.json
```

Where `saves.json` can be replaced by the name of your file.

The code used to represent the game, generate environmental models, train agents, and conduct analysis, as well as the results and discussion of said analyses are contained in `punish_rl.ipynb`. The **Notebook Settings** section allows a user to control the behavior of the notebook. Because many algorithms, such as those that enumerate the state space and generate state-action transition functions, are computationally-costly, the default behavior is to load the saved results from disk rather than generate them fresh each time. This type of setting can be toggled in the `CONFIG` dictionary.

## Description of Files
 -  `play_punish.jl`: an unfinished (rad: defunct) command-line version of PUNISH
 -  `punish_rl.ipynb`: the main analytical notebook of this investigation
 -  `run_times.json`: a file that stores the measured run-times of experiments, so they may be displayed/recalled without running costly algorithms from scratch every session
 -  `training_log.json`: a file that tracks the results of training of machine learning algorithms, i.e. neural network backprop and value iteration, so they may be displayed/recalled without running costly algorithms from scratch every session
 -  `punish_rl_nbexport.html`: a HTML-export of the main analytical notebook, for your reading convenience
 -  `envs/`
     - `*_strategies.json`: probability distributions of the possible actions of a set of given states; generated by the PARLESS technique
     - `*_parameters.json`: JSON-serialized trained neural network parameters
     - `statespace.json`: a JSON list containing the integer encoding of every possible gamestate
     - `transitions.part*.rar`: RAR-compressed volume files containing the JSON-serialized state-action transition functions (too big to upload to GitHub as individual files)
- `data/`
     - `punish_data.sql`: the DDL file that defines the structure of the `punish_data.db` database
     - `punish_data.bat`: a Batch file that uses `punish_data.sql` to generate an empty database `punish_data.db`
     - `punish_data.db`: a SQLite database containing normalized game data
     - `json2db.jl`: a Julia script that normalizes JSON log files, ensures all states and actions are valid, and loads them into `punish_data.db`
     - `state_action_validation.jl`: an auxiliary file to `json2db.jl` containing a few functions from `punish_rl.ipynb` for the purpose of ensuring the validity of states and actions
     - `*saves*.json`: log files used in training or testing the agents, exported by the PUNISH Windows app
- `agents/`
     - `q_*.json`: the state-action value function corresponding to an agent trained in this study  
