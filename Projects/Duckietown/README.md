# Overview

![aido-process](/Projects/Duckietown/screenshots/aido-process.png)

1. Reproducibility: Standardized hardware, enviornments, software and interfaces.
2. Accessibility: Easy to get started

- Low-cost platform for autonomy education and research
- Robotics and machine learning for students of all ages
- Teaches concepts in perception, planning, and control with miniature autonomous vehicles in the classroom

![dt-elements](/Projects/Duckietown/screenshots/dt-elements.png)

Scientific Questions:
1. Sim-to-real: What is the right way to leverage simulation, data logs, and the real robot to build capable agents most effectively?
2. Fair comparision: Compare a relatively wide range of different approaches in a standardized way.

Read Paper for more info:
- [The AI Driving Olympics at NeurIPS 2018](/Projects/Duckietown/papers/AIDO-Paper.pdf)

------------------------------------------------------

### Current Implementation in Simulator:
In [ws-template](/Projects/Duckietown/ws-template) folder the following code template is provided.

#### Fake Navigation
- It contains duckietown simulation, including a city, bots, roads and traffic lights.
- The *fake_navigation* package used to make a bots navigate around the city autonomously.
- There are a set of predefined points towards which the robot will move.
- Useful for Capturing images in an automatic way, for training.
- Let the robot move by itself while testing if the line detector is working correctly.
- To launch other duckbots that move around in order to interact with yours.
![fake_nav](/Projects/Duckietown/screenshots/db_fake_nav.gif)

#### Lane Detection: Basic Implementation
- We want the DuckieBot to be able to distinguish the yellow lane separation line from all others so we will write a very simple script to detect the particular range of yellow that we are interested in.

- Then we will calculate its offset from the center and use this for some function in our RL algorithm as Task Enviornment.
![line_detect](/Projects/Duckietown/screenshots/db_line_detect2.gif)

#### OpenAI ROS DeepQlearning
![line_follow_openai](/Projects/Duckietown/screenshots/openai_ros_pipeline.png)
In order to train the robots with the *openai_ros* [package](http://wiki.ros.org/openai_ros), we will need to create 3 parts of code:
1. The **TaskEnvironment**: that is the class that defines the learning conditions of the robot.
    1. How to get an **observation** for the Reinforcement Learning algorithm
    2. How to execute an **action**
    3. How to compute the **reward**
    4. How to detect when the task is **done**
2. The **RobotEnvironment**: that is the class that obtains the sensor data from the simulation and generates the proper ROS commands to make the robot move.
3. The **training script**: that is the script that contains the learning algorithm, and that orchestrates the calls to the TaskEnvironment in order to make the algorthm learn.
![line_follow_openai](/Projects/Duckietown/screenshots/db_openai.gif)

### Tasks:
1. Using EduBot-2WD with this enviornment.
2. Implementation of Duckietown stack on Jetson Nano and RPiv3.
3. Implementing above tasks with official stack.
4. Qualifying for AI-DO and Robotarium Submission.

------------------------------------------------------

### AI-DO I & II Challenges
1. Lane following (LF)
2. Lane following + Obstacles (LFV)
3. Lane following + Obstacles + intersections (LFVI)

#### Rules:
- Agent runs for 30s or until a major infraction
- Metric one: Distance travelled (# of tiles)
- Metric two: Survival time (in seconds)

Once agent performs well in simulation

You can submit for robotarium challenge where it runs
on actual robot in one of the Robotariums. Robotarium contains
watchtowers which helps localization. Also each bot has
QR Code (April tags) used by localization system for
accurate localization.

To build your agent you have simulator `gym-duckietown`.
Simulator comes with API such as doman randomization for
robust training.

You also have video logs `logs.duckietown.org` for your
training. They dont have annotations so you have to do it.

------------------------------------------------------

### 4 Templates
1. ROS based infrastructure
2. Tensorflow based infrastructure
3. PyTorch based infrastructure
4. No Infrastructure


### 4 Baselines
1. The Duckietown Codebase
    - No ML, it uses classical robotics principles like CV, 
    canny edge, color thresholding, morphlogical operations,
    histogram filter that approximates the bayes filter,
    PID Controller to keep bot on lane.

2. Imitation Learning from Logs
    - ML based, learning Input/Output mapping between image 
    space to steering commands. (shown by NVIDIA in 2016)

3. Reinforcement Learning
    - ML based, This uses simulator and reward structure,
    OpenAI gym to train RL Agent.

4. Imitation Learning in Sim
    - It uses expert trajectories that are generated from 
    simulator. Best way to train robust models.

### Cloud Evaluations
You can submit any number of evalutations in validation with 
output and test submissions output will not be provided. 
Make sure to do local evaluation before submitting so you 
are sure its functional before making it to cloud.

------------------------------------------------------

## How to get started ?
1. Build and test your agent locally
    - using `dts_challenges_evaluate`
    - Once happy with the performance score of your agent
    go ahead to step2.

2. Submit to a challenge
    - This packages up your agent and send it to the cloud
    to evaluate against the server on the cloud.
    - Use `dts_challenges_submit`
    - You get Leaderboard, Visual Output and Debugging Data

3. Run your agent on a robot
    - Use `dts_duckiebot_evaluate`
    - This will evaluate on your duckiebot locally on the same
    wifi network.
    - If you don't have a robot. You can use `dts_challenges_submit`
    to submit to real challenge that will evaluate in robotarium.

------------------------------------------------------

## Resources
1. [Full Documentation](https://docs.duckietown.org/daffy/)
2. [AIDO](https://docs.duckietown.org/daffy/AIDO/out/index.html)
3. [AIDO Paper](/Projects/Duckietown/papers/AIDO-Paper.pdf)
4. [nuScene Challenge and Dataset](https://www.nuscenes.org/)
5. [DeepRacer Amazon challenge](https://aws.amazon.com/deepracer/)
