# Temporal gradient discounting

## Summary: 150 words

## Summary: 1500 words

## Content: 15000 words /draft/

### Abstract
_broad context_:


_narrower context_:

Differentiable simulators show promise improved sample efficiency of reinforcement learning. While standard reinforcement learning methods use only the loss values, the differentiable simulators based methods use the gradient information.


_question_:

The 

_result_:

We rediscover a bias-variance tradeoff method 

_direct implications_:

_broader significance_:

### Introduction

In reinforcement learning time discounting bounds the updates: the future reward is worth exponentially less than the current. This leads to well-defined gradients, well-defined updates and, overall, allows for a large family of reinforcement learning algorithms to work.

Differentiable simulators is an umbrella term for automatic differentiation through a physics simulator. The underlying idea is quite simple: autodiff engines allow to differentiate through any program, thus one can differentiate through a simulation step.

A commonly discussed issue with differentiable simulators is the presence of discontinuities in the system dynamics.

### Method













