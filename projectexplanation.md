# Adversarial Attack & Defense using CNN and FGSM — Interview Explanation

# 1. Project Introduction

## Tell me about your project.

My project focuses on adversarial attacks and defenses in deep learning models.

I implemented a Convolutional Neural Network (CNN) using PyTorch on the CIFAR-10 dataset and analyzed how small adversarial perturbations generated using FGSM can fool image classification models.

I also explored defense techniques like adversarial training to improve robustness against attacks.

---

# 2. Problem Statement

## What problem were you solving?

Deep learning models achieve high accuracy, but they are vulnerable to adversarial attacks.

Even tiny perturbations added to images, which humans cannot notice, can cause neural networks to completely misclassify the image.

My project studies this vulnerability and explores methods to improve model robustness.

---

# 3. What is an Adversarial Attack?

An adversarial attack is a technique where carefully crafted perturbations are added to an input image to intentionally fool a neural network.

Example:

A CNN correctly classifying a cat image can misclassify it as a dog after adding very small perturbations.

---

# 4. Why did you choose FGSM?

I selected FGSM because it is one of the foundational gradient-based adversarial attacks.

It is computationally efficient because it requires only one backpropagation step and clearly demonstrates how gradients can be exploited to attack neural networks.

---

# 5. Dataset Used

## CIFAR-10 Dataset

I used the CIFAR-10 dataset which contains:

- 60,000 RGB images
- 10 classes
- Image size: 32 × 32 × 3

### Classes

- Airplane
- Automobile
- Bird
- Cat
- Deer
- Dog
- Frog
- Horse
- Ship
- Truck

### Dataset Split

- Training Images: 50,000
- Testing Images: 10,000

---

# 6. CNN Architecture

I implemented a Convolutional Neural Network consisting of:

- Convolution Layers
- ReLU Activation
- Max Pooling Layers
- Fully Connected Layers

---

# CNN Workflow

```text
Input Image
     ↓
Convolution Layer
     ↓
ReLU Activation
     ↓
Max Pooling
     ↓
Convolution Layer
     ↓
ReLU Activation
     ↓
Max Pooling
     ↓
Flatten
     ↓


     # 7. Explain CNN Components

## Convolution Layer

Convolution layers extract spatial features such as:

- edges
- textures
- patterns
- shapes

using learnable filters.

---

## ReLU Activation

ReLU introduces non-linearity into the network.

Formula:

```math
f(x) = max(0, x)
```

Without activation functions, the network behaves like a linear model and cannot learn complex patterns.

---

## Max Pooling

Max pooling reduces spatial dimensions while preserving important features.

Benefits:

- reduces computation
- reduces overfitting
- improves efficiency

---

## Fully Connected Layer

Fully connected layers perform final classification using extracted features.

---

# 8. Training Process

During training, images pass through the CNN in the forward pass to generate predictions.

The CrossEntropyLoss function computes prediction error, and backpropagation calculates gradients using the chain rule.

The optimizer updates network weights to minimize loss over multiple epochs.

---

# 9. Training Loop

```python
for images, labels in trainloader:

    optimizer.zero_grad()

    outputs = model(images)

    loss = criterion(outputs, labels)

    loss.backward()

    optimizer.step()
```

---

# 10. Explain Each Training Step

## optimizer.zero_grad()

Clears old accumulated gradients before computing new gradients.

PyTorch accumulates gradients by default.

---

## outputs = model(images)

Performs forward propagation and generates prediction logits.

---

## loss = criterion(outputs, labels)

Calculates prediction error between predicted labels and actual labels.

---

## loss.backward()

Performs backpropagation and computes gradients of loss with respect to model parameters.

---

## optimizer.step()

Updates weights using computed gradients.

---

# 11. FGSM Attack

FGSM stands for Fast Gradient Sign Method.

It is a gradient-based adversarial attack used to generate adversarial examples.

---

# FGSM Formula

```math
x_{adv} = x + \epsilon \cdot sign(\nabla_x J(\theta, x, y))
```

Where:

- x = original image
- x_adv = adversarial image
- ε = perturbation strength
- J = loss function
- ∇x = gradient with respect to image

---

# 12. How FGSM Works

FGSM computes gradients of the loss with respect to the input image instead of model weights.

The sign of the gradient indicates the direction in which pixel values should change to maximize model loss.

A small perturbation scaled by epsilon is added to the image to create an adversarial example.

---

# 13. Why use requires_grad=True?

Normally gradients are computed with respect to model weights.

For FGSM, gradients must also be computed with respect to input images.

So gradient tracking is enabled on the image tensor using:

```python
image.requires_grad = True
```

---

# 14. Why use sign() in FGSM?

Using only the sign of the gradient stabilizes perturbation magnitude and prevents excessively large updates in certain pixels.

The sign function converts gradients into:

- +1
- -1

which gives only direction information.

---

# 15. What is Epsilon?

Epsilon controls attack strength.

- Small epsilon:
  - less visible perturbation
  - weaker attack

- Large epsilon:
  - stronger attack
  - more visible noise

---

# 16. Defense Mechanism

I implemented adversarial training as a defense mechanism.

Adversarial examples generated using FGSM were included during training.

This improved model robustness against adversarial attacks.

---

# 17. Why Adversarial Training Works

By training on adversarial examples, the model learns more robust decision boundaries and becomes less sensitive to small perturbations.

---

# 18. Results

The normal CNN achieved high accuracy on clean CIFAR-10 images.

However, under FGSM attack, accuracy dropped significantly, demonstrating model vulnerability.

After adversarial training, robustness improved substantially while maintaining acceptable clean-image accuracy.

---

# 19. Challenges Faced

- Understanding gradient flow during adversarial example generation
- Computing gradients with respect to input images
- Balancing robustness and clean-image accuracy
- Selecting appropriate epsilon values

---

# 20. What I Learned

This project helped me understand:

- CNN internals
- Backpropagation
- PyTorch autograd
- Gradient-based optimization
- Adversarial attacks
- Model robustness
- Deep learning security

---

# 21. Important Interview Questions

## Why does FGSM work?

Neural networks are highly sensitive to small perturbations in high-dimensional input space.

Small coordinated pixel changes can shift the input across decision boundaries.

---

## Why use gradients in FGSM?

Gradients indicate how input pixels affect model loss, allowing us to identify the optimal direction for perturbation.

---

## Why CNN for image classification?

CNNs efficiently capture spatial patterns and local image features using convolution operations.

---

## Why use CrossEntropyLoss?

CrossEntropyLoss is suitable for multi-class classification and combines softmax activation with negative log likelihood.

---

## What happens if epsilon becomes very large?

Attack success rate increases, but perturbations become visually noticeable and image quality degrades.

---

# 22. Final Project Summary

My project focused on adversarial attacks and defenses in deep learning models using PyTorch.

I implemented a CNN model on the CIFAR-10 dataset and analyzed its vulnerability against FGSM adversarial attacks.

FGSM uses gradients of the loss with respect to the input image to generate perturbations that maximize prediction error.

I also implemented adversarial training to improve robustness against attacks and evaluated model performance under different epsilon values.

Through this project, I gained practical understanding of CNNs, backpropagation, PyTorch autograd, optimization, and adversarial robustness in deep learning.
Fully Connected Layer
     ↓
Prediction
