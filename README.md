# COMP47590 Advanced Machine Learning
## Project 2: Going the Distance
### How to Run
Download zip file, unzip and open "COMP47590 Project 2 Going The Distance.ipynb" file and run

### Overview
This assignment involves training a PPO-based agent to control a car in the `CarRacing-v2` environment using the Stable-Baselines3 library. The environment provides a 96x96 image as the state, and the agent can choose from five discrete actions: left, right, brake, accelerate, and none. The reward structure is such that the agent receives `-0.1` every frame and `+1000/N` for each visited track tile, where `N` is the total number of tiles. Completing the track in fewer frames thus leads to higher rewards.

### Approach
1. **Single Image Agent**:  
   The first model uses a single grayscale image (64x64) as input. PPO hyperparameters recommended by the assignment were used (e.g., `learning_rate=3e-5, n_steps=512, ent_coef=0.001`), and `use_sde=True` for state-dependent exploration in continuous action spaces. Although this environment is presented with discrete actions, using SDE might not yield the best results due to the discrete nature of the action space.

2. **Stacked Image Agent**:  
   The second model processes a stack of 4 consecutive frames. This helps the agent perceive motion dynamics rather than relying on a single static frame. The same PPO hyperparameters were applied.

### Results and Observations
- **Common Issue - Spinning (Doughnut Behavior)**:  
  Both agents suffered from a problematic behavior where the car would spin in place (the so-called "doughnut" phenomenon). This likely indicates the agent falling into a local minimum and not exploring enough to escape suboptimal states.
  
- **Performance Comparison**:  
  Despite this issue, the stacked-image agent performed better overall than the single-image agent. The highest episode mean rewards were around `109.16` for the single-image agent and `138.10` for the stacked-image agent, indicating that additional temporal context helped. However, both agents’ evaluation mean rewards remained negative in some evaluations, suggesting difficulties in stabilizing policy learning.

- **Training Difficulty**:  
  The training process was time-consuming, especially with rendering, which made iterative experimentation and parameter tuning challenging. Further refinements such as improved reward shaping, action space adjustments (e.g., removing the "no action" command), or using ON DUPLICATE KEY UPDATE strategies for handling API keys would be necessary for better performance.

### Reflection
Stacking frames provided a richer context and led to better performance than using a single frame. However, the persistent spinning behavior and the challenge of tuning hyperparameters highlight that further environment-specific adjustments are needed. This might include adjusting the action space, exploring different network architectures, or modifying the reward function to penalize staying still or spinning continuously.

The results emphasize that while PPO and stable hyperparameters offer a solid baseline, domain-specific tweaks and iterative experimentation are crucial to achieve robust performance in complex tasks like CarRacing.

### **The new version(CarRacing-v3) has been released and it reduces the probability of spinning behavbior with different parameters
