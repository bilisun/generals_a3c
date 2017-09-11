# generals_a3c
This repository contains code to simulate generals.io games and to train both policy networks
and actor critic networks(asynchronously) to play generals.io.

It supports:
1. Simulation of gioreplay files.
2. Client to play online generals.io games.
3. Virtual simulation of generals.io games with autogenerated boards.
4. Dataset generation.
5. Code to train a convolutional policy network on generated dataset.
6. OpenAI Gym like environment to interact with generals game
7. Code for A3C convolutional network training on generals.io game. 

Link of convolutional policy network playing [generals.io](http://bot.generals.io/replays/Be0wkw2t-)

## Generals Dataset Generation

To generate a labeled supervised move dataset to train policy networks run the following commands:

* First download and unzip online database of files found [here](http://dev.generals.io/replays).
* After downloading database run generate_data.py which will generate datasets data_x.npy, data_y.npy, data_z.npy. data_x represents an expanded feature map for generals board, while data_y and data_z represent start and end tiles for moves
```
usage: generate_data.py [-h] [--processes PROCESSES] [--data DATA]
                        [--stars STARS] [--players PLAYERS]

optional arguments:
  -h, --help            show this help message and exit
  --processes PROCESSES
  --data DATA           directory where the gioreplay files are stored
  --stars STARS         threshold for stars to parse games from
  --players PLAYERS     number of players needed so that we parse game from
```

## Policy Bot Training 

To train policy network use the following code:
```
usage: policy_trainer.py [-h] [--on-gpu ON_GPU] [--num-epochs NUM_EPOCHS]
                         [--data DATA] [--lr LR]

optional arguments:
  -h, --help            show this help message and exit
  --on-gpu ON_GPU
  --num-epochs NUM_EPOCHS
                        number of epochs to train network
  --data DATA           directory containing data directory
  --lr LR               LR
```

The data should be generated using generate_data.py
The python file outputs saved model at 'policy.mdl'

### Running Policy Bot on Online Server

To test the policy network on private online bot server on generals.io use:
```
usage: policy_online_client.py [-h] [--user_id USER_ID] [--username USERNAME]
                               [--game_id GAME_ID] [--model_path MODEL_PATH]

Policy Bot Player

optional arguments:
  -h, --help            show this help message and exit
  --user_id USER_ID     user_id for bot
  --username USERNAME   username for bot
  --game_id GAME_ID     id for the game
  --model_path MODEL_PATH
                        path of policy model
```

To quickly demo policy bot, clone the repo and run 
```
python policy_online_client.py
```
and then go to the URL [here](http://bot.generals.io/games/viz0)

## A3C Bot Training 

To train policy network use the following code:
```
usage: main.py [-h] [--lr LR] [--gamma GAMMA] [--tau TAU]
               [--entropy-coef ENTROPY_COEF]
               [--value-loss-coef VALUE_LOSS_COEF]
               [--max-grad-norm MAX_GRAD_NORM] [--seed SEED]
               [--num-processes NUM_PROCESSES] [--num-steps NUM_STEPS]
               [--max-episode-length MAX_EPISODE_LENGTH]
               [--no-shared NO_SHARED] [--off-tile-coef OFF_TILE_COEF]
               [--checkpoint-interval CHECKPOINT_INTERVAL]

A3C

optional arguments:
  -h, --help            show this help message and exit
  --lr LR               learning rate (default: 0.00001)
  --gamma GAMMA         discount factor for rewards (default: 0.99)
  --tau TAU             parameter for GAE (default: 1.00)
  --entropy-coef ENTROPY_COEF
                        entropy term coefficient (default: 0.01)
  --value-loss-coef VALUE_LOSS_COEF
                        value loss coefficient (default: 0.5)
  --max-grad-norm MAX_GRAD_NORM
                        value loss coefficient (default: 25)
  --seed SEED           random seed (default: 1)
  --num-processes NUM_PROCESSES
                        how many training processes to use (default: 16)
  --num-steps NUM_STEPS
                        number of forward steps in A3C (default: 30)
  --max-episode-length MAX_EPISODE_LENGTH
                        maximum length of an episode (default: 500
  --no-shared NO_SHARED
                        use an optimizer without shared momentum.
  --off-tile-coef OFF_TILE_COEF
                        weight to penalize bad movement
  --checkpoint-interval CHECKPOINT_INTERVAL
                        interval to save model
```

NUM_PROCESSES threads are used for training and 1 thread is used for evaluation. Model
is saved at reinforce_trained.mdl

### Running Actor Critic Bot on Online Server

To test the policy network on private online bot server on generals.io use:
```
usage: reinforce_online_client.py [-h] [--user_id USER_ID]
                                  [--username USERNAME] [--game_id GAME_ID]
                                  [--model_path MODEL_PATH]

Reinforcement Trained Bot Player

optional arguments:
  -h, --help            show this help message and exit
  --user_id USER_ID     user_id for bot
  --username USERNAME   username for bot
  --game_id GAME_ID     id for the game
  --model_path MODEL_PATH
                        path of a3c trained model
```

To quickly demo policy bot, clone the repo and run 
```
python reinforce_online_client.py
```
and then go to the URL [here](http://bot.generals.io/games/viz0)

