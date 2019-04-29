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


---------------------------------------
Decision Procedure: Theory of Equality
---------------------------------------

- **Congruence Closure Algorithm** is the decision procedure for theory of equality. It is used to decide the satisfiability in the quantifier-free fragment of :math:`T_=`.

    The algorithm computes the congruence closure of the binary relation defined by formula.

- Restrictions:
    - formula only contains conjunctions of literals
    - allow functions, but no predicates

    - eliminating predicates: tranform to *equisatisfiable* formula with only functions
        for each relation constant :math:`p`
            1. introduce a fresh function constant :math:`f_p`
            2. rewrite :math:`p(x_1, ... , x_n)` as :math:`f_p(x_1, ... , x_n) = t`, where :math:`t` is a fresh object constant


- **Definition:** Equivalence relation
    A binary relation :math:`R` over a set :math:`S` is an equivalence relation if it is

    1. reflexive: :math:`\forall s\in S. sRs`
    2. symmetric: :math:`\forall s_1, s_2 \in S. s_1Rs_2 \to s_2Rs_1`
    3. transitive: :math:`\forall s_1, s_2, s_3 \in S. s_1Rs_2 \land s_2Rs_3 \to s_1Rs_3`

- **Definition:** Congruence relation
    Consider set :math:`S` equipped with functions :math:`F = \{ f_1, ... ,f_n\}`

    A relation :math:`R` over :math:`S` is a congruence relation if it is an *equilalence relation* and for every n'ary function :math:`f \in F`:

    .. math::

        \forall \overrightarrow{s}, \overrightarrow{t}. \bigwedge\limits_{i=1}^n s_iRt_i \to f(\overrightarrow{s})Rf(\overrightarrow{t})

- **Definition** Equivalence/congruence class
    For a given equivalence relation :math:`R` over :math:`S`, the equivalence class of :math:`s \in S` under :math:`R` is the set:

    .. math::

        [s]_R := \{s' \in S: sRs' \}.

    If :math:`R` is a congruence relation, the set is called congruence class.

- **Definition:** Equivalence closure
    The equivalence closure :math:`R^E` of a binary relation :math:`R` over :math:`S` is the equivalence relation such that:

    1. :math:`R \subseteq R^E`
    2. for all other equivalence relations :math:`R'` s.t :math:`R \subseteq R'`, we have :math:`R^E \subseteq R'` 


    i.e. the smallest equivalence relation that includes :math:`R`.

- **Definition:** Congruence closure
    Similarly, the congruence closure :math:`R^C` is the smallest congruence relation that includes :math:`R`.

    
    *Example:* Consider :math:`S=\{a, b, c\}` and function :math:`f` such that:

    .. math::

        f(a) = b, \quad f(b)=c, \quad f(c)=c


    The conguence closure of relation :math:`\{ \langle a, b\rangle \}` is:

    .. math::

        R^C = \{ \langle a, b\rangle, \langle a, a\rangle, \langle b, b\rangle, \langle c, c\rangle, \langle b, a\rangle, \langle b, c\rangle, \langle c, b\rangle, \langle a, c\rangle, \langle c, a\rangle\}


- **Theorem:** Satisfiability of a :math:`\Sigma_=` formula
    Consider formula F 

    .. math::

        F: (s_1= t_1) \land ... \land (s_m = t_m) \land (s_{m+1} \neq t_{m+1}) \land ... \land (s_n \neq t_n)

    Let :math:`R_F = \{ \langle x, y \rangle | x=s_i, y=t_i, i\in [1, m]\}`

    F is satisfiable if the congruence closure :math:`\sim` of :math:`R_F` satisfies :math:`s_i \not\sim t_i` for all :math:`i\in[m+1, n]`


.. topic:: Congruence Closure Algorithm (Basic Idea)
    
    Congruence closure algorithm decide satisfiability of 

    .. math::
        
        F: (s_1= t_1) \land ... \land (s_m = t_m) \land (s_{m+1} \neq t_{m+1}) \land ... \land (s_n \neq t_n)

    Steps:

    1. Construct the *congruence closure* :math:`\sim` of :math:`R_F` over the subterm set :math:`S_F`
    
    2. If :math:`s_i \sim s_t` for any i in :math:`[m+1, n]`, :math:`F` is unsatisfiable

    3. Otherwise, :math:`F` is satisfiable.

----------------------------------------
Decision Procedure: Theory of Rationals
----------------------------------------


--------------------------------------
Decision Procedure: Theory of Integers
--------------------------------------


-------------------------------------------
Decision Procedure: Combined Theories
-------------------------------------------



-----------------------------------------------
SMT Solvers and DPPL(:math:`\tau`) Framework
-----------------------------------------------

