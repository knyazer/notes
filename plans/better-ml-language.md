# A better language designed for ML
If your language compiles into jaxprs: you can interoperate with JAX. Awesome!

### Syntax examples
Here are some thoughts about how the syntax "ought" to look like.

#### Model definition

Doing ops like attention, linear layer, etc etc should be as easy as possible.

Any model definition has a few stages:
1. Defintion
2. Initialization
3. Trainability of parameters (ie convolutions are full-rank linear layer, with some frozen weights)

Making an MLP:
```


```

#### Optimizer definition

```
m_t = m_t * \beta + m_{t + 1} * (1 - \beta)
```
