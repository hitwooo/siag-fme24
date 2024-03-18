# README for Finatics Team: SIAG/FME Code Quest 2023 - Code Description

## Overview

This README outlines the functionalities and key components of the Python code developed by the Finatics team for the SIAG/FME Code Quest 2023. The code is structured to simulate and optimize strategies in a Constant Product Market Maker (CPMM) model within a Decentralized Finance (DeFi) context.

## Key Components of the Code

### Class AMM 
This class forms the core of the CPMM model. It handles the initialization of token reserves and pool fees, and defines several key operations:

- `swap_x_to_y()` and `swap_y_to_x()`: Facilitate the swapping of X, Y tokens across all pools.
- `mint()` and `burn()`: Allow for the minting or burning of liquidity from pools.
- `swap_and_mint()` and `burn_and_swap()`: Combined operations for swapping and adjusting liquidity in a single transaction.
- `simulate()`: Simulates the market dynamics over time, considering various transaction types and volumes.
- `feed_forward()`: A function to replicate `simulate()` using pre-generated market transactions. This is done to save code runtime - we do not have to simulate the transactions at each step of gradient descent. We made the simplification of ignoring the effect the initial `theta` has on the simulated transactions (through `muy` and `v_t`).


### Additional Functions
The code also includes a set of predefined parameters and utility functions for testing new strategies and visualizing their performance.

- `strat_test()`: This function tests a given investment strategy (defined by `theta`) against the simulation environment. It evaluates the strategy's performance in terms of mean return, standard deviation, VaR, and CVaR, and can optionally produce graphical output for analysis.
- `strat_plot()`: A utility function for visualizing the performance metrics of different strategies. It can show either raw or scaled values of metrics like CVaR, mean return, and standard deviation, providing a visual comparison between strategies.


### Stochastic Gradient Descent (SGD) for CVaR
This section is dedicated to implementing an SGD algorithm for optimizing investment strategies based on Conditional Value at Risk (CVaR). Key functions include:

- `calc_cvar()`: Computes the CVaR at a given `theta`.
- `calc_grad()`: Determines the gradient of the CVaR at a given `theta`.
- `update_theta()`: Adjusts the `theta` based on the gradient generated from `calc_grad()`.
- `project_onto_simplex()`: Done after `update_theta()` to ensure that `theta` remain within feasible bounds.
- `SSGD()`: Stands for Stochastic Steepest Gradient Descent. This function iteratively updates `theta` by calculating gradients using `stoc_grad()` and adjusting the strategy with `update_theta()`. It tracks the CVaR and other metrics throughout the process, providing insights into the strategy's effectiveness.

### Generalization on Different Seeds
The final part of the code focuses on generalizing the strategy to various market conditions and optimizing the SGD process. This includes generating multiple simulation scenarios and employing minibatch processing for more robust gradient estimation. Market transactions (`z`) are pre-generated using 500 arbitrary seed and stored in `list_vt` and `list_directiont`, after which 5 are randomly picked in each iteration of gradient descent and values of CVaR are calculated on them using `feed_forward()`. Most of the functions in SGD are also employed in this section, along with the additional following:

- `calc_main_seed()`: Specifically designed to track the CVaR using the main simulation seed. This function is crucial for understanding the behavior of a strategy under a consistent set of market conditions.
- `calc_cvar_at_seed()`: This function replaces `calc_cvar()` and calculates the CVaR and the constraint probability for a given `theta` **and `z`**.
- `MSSGD()`:  Stands for *Mini-batch* Stochastic Steepest Gradient Descent. Computes the stochastic gradient of the CVaR using a minibatch of different simulation scenarios. This function replaces `SSGD()`, allowing the algorithm to consider a wider range of market conditions in its optimization process.



## Conclusion

This Python code marks the Finatics team's entry into the complex world of Decentralized Finance (DeFi) via the SIAG/FME Code Quest 2023. As newcomers in financial engineering and mathematical modeling, we have taken on the challenge of developing strategies within a Constant Product Market Maker (CPMM) framework. This initial effort reflects our dedication to learning and innovation in a rapidly evolving domain. Recognizing the importance of collaboration in this competitive landscape, we welcome constructive feedback and insights to enhance our strategies and understanding. The Finatics team is open to engaging in discussions and sharing knowledge about our code and broader DeFi strategies. 

For additional information or queries about our code, please feel free to contact the Finatics team.


