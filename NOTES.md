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
    