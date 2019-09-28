============================
Regression Analysis
============================

-------------------------------
Simple Linear Regression Model
-------------------------------

Model definition
---------------------------------
This is a basic regression model with one predictor variable and the function is linear:

.. math::

    Y_i = \beta_0 + \beta_1 X_i + \epsilon_i

where:
    - :math:`Y_i` is the value of the response variable in the ith trial
    - :math:`\beta_0` and :math:`\beta_1` are parameters
    - :math:`X_i` is the value of the predictor variable in the ith trial. :math:`X_i` is a constant
    - :math:`\epsilon_i` is a random error term with mean :math:`E[\epsilon_i] = 0` and variance :math:`\sigma^2[\epsilon_i] = \sigma^2`

For this model, there are some importance features:

1. :math:`\beta_0 + \beta_1 X_i` is a constant term, :math:`\epsilon_i` is a random term, hence :math:`Y_i` is a random variable

2. :math:`E[Y_i] = \beta_0 + \beta_1 X_i`

3. :math:`\sigma^2[Y_i] = \sigma^2[\epsilon_i] = \sigma^2`

4. The error terms are uncorrelated to each other, so are the response :math:`Y_i`

The **regression function**, by definition, relates the means of the probability distributions of Y for given X to the level of X. Hence, the regression function for this model is:

.. math::

    E[Y] = \beta_0 + \beta_1 X

.. topic:: A Simple Linear Regression Model    

    A consultant is studying the relationship between the number of bids requested by construction contractors for basic lighting equipment during a week and the time required to prepare the bids. Suppose the regression model is:

    .. math::

        Y_i = 9.5 + 2.1X_i + \epsilon_i

    .. figure:: _static/regression/figure1_6.png 
        :scale: 40 %

Meaning of regression parameters
---------------------------------

The parameters :math:`\beta_0` and :math:`\beta_1` are *regression coefficients*. 

- :math:`\beta_0` is the *slope* of the regression line. It indicates the change in the mean of :math:`Y` per unit increase in :math:`X`.

- :math:`\beta_1` is the :math:`Y` *intercept* of the regression line. When the scope of the model includes :math:`X=0`, :math:`\beta_0` gives the mean of the probability distribution of :math:`Y` at :math:`X=0`.
    - Note: when the scope of the model does not cover :math:`X = 0`, the intercept does not have any particular meaning. For example, we cannot say the time to build a lot with 0 size is a positive number.

----------------------------------
Estimation of Regression Function
----------------------------------

Method of least squares
----------------------------------
We use the method of least squares to find estimators of the regression parameters :math:`\beta_0` and :math:`\beta_1`. This method considers the sum of the squared deviations of each observations :math:`Y_i` from its expected value :math:`E[Y_i]`:

.. math::

    Q = \sum_n^{i=1} (Y_i - \beta_0 -\beta_1X_i)^2

The estimators of :math:`\beta_0` and :math:`\beta_1` are the values :math:`b_0` and :math:`b_1` that **minimize** the criterion :math:`Q` for a given sample observations :math:`(X_1, Y_1), ... (X_n, Y_n)`. :math:`b_0` and :math:`b_1` are called *point estimators* of :math:`\beta_0` and :math:`\beta_1`, respectively.

.. topic:: Example

    In a small-scale study of persistence, an experimenter gave three subjects a very difficult task. Data on the age of the subject (X) and on the number of attempts to accomplish the task before giving up (Y) follow:

    =======================   === === ====
    Subject i:                1   2   3
    =======================   === === ====
    Age Xi:                   20  55  30
    Number of attempts Yi:    5   12  10
    =======================   === === ====

    .. figure:: _static/regression/figure1_9.png 
        :scale: 40 %

    In this example, the estimators on the right are better than those on the left.


We can calculate :math:`b_0` and :math:`b_1` using the following equations:

.. math::

    b_1 = \frac{\sum(X_i - \bar{X}) (Y_i - \bar{Y})}{\sum(X_i - \bar{X})^2} \\

.. math::

    b_0 = \bar{Y} - b_1 \bar{X}

where :math:`\bar{X}` and :math:`\bar{Y}` are the means of the :math:`X_i` and the :math:`Y_i` observations, respectively.


There is an important theorem about the least squares estimators:

.. topic:: Gauss-Markov theorem

    Under the conditions of the regression model, the least squares estimators :math:`b_0` and :math:`b_1` are *unbiased* and have minimum variance among all unbiased linear estimators.

Because :math:`b_0` and :math:`b_1` are unbiased estimators, we have: 

.. math::

    E[b_0] = \beta_0

.. math::

    E[b_1] = \beta_1

The second statement of the theorem means that among all unbiased linear estimators, :math:`b_0` and :math:`b_1` have the smallest variability in repeated samples in which the X levels remain unchanged.


Point estimation of mean response
----------------------------------
Given sample estimators :math:`b_0` and :math:`b_1` of the parameters in the regression function:

.. math::

    E[Y] = \beta_0 + \beta_1 X

we estimate the regression function as follows:

.. math::
    
    \hat{Y} = b_0 + b_1X 

where :math:`\hat{Y}` is the value of the estimated regression function at the level :math:`X` of the predictor variable.

Here are some names and definitions:

- value of the response variable :math:`Y` is called *response*
- :math:`E[Y]`, the mean of the probability distribution of :math:`Y` corresponding to the level :math:`X` of the predictor variable, is called the *mean response*
- :math:`\hat{Y}` is a point estimator of the mean response at level :math:`X`
    - :math:`\hat{Y}` is an unbiased estimator of :math:`E[Y]`, with minimum variance
- :math:`\hat{Y}_i = b_0 + b_1X_i` for :math:`i = 1, ..., n` is the *fitted value* for the ith case
    - Note: :math:`Y_i` is the observed value for the ith case


Residuals
----------------------------------

The ith *residual* is the difference between the observed value :math:`Y_i` and the corresponding fitted value :math:`\hat{Y}_i`:

.. math::

    e_i = Y_i - \hat{Y}_i = Y_i - b_0 - b_1X_i


.. figure:: _static/regression/figure1_12.png 
    :scale: 40 %
