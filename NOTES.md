# Lesson 3

## Recurrent Neural Networks

Rnns are networks with hidden layers that `carry information` from the previous hidden layers

They can be represented as one block

[!RNN](/resources/RNN.png)

For Natural language processing:

- Input layer:
        
        The current word's embedding vector (representing the input at time step t)

                             +

        The previous hidden state vector (the memory from time step t-1)

- Hidden Layer

    Multiplying these vectors by their respective weight matrices ($\mathbf{U}$ (input weights) and $\mathbf{W}$ (previous layers weights)), summing them, and applying a non-linear activation function to compute the new hidden state.

- Output Layer

    The new predition, probability distribution over the vocabulary. We apply a softmax to transform the predictions into the desired distribution, we want them to be from 0 to 1.

> Advantages:
>
> - Can process any length rather than fixed as most Newral Networks (we can add words)
> - Model size remains the same for longer inputs, the weights remain the same in size 
> - Same weights applied on every time step

> Disadvantages:
> - Slow Computation
> - Serialized Processing, cannot add parallelization

### Kinds of RNNs

The different kinds of RNNs are based on the `ammount` of `input` and `output`

- One to One: Not an RNN, eg. a basic CNN image classification one input, one output
- One to many: Eg and image to a sentence
- Many to one: Eg detecting a sentence nature (good or bad)
- Many to Many:
    
    These are again split as to how the iput is processed

    - read all throught time and then output -> Language Translation
    - read and output, until through time everything is read -> Word tagging 
    
### Back Propagation Through time

1. Forward Pass and Total LossUnrolling: An RNN processes a sequence chronologically step-by-step, generating a prediction and a local loss ($J^{(t)}(\theta)$) at each time step.   Total Loss ($J(\theta)$): The overall loss is the average of all individual time-step losses across the sequence length $T$:
   $$J(\theta) = \frac{1}{T} \sum_{t=1}^{T} J^{(t)}(\theta)$$

2. Backpropagation Through Time (BPTT)

    $$\frac{\partial J}{\partial W_h} = \frac{1}{T} \sum_{t=1}^{T} \sum_{k=1}^{t} \frac{\partial J^{(t)}}{\partial h^{(t)}} \left( \prod_{j=k+1}^{t} \frac{\partial h^{(j)}}{\partial h^{(j-1)}} \right) \frac{\partial h^{(k)}}{\partial W_h}$$

    1. The Outer Sum ($\sum_{t=1}^{T}$): We loop through every time step $t$ where a loss $J^{(t)}$ is calculated because the total loss averages all of them.
    2. The Inner Sum ($\sum_{k=1}^{t}$): An error at time $t$ flows backward to all preceding time steps $k$ ($1$ up to $t$).
    3. The Product Chain ($\prod_{j=k+1}^{t}$): This is the bridge across time. To get from the future error at step $t$ back to step $k$, the gradient must multiply the hidden-to-hidden transition derivatives ($\frac{\partial h^{(j)}}{\partial h^{(j-1)}}$) across every intermediate step.
    3. The Local Derivative ($\frac{\partial h^{(k)}}{\partial W_h}$): At the target step $k$, this connects the hidden state back to the weight $W_h$ we want to update.
<br/>
<br/>
> Vanishing Gradients:
>
>  Because BPTT relies on multiplying these hidden-state derivatives ($\prod$) over and over across long time steps, it creates an exponential compounding effect.   

> Exploding Gradients
>
>If the derivatives are greater than 1, the gradient grows infinitely, destabilizing training.