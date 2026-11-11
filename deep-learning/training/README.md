# Training

How networks learn: the training loop, backpropagation and the techniques that make deep networks trainable.

This section is part of the knowledge area [Deep learning](../README.md). Read the notes in the order below.

## Notes

- [Training a neural network](training-a-neural-network.md): A neural network learns by repeating one loop: compute weighted sums and activations layer by layer to produce a prediction `ŷ`, measure the error against the true value `y` with a loss function, send the error backward through the network with backpropagation to get the gradient of every weight, and update the weights with gradient descent.
- [Backpropagation](backpropagation.md): Backpropagation is the algorithm that computes, for every weight of a neural network, how much that weight contributed to the error.

## Planned topics

- Optimizers: momentum, RMSProp and Adam
- Learning rate schedules
- Batch and layer normalization
- Dropout and regularization for deep networks
- Vanishing and exploding gradients
