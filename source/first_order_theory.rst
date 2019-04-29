====================================
First Order Theroies
====================================
A summary of the lecture notes of `CS389L <http://www.cs.utexas.edu/~isil/cs389L/>`_-Automated Logical Reasoning.


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
    A structure :math:`M = \langle U, I\rangle` is a model of theory T, or T-model, if :math:`M \vDash A` for every :math:`A \in A_T`.

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


- **Decidability and Completeness**

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

    2. Pluse zero: :math:`\forall x. x+0=x`
        - identity for addition

    3. Successor: :math:`\forall x, y. x+1=y+1 \to x=y`

    4. Plus successor: :math:`\forall x, y. x+ (y+1) = (x+y)+1`

    5. Times zero: :math:`\forall x. x \cdot 0 = 0`

    6. Times successor: :math:`\forall x, y. x \cdot (y+1) = x\cdot y + x`

    7. Axiom schema for induction:
        - any valid interpretation must obey induction

    .. math::

        (F[0] \land (\forall x. F[x] \to f[x+1])) \to \forall x. F[x]


- **Inequalities**
    The theory of Peano arithmetric doesn't have inequality symbols.

    - :math:`x \cdot y \geq z \Rightarrow \exists w. x \cdot y = z + w`
    - :math:`x \cdot y < z \Rightarrow \exists w. \lnot (w=0) \land x \cdot y + w = z`


- **Decidability and Completeness**
    
    - Validity in full :math:`T_{PA}` is *undecidable*

    - Validity in quantifier-free fragment of :math:`T_{PA}` is *undecidable*

    - :math:`T_{PA}` is *imcomplete*

    - Where problem is: multiplication!

        


Presburger Arithmetric (natural, addition)
--------------------------------------------------------------------
- **Signature**

    .. math::

        \Sigma_{\mathbb{N}} : \{0, 1, +, =\}

    - *Note:* remove multiplication from Peano

- **Axioms**

    Same as Peano's, except removing *times zero* and *times successor*.


- **Decidability and Completeness**
    
    - Validity in full :math:`T_{PA}` is *decidable*
       
        - super exponential :math:`O(2^{2^n})`

    - Validity in quantifier-free fragment of :math:`T_{PA}` is *decidable*
        
        - but in coNP-complete (compliment is NP-complete)
        
        - quantifier elimination: for any formula F in :math:`T_{\mathbb{N}}`, there is an equivalent quantifier-free formula F'.

    - :math:`T_{PA}` is *complete*
        
        - for any sentence F, :math:`T_{\mathbb{N}} \vDash F \lor T_{\mathbb{N}}\vDash \lnot F`

Theory of integers 
----------------------------------

- **Signature**

    .. math::

        \Sigma_{\mathbb{z}} : \{..., -2, -1, 0, 1, 2, ...., -3 \cdot, -2 \cdot, 2 \cdot, 3 \cdot ,..., +, =, > \}

    - *Note:* only has >
    - also referred to as: linear arthmetric over integer
    - equicalent in expressiveness to Presburger arithmetic


Theory of rationals 
----------------------------------

- **Signature**

    .. math::

        \Sigma_{\mathbb{Q}} : \{0, 1, +, -, =, \geq\}

    - *Note:* doesn't allow strict inequality
        - :math:`\forall x,y. \exists z. x+y>z \Rightarrow \forall x, y. \exists z. \lnot (x+y=z) \land x+y \geq z`


- **Decidability**
    
    - Validity in full :math:`T_{\mathbb{Q}}` is *decidable*
       
        - but doubly exponential

    - Validity in *conjuctive quantifier-free* fragment :math:`T_{\mathbb{Q}}` is *decidable* in *polynomial* time



Theory of arrays
----------------------------------

- **Signature**

    .. math::

        \Sigma_{A} : \{\cdot[\cdot], \cdot \langle \cdot \triangleleft \cdot \rangle, =\}

    - :math:`a[i]` binary function
        - read array a at index i ("read(a, i)")
    - :math:`a \langle i \triangleleft v \rangle` ternary function
        - write value v to index i of array a ("write(a, i, e)")
        - represents the resulting array after writing 


- **Axioms**

    Reflexivity, symmety, transitivity, and the following:

    1. Array congruence: :math:`\forall a, i, j. i=j \to a[i] = a[j]`
    2. Read-over-write 1: :math:`\forall a, v, i, j. i=j \to a \langle i \triangleleft v \rangle [j] = v`
    3. Read-over-write 2: :math:`\forall a, i, j. i \neq j \to a \langle i \triangleleft v \rangle [j] = a[j]`



- **Decidability**
    
    - Validity in full :math:`T_{A}` is *not* decidable

    - Validity in *quantifier-free* fragment of :math:`T_{A}` is *decidable* but not expressive enough


Combined Theories
----------------------------------

Given two theories :math:`T_1` and :math:`T_2` that have the :math:`=` predicate, we define a combined theory :math:`T_1 \cup T_2`:

- **Signature**: :math:`\Sigma_1 \cup \Sigma_2`

- **Axioms** :math:`A_1 \cup A_2`


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