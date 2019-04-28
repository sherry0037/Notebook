====================================
First Order Theroies
====================================
A summary of the lecture notes of `CS389L <http://www.cs.utexas.edu/~isil/cs389L/>`_.-Automated Logical Reasoning.


.. contents::

----------------------------------
Syntax and Semantics
----------------------------------

- A first-order theory T consists of
    - Signature :math:`\Sigma_T`
        set of constants, functions, and predicate symbols
    - Axioms :math:`A_T`
        - set of FOL sentences over :math:`\Sigma_T`
        - provide the meaning of symbols in :math:`\Sigma_T`

- **Definition:** :math:`\Sigma_T` formula
    Formula constructed from symbols of :math:`\Sigma_T` and variables, logival connectives and quantifiers.

- **Definition:** T-Model
    A structure :math:`M = <U, I>` is a model of theory T, or T-model, if :math:`M \vDash A` for every :math:`A \in A_T`.

- **Definition:** Satistiable modulo T
    Formula F is satistiable modulo T if *there exists* a T-model M and variable assignment :math:`\sigma` 
    such that :math:`M, \sigma \vDash F`.

- **Definition:** Valid modulo T
    Formula F is valid modulo T if *for all* a T-models M and variable assignments :math:`\sigma`, we have :math:`M, \sigma \vDash F`.

    Write as: :math:`T \vDash F`.

    - if a formula is valid in FOL, it is also valid module T
    - if a formula is valid modulo T, it is *not* necessarily valid in FOL.

- **Definition:** Equivalence modulo T
    Two formulas :math:`F_1` and :math:`F_2` are euivalent module theory T if for every T-model M and for every variable assignment
    :math:`\sigma`:

    .. math::

        M, \sigma \vDash F_1 \text{ iff } M, \sigma \vDash F_2

    Or write as:

    .. math::

        T \vDash F_1 \leftrightarrow F_2


    - *Example:* :math:`x = y` and :math:`y = x` are equivalent modulo :math:`T_=`, but not equivalent in FOL

- **Definition:** Completeness of Theory
    A theory T is complete if for every *sentence* (no free variable) F, if T entails F or its negation:

    .. math::

        T \vDash F \text{ or } T \vDash \lnot F

    - FOL is *not* complete: neither :math:`p(a)` nor :math:`\lnot p(a)` is valid.







----------------------------------
Commonly-Used First-Order Theories
----------------------------------

Theory of Equality :math:`T_=`
----------------------------------

- **Signature**

    .. math::

        \Sigma_= : \{=, a, b, c, ..., f, g, h, ..., p, q, r, ...\}

    - :math:`=`: binary predicate
    - all constants, functions and predicate symbols

- **Axioms**
    
    1. Reflexivity: :math:`\forall x. x=x`

    2. Symmetrix: :math:`\forall x, y. (x=y \to y=x)`

    3. Transitivity :math:`\forall x, y, z. (x=y \land y=z \to x=z)`


- **Axiom Schemata**
    **Definition:** A set of axioms instantiated for each function and predicate symbol.

    - Function congruence:
        For any n-ary *function* f, two terms :math:`f(\overrightarrow{x})` and :math:`f(\overrightarrow{y})` are equal if :math:`\overrightarrow{x}` and :math:`\overrightarrow{y}` are equal:

        .. math::

            \forall x_1, ..., x_n, y_1, ... ,y_n. \bigwedge\limits_{i} (x_i = x_j) \to (f(x_1, ... x_n) = f(y_1, ..., y_n))


    - Predicate congruence:
        For any n-ary *predicate* p, two formulas :math:`p(\overrightarrow{x})` and :math:`p(\overrightarrow{y})` are equivalent if :math:`\overrightarrow{x}` and :math:`\overrightarrow{y}` are equal:

        .. math::

            \forall x_1, ..., x_n, y_1, ... ,y_n. \bigwedge\limits_{i} (x_i = x_j) \to (p(x_1, ... x_n) \leftrightarrow p(y_1, ..., y_n))


- Decidability and Completeness

    - *Not* decidable (wether a :math:`T_=` formula is provable)

        - FOL is not decidable.

        - Quantifier-free fragment is decidable but *NP-complete*.

    - *Not* complete



Peano Arithmetric :math:`T_{PA}` (natural, multiplication, addition)
--------------------------------------------------------------------

- **Signature**

    .. math::

        \Sigma_{PA} : \{0, 1, +, \cdot, =\}

    - 0, 1 are constants
    - +, :math:`\cdot` are binary functions
    - = is a binary predicate

- **Axioms**

    Equality axioms, reflexivity, symmety, transitivity, and the following:
    
    1. Zero: :math:`\forall x. \lnot(x+1 = 0))`
        - :math:`0` is the minimal element of :math:`\mathbb{N}`


    .. admonition:: todo

        add axioms


Presburger Arithmetric (natural, addition)
--------------------------------------------------------------------


Theory of integers 
----------------------------------



----------------------------------
Theory of Equality
----------------------------------


----------------------------------
Theory of Rationals
----------------------------------


----------------------------------
Theory of Integers
----------------------------------


-------------------------------------------
Decision Procedures for Combined Theories
-------------------------------------------



-----------------------------------------------
SMT Solvers and DPPL(:math:`\tau`) Framework
-----------------------------------------------