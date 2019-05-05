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

- **Definition:** first-order Markov chain
    If we assume that each of the conditional distributions on the RHS is independent of all previousobservations except the most recent (the Markov Property), we obtain the *first-order Markov chain*:

.. math::

    p(x_1, ..., x_N) &= p(x_1)\prod\limits_{n=2}^N p(x_n|x_{n-1})\\
                     &= p(x_1) p(x_2|x_1) p(x_3|x_2) ...


.. topic:: Illustration of first-order Markov chain

    .. figure:: _static/hidden_markov_models/first_order_markov_chain.png                     

The conditional distribution for observation :math:`x_n`, given all of the observations up to time :math:`n` is given by:

.. math::

    p(x_n | x_1, ..., x_{n-1}) = p(x_n|x_{n-1})


If we use this model to predict the next observation in a sequence, the distribution of predictions will only depend on the value of the immediately preceding observation, and will be independent of all earlier observations.


- **Definiton:** homogeneous Markov chain
    A markov chain is *homogeneous* when we assume a stationary time series, i.e. conditional distributions :math:`p(x_n|x_{n-1})` are equal. 


Higher-order Markov chain
----------------------------------
Sometimes the trends in the data over several successive observations will provide important information in predicting the next value. We can increase the number of obeservations the predictions depend on and obtain higher-order Markov chains.

For example, the joint distribution of the second-order Markov chain is given by:

.. math::

        p(x_1, ..., x_N) &= p(x_1)p(x_2|x_1)\prod\limits_{n=3}^N p(x_n|x_{n-1}, x_{n-2})\\
                     &= p(x_1) p(x_2|x_1) p(x_3|x_2, x_1) p(x_4|x_3, x_2) ...

.. topic:: Illustration of second-order Markov chain

    .. figure:: _static/hidden_markov_models/second_order_markov_chain.png



More generally, we have :math:`M^{th}`-order markov chain where

.. math::

    p(x_n |x_1, ... x_{n-1}) = p(x_n| x_{n-1}, ... x_{n-M})


- *Note* the incresed flexibility results in a model with a much larger number of parameters (can grow exponentially).


Latent variables
----------------------------------
Suppose we do not want to limit our model to the Markov assumption and yet can be specified using a limited number of free parameters, we can introduce *latent variables* to permit a rich class of models to be constructed out of simple components. (Similar approach: mixture distributions, continuous latent variable models, etc.)

- **Definition** state space model
    For each observation :math:`x_n`, we introduce a corresponding latent variable :math:`z_n` (can have different dimentionality/type). If we assume that the latent variables form a Markov chain, we have a graphical structure called *state space model*. 

.. topic:: State space model

    .. figure:: _static/hidden_markov_models/markov_chain_latent.png


It satisfies the key conditional independence property that :math:`z_{n-1}` and :math:`z_{n+1}` are **independent** given :math:`z_n`, and the joint distribution of the model is given by:

.. math::

    p(x_1, ..., x_N, z_1, ... z_N) &= p(z_1) (\prod\limits_{n=2}^N p(z_n|z_{n-1})) (\prod\limits_{n=1}^N p(x_n|z_n))\\
                                   &= p(z_1)p(z_2|z_1)p(z_3|z_2)...p(x_1|z_1)p(x_2|z_2)p(x_3|z_3)...






----------------------------------
Hidden Markov Models
----------------------------------