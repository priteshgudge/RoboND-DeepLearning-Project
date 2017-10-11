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

