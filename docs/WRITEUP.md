### Deep Learning Follow Me Project

## Introduction
The project was aimed at implementing a Drone Simulation to follow a moving entity 
in an environment with multiple distractions. A fully convolutional neural network with 
multiple hidden layers was used to used to accomplish this.

## Data Collection
Data collection was performed by using the simulator. Specifically, two cases were covered.
First, the drone has two patrol points with the `hero` following a zigzag path underneath, 
with other spawns(crowd figures) present.
Second, the drone follows the target with a complicated path traced, also with crowd figures.

[Figure1]: ./misc/data_collection_1.png
[Figure2]: ./misc/data_collection_2.png
[NetworkDiagram]: ./misc/network_diagram.png

# Figure 1
![alt_text][Figure1]
# Figure 2
![alt_text][Figure2]

## Network Architecture
The convolutional networks I have encountered in my DeepLearning Nanodegree, 
used for image recognition are combinations of Convolutional Neural Networks with Fully Connected Layers in the end with softmax activations
to identify the probability of a particular target in the target set.

In this case fully-connected layers will not work as they remove spatial information
of the object and maintain only object identification attributes.
Hence, we need to use a fully-convolutional network, where every layer is a convolutional layer.

# Network Diagram:
![alt_text][NetworkDiagram]

The fully convolutional network implemented here has two primary components,
the encoding stage and the decoding stage, which are connected by a 1x1 convolutional layer.
The encoding stage works to extract the features which are useful for segmentation of the
input image. It works in typical fashion of neural networks, with the initial layers trying
to identify simple patterns(lines, curves) and further layers learning more complex shapes
and patterns. 

The 1x1 convolutional layer implements a fully-connected functional logic
while maintaining spatial information.
The decoder works to via multiple layers to upscale the encoder output to the
same dimensions of the input image. The skip connections connect the non-adjacent layers,
which allows the network to retain some information which otherwise would have been lost.
The last layer is a convolutional layer with a softmax activation which makes a pixel-level
segmentation between the classes.


## Training
The training was performed on by launching an p2.xlarge AWS instance in the 
US-West Oregon region. I already had limits for GPU instances in this region from 
the time I was completing my Deep Learning NanoDegree. The experience and the thrill
in this project for me was comparable to implementing DCGAN to generate images.

Initially I ran the training with only 10 epochs to check if everything was setup correctly
and the network appeared to be converging. 
The training time was a surprise as it took only about 1.3 hours to complete, much lesser
than for other neural networks I had trained with similar data.

The graphs for the training can be seen in the Ipython notebooks. 

## Experiments and Results
Carrying over my previous knowledge from training neural networks, I started with a
simple network with 2 encoder and 2 decoder layers to validate the idea. 
By choosing smaller networks, we can ensure overall complexity is kept low,
keeping the training time short while developing and also avoid over-fitting.
Complexity and depth of a neural network can be increased if network is under-fitting,
later on in the experimentation.

The batch chosen was 32 after trying out 128 and 64. 128 and 64 were causing longer
training time per epoch than 32, hence I finalized on 32. I started
with 10 epochs to begin with to test out the working of the network. After 10 epochs, the network
had not reached loss convergence, and hence I further increased the epochs
to 100 and allowed the network training to run for 1 hour.

The initial learning rate chosen was 0.01, based on the notes that in case of batch
normalization, higher learning rates are acceptable. 

The higher the validation steps and the steps per epoch, the higher was the loss.

The IOU score on the attempt with learning rate 0.01 was 0.34. This data was tried
out with simulator. The quad was able to track the target for some time, but lost her
when sharp turns were taken by the target. This proof of concept, though not complete,
allowed confidence for future runs.

The network works satisfactorily for the validation tests from the sample evaluation data.
The below are the confusion matrices for the scores:

1. When the quad is following the target:
The confusion matrix is:
| C | F |
| ---| --- |
| 3 | 0   |
| 0 | 539 | 

2. When quad is on patrol and target is not visibile
| C | F |
| ---| --- |
|  91 | 179 |
|  0 | 0  |

3. Scores for target detection from away
| C | F |
| ---| --- |
| 15| 6|
| 97 | 204 |

## Conclusion
The drone is able to track and follow the `hero` target in simulation even when the 
target is making sharp turns. The network can be similarly trained for other
targets like vehicles, animals by training it against those images. In the
future we would like to have a configurable drone(and trained models), which on selection of any type of
target should be able it down and follow it. 

The IOU scores, I think can be improved if a few more convolutional layers are added in,
but a close watch should be kept on over-fitting. An increased amount of training data
is required to reduce the amount of false-positives the model gets due to the other people
in the scene.

This was a very involved project and much better planned out than the previous one. The 
problem description and the corresponding examples can be more detailed to prevent ambiguity.
