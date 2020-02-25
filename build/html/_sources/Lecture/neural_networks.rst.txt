================================
Neural Networks
================================


----------------
Backpropagation
----------------

Backpropagation through time (BPTT)
------------------------------------

Reading material: https://machinelearningmastery.com/gentle-introduction-backpropagation-time/

BPTT is the application of backpropagation training algorithm to recurrent neural network. It works by unrolling all input timesteps. Each timestep has one input timestep, one copy of the network and one output. Errors are calculated for each timestep and accumulated to the end. The network is rolled back up and the weights are updated.

BPTT is computationally expensive when the number of timesteps is large. It also suffers from the weight vanish/explode problem.

Truncated backpropagation through time (TBPTT)
----------------------------------------------

TBPTT is a modified version of BPTT to solve the problems mentioned above. It is a more practical method.

TBPTT processes the sequence one timestep at a time. At every k1 timesteps, it runs BPTT for k2 timesteps (parameter updates). The hidden states have been exposed to many timesteps. so may contain useful information about the far past.

We can summarize the algorithm as follows:

1. Present a sequence of k1 timesteps of input and output pairs to the network.
2. Unroll the network then calculate and accumulate errors across k2 timesteps.
3. Roll-up the network and update weights.
4. Repeat

The TBPTT algorithm requires the consideration of two parameters:

- k1: The number of forward-pass timesteps between updates. Generally, this influences how slow or fast training will be, given how often weight updates are performed.
- k2: The number of timesteps to which to apply BPTT. Generally, it should be large enough to capture the temporal structure in the problem for the network to learn. Too large a value results in vanishing gradients.

---------------
Neuroevolution
---------------

Reading material: http://www.scholarpedia.org/article/Neuroevolution

Neuroevolution is a biological-inspired method for creating artificial intelligence. It is motivated by the evolution of biological nervous systems. It can be seen as means to investigate how intelligence evolved in nature, and as a practical method for engineering artificial neural networks.

Neuroevolution has the following advantages:

- Most common artificial neural network (ANN) learning algorithm is supervised learning and needs a labeled corpus of input-output pairs. Neuroevolution does not need such corpus and can learn from sparse feedback. 

- Neuroevolution generalizes to a wide range of network architectures and neural models. In addition to modifying the strengths of neural connections (weights), it can optimize parameters like:

    - structure of the network (add more neurons or connections)
    - type of computation performed by individual neurons
    - learning rules that modify the network during evaluation

Applications of neuroevolution includes:

- reinforcement learning (board games, video games)
- evolutionary robotics (controlling mobile robots)
- artificial life

Algorithm
---------------

Goal: evolve a population of genetic encoding of neural networks in order to find a network that solves the given task.

The generate-and-test loop:

1. choose one encoding in the population (genotype) and decode it into the corresponding neural network (phenotype)
2. employ the network in the task and measure its performance. Obtain a fitness value.
3. after all members are evaluated, genetic operators are used to create the next generation of the population

    - encodings with the highest fitness are mutated and crossed over with each other
    - the resulting offspring replaces the genotypes with the lowest fitness

4. continue until a network with a sufficiently high fitness is found

Advanced Neuroevolution
------------------------

There are some problems with the basic neuroevolution implementation:

- the population converges and loses diversity
- competing conventions: different, incompatible encodings for the same solution
- too many parameters to be optimized simultaneously

To solve these problems, there are some advanced neuroevolution techniques:

**Evolving Partial Networks**: Enforced Sub-Populations (ESP).

- each neuron is in a separate subpopulation and evolves independently
- the populations learn compatible subtasks
- this approach encourages diversity because different kinds of neurons are required to get a good network
- this approach discourages competing conventions because neurons are optimized for compatible roles
- large search space is divided into subtasks

**Evolutionary Strategies**: 

- small populations with no crossover
- use intelligent mutations

**Evolving Network Structure**: Neuroevolution of Augmenting Topologies (NEAT)

- based on complexification of networks and behavior

**Indirect Encodings**: Cellular Encoding (CE)

- instructions for constructing the network evolved


--------------------
Self-Organizing Map
--------------------

Reading materials: 

    - https://towardsdatascience.com/self-organizing-maps-ff5853a118d4

A self-organizing map (SOM) is a type of artificial neural network that is trained using unsupervised learning to produce a low-dimensional, discretized representation of the input space. It can be used as a method of dimensionality reduction.

What's different about SOM?

- competitive learning instead of error-correction learning (backpropagation)
- preserve the topological properties of the input space 

SOM Algorithm

1. Initialize weight vectors of each note
2. Choose a vector randomly from the training data
3. Go through every nodes in the map to find the one with the weights most like the selected vector. This winning node is called the Best Matching Unit (BMU)
4. Find the neighbors of BMU. The amount of neighbors decreases over time
5. The wining weight is rewarded to become more like the sample vector. The neighbors are also rewarded. The closer a node is to the BMU, the more its weights get altered. The farther away the neighbor is from the BMU, the less it learns.
6. Repeat for N iterations

Measurements such like the Euclidean distance are used to find the BMU.

As the results, the weight vectors become approximations of input vectors and neighboring weight vectors become more parallel.

Why use SOM?

- reduce high dimensional inputs to 2 dimensional space to make the similarities evident -> reduce the problem to a 2D classification.


Cons of SOM:

- It does not build a generative model. We can not generate a data similar to the training dataset.
- Not working well for categorical data
- The time for preparing model is slow, hard to train against slowly evolving data.

Learning Vector Quantization (LVQ)
-----------------------------------
Reading materials:

    - https://machinelearningmastery.com/learning-vector-quantization-for-machine-learning/
    - https://www.tutorialspoint.com/artificial_neural_network/artificial_neural_network_learning_vector_quantization.htm

SOM is a visualization tool, but not good at classification. LVQ is a supervised classification algorithm that can fine-tune the boundaries. Since it is a supervised method, a set of codebook vectors are feed in as training data.

    - A codebook vector is a list of numbers that have the same input and output attributes as your training data. For example, if your problem is a binary classification with classes 0 and 1, and the inputs width, length height, then a codebook vector would be comprised of all four attributes: width, length, height and class.
    - In the language of neural networks, each codebook vector may be called a neuron, each attribute on a codebook vector is called a weight and the collection of codebook vectors is called a network.

LVQ Algorithm
--------------

1. Initialize reference vectors

    - from the training vectors, take the first m (number of clusters) training vectors and use them as weight vectors. Use the remaining vectors for training.
    - assign the initial weight and classification randomly
    - apply K-means clustering method

2. For every training input vector x

    - find the winning unit J where the Euclidean Distance from x to j is minimum
    - update the weight of the winning unit. Updating rules depend on whether the classification is correct or not (with a learning rate).
    - reduce the learning rate

3. Repeat until stopping condition is met (reach the maximum number of epochs or learning rate reduced to zero).

Modifications
--------------

**LVQ2**. Only make correction when input falls in wrong side of the boundary. This algorithm will give a finer tuning of the boundary.

**LVQ3** Make correction whenever input falls within a window. This algorithm is smoother and faster.


---------------------------
Computational Neuroscience
---------------------------

Reading material: http://nn.cs.utexas.edu/downloads/papers/miikkulainen.iconip98.pdf

This paper summarizes work to date on an artificial neural network model, RF-LISSOM (Receptive-Field Laterally Interconnected Syn- ergetically Self-Organizing Map), that shows how the neurons’ receptive fields, 2-D columnar organization, and lateral connectivity can be learned from the input based on Hebbian self-organization. These same mechanisms explain how the cortex can remain plastic and adapt to changes in input and to internal lesions. The model suggests that the resulting organization is formed to represent and process visual input efficiently, forming a redundancy-reduced sparse coding of the visual input. The self-organized model can then be used to model various low-level visual phenomena, including tilt aftereffects and segmentation and binding.

.. topic:: Hebbian Learning

    Hebbian learning is one of the oldest learning algorithms, and is based in large part on the dynamics of biological systems. A synapse between two neurons is strengthened when the neurons on either side of the synapse (input and output) have highly correlated outputs. In essence, when an input neuron fires, if it frequently leads to the firing of the output neuron, the synapse is strengthened. Following the analogy to an artificial system, the tap weight is increased with high correlation between two sequential neurons.

Roles of computational model:

- test hypotheses and make predictions
- build better artificial systems
- improve medical treatment

How is V1 constructed?

- Input-driven self-organization

Predictions:

- Input deprivation
- Connection patterns
- Plasticity
- Illusions and aftereffects
- Visual coding

Visual coding
--------------

The goal of visual coding is to represent the important features of the input and represent more information within a limited system
