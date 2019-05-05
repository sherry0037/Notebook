====================================
Hidden Markov Models
====================================
A summary of Chapter 13 Sequential Data of *Pattern Recognition and Machine Learning - Springer 2006*.

.. contents::

----------------------------------
Markov Models
----------------------------------

Using the product rule, we can express the joint distribution for a sequence of obervations in the form:

.. math::

    p(x_1, ..., x_N) &= \prod\limits_{n=1}^N p(x_1, ... x_{n-1})\\
                     &= p(x_1) p(x_2|x_1) p(x_3|x_2, x_1) ...


First-order Markov chain
----------------------------------

- **Definition:** First-Order Markov Chain
    If we assume that each of the conditional distributions on the RHS is independent of all previousobservations except the most recent (the Markov Property), we obtain the *first-order Markov chain*:

.. math::

    p(x_1, ..., x_N) &= p(x_1)\prod\limits_{n=2}^N p(x_n|x_{n-1})\\
                     &= p(x_1) p(x_2|x_1) p(x_3|x_2) ...


.. topic:: Illustration of First-Order Markov Chain

    .. figure:: _static/hidden_markov_models/first_order_markov_chain.png                     

The conditional distribution for observation :math:`x_n`, given all of the observations up to time :math:`n` is given by:

.. math::

    p(x_n | x_1, ..., x_{n-1}) = p(x_n|x_{n-1})


If we use this model to predict the next observation in a sequence, the distribution of predictions will only depend on the value of the immediately preceding observation, and will be independent of all earlier observations.


- **Definiton:** Homogeneous Markov Chain
    A markov chain is *homogeneous* when we assume a stationary time series, i.e. conditional distributions :math:`p(x_n|x_{n-1})` are equal. 


Higher-order Markov chain
----------------------------------
Sometimes the trends in the data over several successive observations will provide important information in predicting the next value. We can increase the number of obeservations the predictions depend on and obtain higher-order Markov chains.

For example, the joint distribution of the second-order Markov chain is given by:

.. math::

        p(x_1, ..., x_N) &= p(x_1)p(x_2|x_1)\prod\limits_{n=3}^N p(x_n|x_{n-1}, x_{n-2})\\
                     &= p(x_1) p(x_2|x_1) p(x_3|x_2, x_1) p(x_4|x_3, x_2) ...

.. topic:: Illustration of Second-Order Markov Chain

    .. figure:: _static/hidden_markov_models/second_order_markov_chain.png



More generally, we have :math:`M^{th}`-order markov chain where

.. math::

    p(x_n |x_1, ... x_{n-1}) = p(x_n| x_{n-1}, ... x_{n-M})


- *Note* the incresed flexibility results in a model with a much larger number of parameters (can grow exponentially).


Latent variables
----------------------------------
Suppose we do not want to limit our model to the Markov assumption and yet can be specified using a limited number of free parameters, we can introduce *latent variables* to permit a rich class of models to be constructed out of simple components. (Similar approach: mixture distributions, continuous latent variable models, etc.)

- **Definition** State Space Model
    For each observation :math:`x_n`, we introduce a corresponding latent variable :math:`z_n` (can have different dimentionality/type). If we assume that the latent variables form a Markov chain, we have a graphical structure called *state space model*. 

.. topic:: State Space Model

    .. _figure_13.5:
    .. figure:: _static/hidden_markov_models/markov_chain_latent.png


It satisfies the key conditional independence property that :math:`z_{n-1}` and :math:`z_{n+1}` are **independent** given :math:`z_n`, and the joint distribution of the model is given by:

.. math::
    :label: equ1

    p(x_1, ..., x_N, z_1, ... z_N) &= p(z_1) (\prod\limits_{n=2}^N p(z_n|z_{n-1})) (\prod\limits_{n=1}^N p(x_n|z_n))\\
                                   &= p(z_1)p(z_2|z_1)p(z_3|z_2)...p(x_1|z_1)p(x_2|z_2)p(x_3|z_3)...
    

Using the d-separation criterion, we see that there is always a path connecting any two observed variables :math:`x_n` and :math:`x_m` voa tje ;atemt variables, and that this path is never blocked.

In this model, the predictions for :math:`x_{n+1}` depends on all previous observations, because :math:`p(x_{n+1}|x_1, ..., x_n)` does not exhibit any conditional independence properties.


.. admonition:: TODO

    d-separation

There are two models described by this graph:

    1. *Hidden Markov model* if the latent variables are discrete;
    2. *Linear dynamical system* if both the latent and the observed variables are Gaussian.

*Note:* the observed variables in an HMM may be discrete or continuous, and a variety of different conditional distributions can be used to model them.


----------------------------------
Hidden Markov Models
----------------------------------
The hidden Markov model is a specific instance of the state space model in which the latent variables are discrete. If we examine a single time slice of the model, we see that it corresponds to a mixture distribution, with component densities given by :math:`p(\mathbf{x}|\mathbf{z})`.


.. admonition:: TODO

    Mixture model


The discrete multinomial variables :math:`\mathbf{z}_n` represent the latent variables, and :math:`\mathbf{x}_n` the corresponding observation. We use a 1-of-K coding scheme.


Specification - Transition probabilities, matrix and diagram
--------------------------------------------------------------

The latent variable :math:`\mathbf{z}_n` depends on the previous latent ariable :math:`\mathbf{z}_{n-1}`, and the conditional distribution :math:`p (\mathbf{z}_n|\mathbf{z}_{n-1})` is stored in the matrix :math:`\mathbf{A}`. The elements of :math:`\mathbf{A}` are referred to as **transition probabilities**:

.. math::

    A_{jk} := p(z_k^n = 1|z^{n-1}_j = 1), \\

where

.. math::

    0 \leq A_{jk} &\leq 1\\
    \sum_k A_{jk} &= 1

*Note:* 
    - :math:`A_{jk}` is the probabitliy of the latent variable transiting from state j to state k.
    - :math:`\mathbf{A}` has :math:`K(K-1)` independent parameters (-1 because the probabilities sum to one) 

The conditional distribution can then be explicitly written as:

.. math::

    p (\mathbf{z}_n|\mathbf{z}_{n-1}, \mathbf{A}) = \prod\limits_{k=1}^K \prod\limits_{j=1}^K A_{jk}^{z^{n-1}_j z^n_k}


The initial latent node :math:`\mathbf{z_1}` has a marginal distribution :math:`p(\mathbf{z_1})` represented by a vector of probabilities :math:`\mathbf{\pi}` with :math:`\pi_k := p(z^1_k = 1)`, so that

.. math::

    p(\mathbf{z_1}|\mathbf{\pi}) = \prod\limits_{k=1}^K \pi_k^{z_k^1},

where :math:`\sum_k \pi_k = 1`.

If we unfold a state transition diagram over time, we have an alternative representation of the transitions, known as *lattive* or *trellis* diagram.


.. topic:: Example of Transition Diagram 

    .. figure:: _static/hidden_markov_models/transition_diagram_1.png    

    .. figure:: _static/hidden_markov_models/transition_diagram_2.png     


Specification - Emission probabilities
------------------------------------------------
We now define the conditional distributions of the observed variables :math:`p(\mathbf{x}_n | \mathbf{z}_n, \mathbf{\phi})`, where :math:`\mathbf{\phi}` is a set of parameters governing the distribution. 

These emission probabilities can be given by:

    1. Gaussians if the elements of :math:`\mathbf{x}` are coninuous variables,
    2. or conditional probability tables if :math:`\mathbf{x}` is discrete.

Because :math:`\mathbf{x}_n` is observed, given :math:`\mathbf{\phi}`, :math:`p(\mathbf{x}_n | \mathbf{z}_n, \mathbf{\phi})` consists of a vector of K numbers, corresponding to the K possible states of the binary vector :math:`\mathbf{z}_n`:

.. math::

    p(\mathbf{x}_n | \mathbf{z}_n, \mathbf{\phi}) = \prod\limits_{k=1}^K p(\mathbf{x}_n | \mathbf{\phi})^{z^n_k}.


*Note:*

    - We will assume a *homogeneous* model, where the parameters :math:`\mathbf{A}` and :math:`\mathbf{\phi}` are shared.

    - A mixture model for an i.i.d. data set corresponds to the case where :math:`A_{jk}` are the same for all j. The conditional distribution :math:`p(\mathbf{z}_n | \mathbf{z}_{n-1})` is independent of :math:`\mathbf{z}_{n-1}`.
        - corresponds to deleting the horizontal links in :numref:`figure_13.5`.


Specification - Joint probability
------------------------------------------------
The joint probability distribution over both latent and observed variables is given by:

.. math::
    
    p(\mathbf{X}, \mathbf{Z} | \mathbf{\theta}) = p(\mathbf{z_1}|\mathbf{\pi}) \Big[ \prod\limits_{n=2}^N p(\mathbf{z}_n|\mathbf{z}_{n-1} \mathbf{A} ) \Big]  \prod\limits_{m=1}^N p(\mathbf{x}|\mathbf{z}, \mathbf{\phi}),

where :math:`\mathbf{X} = \{ \mathbf{x}_1, ...,  \mathbf{x}_N \}`, :math:`\mathbf{Z} = \{ \mathbf{z}_1, ...,  \mathbf{z}_N \}`, and :math:`\mathbf{\theta} = \{ \mathbf{\pi}, \mathbf{A}, \mathbf{\phi}\}` is the set of parameters.

*Note:* more general form: :eq:`equ1`