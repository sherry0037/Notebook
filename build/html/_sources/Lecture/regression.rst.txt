============================
Regression Analysis
============================

.. contents::

Chapter 1: Linear Regression with One Predictor Variable
========================================================

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
    :label: eq_b1

    b_1 = \frac{\sum(X_i - \bar{X}) (Y_i - \bar{Y})}{\sum(X_i - \bar{X})^2} \\

.. math::
    :label: eq_b0

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


.. topic:: Note

    The model error term :math:`\epsilon_i = Y_i - E[Y_i]` and the residual :math:`e_i = Y_i - \hat{Y}_i` are **different**. The former is unknown, because the true regression line, :math:`E[Y_i]` is unknown. The latter is known, because the fitted value :math:`\hat{Y}_i` on the estimated regression line is known.

----------------------------------------------------
Estimation of Error Terms Variance :math:`\sigma^2`
----------------------------------------------------

Point estimator of :math:`\sigma^2`
------------------------------------

First consider sampling from a single population. The variance :math:`\sigma^2` of a single population is estimated by the sample variance :math:`s^2`. The sample variance is calculated as follows:

.. math::

    \begin{equation} 
    \begin{split}
        s^2 &= \frac{\text{sum of squares}}{\text{degree of freedom}}\\
            &= \frac{\sum^n_{i=1}(Y_i - \bar{Y})^2}{n-1}
    \end{split}
    \end{equation}

The sample variance is often called a *mean square*.


Now consider the case for regression model. We also calculate the sum of squared first, then divide it by the degree of freedom. The sum of squares, denoted by SSE (*error sum of squares*, or *residual sum of squares*), is:

.. math::

    SSE = \sum^n_{i-1} (Y_i - \hat{Y}_i)^2 = \sum^n_{i-1}e_i^2

Note that here, the mean of :math:`Y_i` depends on the level :math:`X_i`; hence different to the previous case, we are subtracting the estimated mean :math:`\hat{Y}_i` for each level, instead of the sample mean :math:`\bar{Y}`.

The sum of squares SSE has :math:`n-2` degrees of freedom, because we need to estimate both :math:`\beta_0` and :math:`\beta_1`. The mean square, denoted by MSE (*error mean square* or *residual mean square*), is:

.. math::

    \begin{equation} 
    \begin{split}
        s^2 &= MSE = \frac{SSE}{n-1}
            = \frac{\sum^n_{i-1} (Y_i - \hat{Y}_i)^2}{n-2} = \frac{\sum^n_{i-1}e_i^2}{n-2}
    \end{split}
    \end{equation}

MSE is an unbiased estimator of :math:`\sigma^2`, so :math:`E[MSE] = \sigma^2`.


-------------------------------
Normally Distributed Error 
-------------------------------

To set up interval estimates and make tests, we need to make an assumption about the form of the distribution of the :math:`\epsilon_i`. The standard assumption is that they are normally distributed:

.. math::

    \epsilon_i \sim_{i.i.d} N(0, \sigma^2)

Note: the uncorrelatedness assumption of :math:`\epsilon_i` becomes independence. As a result, :math:`Y_i` are independent normal random variables.


Chapter 2: Inferences in Regression and Correlation Analysis
=============================================================
Throughout this chapter, we assume the normal error regression model described in Chapter 1 is applicable.

-------------------------------
Inferences on :math:`\beta_1`
-------------------------------
Frequently, we are interested in drawing inferences about :math:`\beta_1`, the slope of the regression line. We also want to do tests on :math:`\beta_1`, particularly in this form:

.. math::

    \begin{equation} 
    \begin{split}
        H_0: \beta_1 &= 0\\
        H_a: \beta_1 &\neq 0
    \end{split}
    \end{equation}

:math:`\beta_1 = 0` not only implies that there is no linear association between X and Y, but also that there is not relation of any type between X and Y, because the probability distributions of Y are then identical at all levels of X.

Sampling distribution of :math:`\beta_1`
----------------------------------------

The point estimator Ref :math:`b_1` is given in :eq:`eq_b1` as follows:

.. math ::

    b_1 = \frac{\sum(X_i - \bar{X}) (Y_i - \bar{Y})}{\sum(X_i - \bar{X})^2}

The sampling distribution of :math:`b_1` is the different values of :math:`b_1` that would be obtained with repeated sampling when the levels of X are held constant.

For normal error regression model, the sampling distribution of :math:`b_1` is normal, with mean and variance:

.. math::

    \begin{equation} 
    \begin{split}
        E[b_1] &= \beta_1\\
        \sigma^2[b_1] &= \frac{\sigma^2}{\sum(X_i - \bar{X})^2}
    \end{split}
    \end{equation}

The normality of :math:`b_1` follows from the fact that :math:`b_1` is a linear combination of the :math:`Y_i`.

We can estimate the variance of :math:`b_1` by replacing :math:`\sigma^2` with its unbiased estimator :math:`MSE`:

.. math::

    s^2[b_1] = \frac{MSE}{\sum(X_i - \bar{X})^2}

where the point estimator :math:`s^2[b_1]` is an unbiased estimator of :math:`\sigma^2[b_1]`.


Distribution of :math:`(b_1 - \beta_1) / s[b_1]`
-------------------------------------------------

Since :math:`b_1` is normally distributed, we know that the standardized statistic :math:`(b_1 - \beta_1) / \sigma[b_1]` has standard normal distribution.

We need to estimate :math:`\sigma[b_1]` by :math:`s[b_1]`, hence we are interested in the studentized statistic :math:`(b_1 - \beta_1) / s[b_1]`:

.. math::

    \frac{b_1 - \beta_1}{s[b_1]} \sim t(n-2),

which is a t distribution with :math:`n-2` degrees of freedom.


Confidence interval for :math:`\beta_1`
---------------------------------------

The  :math:`1-\alpha` confidence interval for :math:`\beta_1` is:

.. math::

    [b_1 - t(1-\alpha/2; n-2) s[b_1]], b_1 + t(1-\alpha/2; n-2) s[b_1]],

where :math:`t(1-\alpha/2; n-2)` denotes the :math:`(\alpha/2)100` percentile of the t distribution with n-2 degrees of freedom. For example, for a 95 percent confidence interval with sample size of 25, we have :math:`t(.975; 23) = 2.069`.

This can also be stated in a probability statement:

.. math::

    P[t(\alpha/2; n-2) \leq \frac{b_1 - \beta_1}{s[b_1]} \leq t(1-\alpha/2; n-2)] = 1 - \alpha


Hypothesis test for :math:`\beta_1`
---------------------------------------
Consider a two-sided test with the test alternatives:

.. math::

    \begin{equation}                                   
    \begin{split}
        H_0: \beta_1 &= 0\\
        H_a: \beta_1 &\neq 0
    \end{split}
    \end{equation}

We have the test statistic:

.. math::

    t^* = \frac{b_1}{s[b_1]} \sim t(n-2)

and the decision rules is:

.. math::

    \begin{equation} 
    \begin{split}
        &\text{If } |t^*| \leq t(1-\alpha/2; n-2)\text{, reject }H_0\\
        &\text{If } |t^*| > t(1-\alpha/2; n-2)\text{, fail to reject }H_0 
    \end{split}
    \end{equation}

The P-value is the probability :math:`P[t(n-2) > t^*]`. If P-value is less than the level of significance :math:`\alpha`, we reject the null hypothesis. 

P-value is interpreted as follows: if :math:`b_1 = 5`, the P-value is the probability of observing :math:`b_1` at least far away from 0 (:math:`b_1 \geq 5` or :math:`b_1 \leq -5`) assuming that the Null hypothesis is true.


-------------------------------
Inferences on :math:`\beta_0`
-------------------------------
We only consider the case when the scope of the model includes :math:`X=0`.

Sampling distribution of :math:`\beta_1`
----------------------------------------

The point estimator Ref :math:`b_0` is given in :eq:`eq_b0` as follows:

.. math ::

    b_0 = \bar{Y} - b_1 \bar{X}

For normal error regression model, the sampling distribution of :math:`b_1` is normal, with mean and variance:

.. math::

    \begin{equation} 
    \begin{split}
        E[b_0] &= \beta_0\\
        \sigma^2[b_0] &= \sigma^2 \left[ \frac{1}{n} + \frac{\bar{X}^2}{\sum(X_i - \bar{X})^2} \right]
    \end{split}
    \end{equation}

The normality of :math:`b_0` follows from the fact that :math:`b_0` is a linear combination of the :math:`Y_i`.

We can estimate the variance of :math:`b_0` by replacing :math:`\sigma^2` with its unbiased estimator :math:`MSE`:

.. math::

    s^2[b_0] = MSE \left[ \frac{1}{n} + \frac{\bar{X}^2}{\sum(X_i - \bar{X})^2} \right]

where the point estimator :math:`s^2[b_0]` is an unbiased estimator of :math:`\sigma^2[b_0]`.

Confidence interval for :math:`\beta_0`
----------------------------------------

The  :math:`1-\alpha` confidence interval for :math:`\beta_0` is:

.. math::

    [b_0 - t(1-\alpha/2; n-2) s[b_0]], b_0 + t(1-\alpha/2; n-2) s[b_0]]

-------------------------------
Power of Tests
-------------------------------
Consider the general test concerning :math:`\beta_1`: 

.. math::

    \begin{equation}                                   
    \begin{split}
        H_0: \beta_1 &= \beta_{10}\\
        H_a: \beta_1 &\neq \beta_{10}
    \end{split}
    \end{equation}

The **power** of this test is the probability that the decision rule will lead to conclusion :math:`H_a` when :math:`H_a` holds:

.. math::

    Power = P(|t^*| > t(1-\alpha/2; n-2) | \delta),

where :math:`\delta` is the *noncentrality measure* - how far the true value of :math:`\beta_1` is from :math:`\beta_{10}`:

.. math::

    \delta = \frac{\beta_1 - \beta_{10}}{\sigma{\beta_1}}


