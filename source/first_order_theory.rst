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
        For any n-ary *function* f, two terms :math:`f(\vec{x})` and :math:`f(\vec{y})` are equal if :math:`\vec{x}` and :math:`\vec{y}` are equal:

        .. math::

            \forall x_1, ..., x_n, y_1, ... ,y_n. \bigwedge\limits_{i} (x_i = x_j) \to (f(x_1, ... x_n) = f(y_1, ..., y_n))


    - Predicate congruence:
        For any n-ary *predicate* p, two formulas :math:`p(\vec{x})` and :math:`p(\vec{y})` are equivalent if :math:`\vec{x}` and :math:`\vec{y}` are equal:

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

- **Congruence Closure Algorithm** is the decision procedure for theory of equality. It is used to decide the satisfiability in the *quantifier-free* fragment of :math:`T_=`.

    The algorithm computes the congruence closure of the binary relation defined by formula.

- Restrictions:
    - formula only contains *conjunctions* of literals
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

        \forall \vec{s}, \vec{t}. \bigwedge\limits_{i=1}^n s_iRt_i \to f(\vec{s})Rf(\vec{t})

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

Congruence Closure Algorithm
---------------------------------

.. topic:: Congruence Closure Algorithm (Basic Idea)
    
    Congruence closure algorithm decides satisfiability of 

    .. math::
        
        F: (s_1= t_1) \land ... \land (s_m = t_m) \land (s_{m+1} \neq t_{m+1}) \land ... \land (s_n \neq t_n)

    Steps:

    1. Construct the *congruence closure* :math:`\sim` of :math:`R_F` over the subterm set :math:`S_F`
    
    2. If :math:`s_i \sim s_t` for any i in :math:`[m+1, n]`, :math:`F` is unsatisfiable

    3. Otherwise, :math:`F` is satisfiable.


.. admonition:: TODO

    add example


- Computing congruence closure
    To compute congruence closure efficiently, we'll represent the subterm set of the formula as a DAG:

    - Node: a subterm and a unique id

    - Edges: point from function symbol to arguments

    We need to merge congruence classes:

    - Each class has a *representative*, each subterm (node) has a *find* pointer that eventually leads to the representative of its congruence class.

    - Each representative node has *parents*: pointer from representative to parents of all subterms in the class.

    How to merge congruence classes of two terms :math:`t_1` and :math:`t_2`:

    1. Find representatives of :math:`t_1` and :math:`t_2`

    2. Change *find* field of :math:`Rep(t_1)` to point to :math:`Rep(t_2)`

    3. Update parents: move parents of :math:`Rep(t_1)` to :math:`Rep(t_2`)

- Process equalities
    To process :math:`t_1 = t_2`:

    1. Find representatives of :math:`t_1` and :math:`t_2`

    2. Merge equivalence classes

    3. Retrieve the set of parents :math:`P_1`, :math:`P_2` stored in :math:`Rep(t_1)`, :math:`Rep(t_2`)

    4. For each :math:`(p_i, p_j) \in P_1 \times P_2`, if :math:`p_1` and :math:`p_2` are *congruent*, process equality :math:`p_i = p_j`

        - *Note:* at this step, new equalities may be generated.


.. topic:: Congruence Closure Algorithm (Full Version)
    
    Congruence closure algorithm decides satisfiability of 

    .. math::
        
        F: (s_1= t_1) \land ... \land (s_m = t_m) \land (s_{m+1} \neq t_{m+1}) \land ... \land (s_n \neq t_n)

    Steps:

    1. Compute subterms and construct initial DAG (each node's representative is itself)

    2. For each  :math:`i \in [i, m]`, process equality :math:`s_i = t_i`

    3. For each  :math:`i \in [i, m]`, process inequality :math:`s_i \neq t_i`:

        (i) if :math:`\exists i. Rep(s_i) = Rep(t_i)`: return UNSAT

        (ii) if :math:`\forall i. Rep(s_i) \neq Rep(t_i)`: return SAT


.. admonition:: TODO

    add example


- Time complexity
    This algorithm is :math:`O(e^2)`. Can be solved in :math:`O(elog(e))`.


----------------------------------------
Decision Procedure: Theory of Rationals
----------------------------------------
We only consider *quantifier-free* *conjuctive* :math:`T_{\mathbb{Q}}` formulas. Deciding satistiability of qff conjuctive formulas is a special case of *linear programming*, which can be solved by the *Simplex* algorithm.


Linear programming
----------------------------------------

.. topic:: Linear Programming

    In a **linear programming** problem, we have an :math:`m \times n` matrix :math:`A`, an :math:`m`-dimensional vector :math:`\vec{b}`, and an :math:`n`-dimensional vector :math:`\vec{c}`

    We want to solve the problem:

    .. math::

        max_{\vec{x}} \vec{c}^T \vec{x}

    subject to 

    .. math ::
        A\vec{x} \leq \vec{b}


.. admonition:: TODO

    Geometric formulation and LP lingo

    - feasible solution
    - optimal solution
    - bounded


- :math:`T_\mathbb{Q}` as LP problem

    1. Convert a :math:`T_\mathbb{Q}` formula to NNF

    2. Rewrite it as *equisatisfiable* formula containing only :math:`\leq` and :math:`>0`:

    .. math::

        \vec{a}^T \vec{x} \geq c \quad&\Rightarrow\quad -\vec{a}^T \vec{x} \leq -c\\
        \vec{a}^T \vec{x} < c    \quad&\Rightarrow\quad \vec{a}^T \vec{x} + y \leq c \land y > 0\\
        \vec{a}^T \vec{x} = c    \quad&\Rightarrow\quad \vec{a}^T \vec{x} \leq c \land -\vec{a}^T \vec{x} \leq -c\\
        \vec{a}^T \vec{x} \neq c \quad&\Rightarrow\quad (\vec{a}^T \vec{x} + y \leq c \land y >0) \lor (-\vec{a}^T \vec{x} + y \leq -c \land y >0)

    
    3. Convert to DNF. F is satisfiable iff any of the clauses satisfiable.
    Each clause is of the following form:

    .. math::

        &\bigwedge a_{i1}x_i + ... + a_{in}x_n \leq b_i\\
        \land \quad &\bigwedge a_{i1}x_i + ... + a_{in}x_n + y \leq \beta_i\\
        \land \quad &y>0

    This constraint is satisfiable iff the opitmal solution of the following LP problem is **strictly positive**:

    .. math::

        &\text{Maximize      } \quad y \\
        &\text{Subject to} \quad\bigwedge a_{i1}x_i + ... + a_{in}x_n \leq b_i \\
        &\qquad\quad\land \quad \bigwedge a_{i1}x_i + ... + a_{in}x_n + y \leq \beta_i


Simplex algorithm
----------------------------------------
To apply Simplex, a linear inequality system needs to be converted into *standard form*, and then into *slack form*.

- **Standard form**

.. math::

    \text{Maximize      } &\quad\vec{C}^T\vec{x} \\
        \text{Subject to} &\quad A\vec{x} \leq \vec{b}\\
                            &\quad\vec{x} \geq 0


- We can convert every LP problem into an *equisatisfiable* standard form representation.
    - Equisatisfiable: original problem has optimal objective value c iff problem in standard form has optimal objective value c

    - If :math:`x_i` does not have non-negativity constraint
        - introduce :math:`x_i'` and :math:`x_i''`
        - replace :math:`x_i` with :math:`x_i' - x_i''`
        - add two constraints :math:`x_i' \geq 0` and :math:`x_i'' \geq 0`.


- **Slack form**

    In slack form, we only have equalities; the only inequality allowed is non-negativity constraints

    - For each inequality :math:`A_i\vec{x} \leq b_i`, introduce a *slack variable* :math:`s_i`.

    - Rewrite inequality as equality :math:`s_i = b_i - A_ix` and introduce non-negativity constraint :math:`s_i \geq 0`


- **Example**

    Consider the following linear program:

    .. math::

        \text{Maximize}   &\quad 2x_1 - 3x_2 \\
        \text{Subject to} &\quad x_1 + x_2 \leq 7 \\
                          &\quad -x_1 - x_2 \leq -7 \\
                          &\quad x_1 - 2 x_2 \leq 4 \\
                          &\quad x_1 \geq 0\\

    Equisatisfiable system in standard form (replace :math:`x_1` with :math:`x_2 - x_3`):

    .. math::

        \text{Maximize}   &\quad 2x_1 - 3x_2  + 3x_3\\
        \text{Subject to} &\quad x_1 + x_2 - x_3 \leq 7 \\
                          &\quad -x_1 - x_2 + x_3 \leq -7 \\
                          &\quad x_1 - 2 x_2 + 2x_3 \leq 4 \\
                          &\quad x_1, x_2, x_3\geq 0\\


    In slack form:

    .. math::

        \text{Maximize}   &\quad 2x_1 - 3x_2  + 3x_3\\
        \text{Subject to} &\quad x_4 = 7 - x_1 - x_2 + x_3 \\
                          &\quad x_5 = -7 + x_1 + x_2 - x_3  \\
                          &\quad x_6 = 4 - x_1 + 2 x_2 - 2x_3 \\
                          &\quad x_1, x_2, x_3, x_4, x_5, x_6 \geq 0\\


- **Slack form**

    - variables on the left-hand side are *basic variables*, denoted by :math:`B`

    - variables on the right-hand side are *non-basic variables*, denoted by :math:`N`

    - **Invariant:** only non-basic variables can appear in the objective function

    - write the slack form as:

        .. math::

            z &= v + \sum\limits_{x_j\in N} c_j x_j \quad\text{(objective function)}\\
            x_i &= b_i - \sum\limits_{x_j\in N}  a_{ij}x_j \quad(\text{for every  } x_i \in B)


        - the non-negativity constraints are omitted

- The Simplex Algorithm
    The algorithm has two phases:

        1. *Phase 1:* Compute a feasible basic solution, if one exists
        2. *Phase 2:* Optimize value of objective function (by pivoting)


.. topic:: Simplex Phase 2

    In phase 2, we start with a feasible basic solution, then each iteration rewrites one slack from into an equivalent slack form (pivot). Geometrically, each iteration walks from one vertex to an adjacent vertex until it reaches a local maximum, which is also the global optimum by convexity.

    We have the problem:

    .. math::

            z &= v + \sum\limits_{x_j\in N} c_j x_j \quad\text{(objective function)}\\
            x_i &= b_i - \sum\limits_{x_j\in N}  a_{ij}x_j \quad(\text{for every  } x_i \in B)

    Step 1: given term :math:`c_jx_j` with positive :math:`c_j` in objective function, we want to increase :math:`x_j` as much as possible.

        - Find the most restricting equality for :math:`x_j`:
            1. :math:`x_j`'s coefficient :math:`a_{ij}` is positive
            2. has smallest value of :math:`\frac{b_i}{a_{ij}}`

    Step 2: pivot operation

        - Suppose the equality with basic var :math:`x_i` is the most restrictive for :math:`x_j`

        - Rewrite :math:`x_j` in terms of :math:`x_i` and plug into other equations

        - Now :math:`x_j` is basic, :math:`x_i` is non-basic. :math:`x_j`'s value increased from 0 to :math:`\frac{b_i}{a_{ij}}` (also the objective value) 

    Repeats this operation until one of the two conditions hold:

        1. *ALL* coefficients in objective function are *nagative*
            - found optimal solution

        2. There exists a non-basic variable :math:`x_j` with positive coefficient :math:`c_j` in objective functon, but all coefficients :math:`a_{ij}` are negative
            - optimal solution = :math:`\infty`


.. admonition:: TODO

    add example

- Degenerate problems
    The objective value can stay the same after pivoting. For degenerate problems, Simplex might not terminate.

    There are pivot selection strategies for which Simplex is guaranteed to terminate.

        - **Bland's Rule** if there are multiple variables with positive coeeficients in objective funtion, always choose the variable with the *smallest* index

.. topic:: Simplex Phase 1
    
    In phase 1, we want to find a feasible basic solution if it exists.

    To do this, we construct an *auxiliary linear program* :math:`L_{aux}`, which has the properties:

        - we can find a feasible basic solution for it after at most one pivot operation

        - **the original LP has a feasible solution iff the optimal objective value for** :math:`L_{aux}` **is zero**

    Consider the original LP problem:

    .. math::
        
        \text{Maximize}   &\quad \sum\limits_{j=1}^{n}c_jx_j\\
        \text{Subject to} &\quad \sum\limits_{j=1}^{n}a_{ij}x_j \leq b_j \quad(i \in [i, m])\\
                          &\quad   x_j \geq 0 \quad(j \in [1, n])\\

    The corresponding auxiliary linear problem is: 


    .. math::
        
        \text{Maximize}   &\quad -x_0\\
        \text{Subject to} &\quad \sum\limits_{j=1}^{n}a_{ij}x_j - x_0 \leq b_j \quad(i \in [i, m])\\
                          &\quad   x_j \geq 0 \quad(j \in [0, n])\\

    In slack form:

    .. math::

        z &= -x_0\\
        x_i &= b_i + x_0 - \sum\limits_{x_j\in N}  a_{ij}x_j

    If all :math:`b_i`'s are positive, basic solution already feasible. Otherwise:
        - find the equality :math:`x_i` with most negative :math`b_i`
        - make :math:`x_0` new basic variable, and :math`x_i` non-basic

    After this one pivot operation, all :math`b_i`'s are non-negative; thus the basic solution is feasible.


.. admonition:: TODO

    add example


---------------------------------------
Decision Procedure: Theory of Integers
---------------------------------------
Similarly as before, we only consider *quantifier-free* :math:`T_{\mathbb{Z}}` formulas without disjunctions. We want to solve the following problem:

Given an :math:`m \times n` matrix :math:`A` with only integer coefficients and a vector :math:`\vec{b}` in :math:`\mathbb{Z}^n`, does

.. math::
    
    A\vec{x} \leq \vec{b}

has **integer** solutions?

*Note:* Finding rational solution is poly-time, but integer problem is NP-complete (without disjunctions).

Omega Test
-------------------------------------------

.. admonition:: TODO

    Historical perspective: array dependence analysis

    - the problem of determine wether the same element is both read and written to at the same time during the execution of a program

    .. math::

        w_i = r_i \land w_j = r_j

The main idea of Omega test is to eleminate variables one by one from the initial system :math:`A\vec{x} \leq \vec{b}`. Geometrically it corresponds to computing a projection of a polytope in n-dimensional space to an n-1-dimensional space.

- **Omega Test:** Work Flow
    
    1. Real shadow (overapproximation)
        - if no solution: return **UNSAT**
        - otherwise continue

    2. Dard shadow (underapproximation)
        - if has solution: return **SAT**
        - otherwise continue

    3. Gray shadows
        - any subproblem has solution: **SAT**
        - otherwise UNSAT

.. topic:: Real Shadow (Fourier-Motzkin technique)

    We ignore requirement that solution must be integer.

    .. admonition:: TODO

        add formal definition

- **Example** for real shadow

    Consider the set of inequalities:

    .. math::

        x \leq y+10 \quad y \leq 15 \quad -x+20 \leq y

    - First rearrange the inequalities to isolate y on one side:

    .. math::

        &(1) \quad x-10\leq y\\
        &(2) \quad y \leq 15 \\
        &(3) \quad -x+20 \leq y

    - From (1) and (2), we have :math:`x-10 \leq 15 \equiv x\leq 25`

    - From (2) and (3), we have :math:`-x +20 \leq 15 \equiv x\geq 5`

    - The real shadow on x-axis is :math:`5 \leq x \leq 25`

.. topic:: Dark Shadow

    Dark shadow only projects those parts of polytope that are *at least one unit thick* in the x-dimension; thus we are guaranteed to have an interger solution for x if dark shadow has integer solution.

    Consider a pair of inequalities corresponding to lower and upper bounds on :math:`x`:

    .. math::

        L \leq ax \quad bx \leq U \\
        \equiv \frac{L}{a} \leq x \leq \frac{U}{b}

    Then to guarantee there is an integer value for :math:`x`, we have the constraint:

    .. math::

        aU - bL > ab-a-b

    .. admonition:: TODO

        add derivations of the formula


- **Example** for dark shadow

     Consider the set of inequalities:

    .. math::

        &(1) \quad 4y \geq x \\
        &(2) \quad 2y \geq 6-3x \\
        &(3) \quad 3y \leq 7-x

    - From (1) and (3), we have :math:`a=4, L=x, b=3, U=7-x`:

    .. math::

        4(7-x) -3x > 12 -4 -3 \implies x<\frac{23}{7}

    - From (2) and (3), we have :math:`a=2, L=6-3x, b=3, U=7-x`:

    .. math::

        2(7-x) -3(6-3x) > 6-3-2 \implies x<\frac{5}{7}

    - The real shadow on x-axis is :math:`\frac{5}{7}<x<\frac{23}{7}`


.. topic:: Gray Shadows

    If real shadow has integer solutions, but dark shadow does not, we still cannot conclude about the original problem. In this case, we construct the gray shadows, which look for integers *outside* the dark shadow, but *inside* the real shadow.

    By construction, points inside the real shadow satisfies:

    .. math::

        bL \leq abx \leq aU

    And points outside the dark shadow satisfies:

    .. math::

        aU-bL \leq ab-a-b

    Combining these two, points in the gray shadow must satisfy:

    .. math::

        L \leq ax \leq L + \frac{ab -a-b}{b}

    Observe that :math:`ax` must be integer. We then construct each gray shadow by adding the equality:

    .. math::

        ax = L +i

    for :math:`i` in :math:`[0, \frac{ab-a-b}{b}]`.

    - **If any subproblem has integer solution, then so does original problem**

    - **If no subproblem has integer solution, original problem is UNSAT**


.. admonition:: TODO

    add example

- Remarks

    - If there are :math:`n` integers between :math:`0` and :math:`\frac{ab-a-b}{b}`, Omega test constructs :math:`n` gray shadows

    - Thus it is very sensitive to coefficients: the larger :math:`a` is, the more gray shadows we must consider



-------------------------------------------
Decision Procedure: Combined Theories
-------------------------------------------
Given decision procedures for quantifier-free :math:`T_1` and :math:`T_2`, we want a decision procedure to decide satisfiability of formulas in qff :math:`T_1 \cup T_2`.

We use the Nelson-Oppen method. It has the following restrictions:

1. Only allows combining *quantifier-free* fragments

2. Only allows combining formulas *without disjunctions* (Note: can convert to DNF)

3. Signatures can only share equality: :math:`\Sigma_1 \cap \Sigma_2 = \{=\}`

4. :math:`T_1` and :math:`T_2` must be **stably infinite**

- **Definition:** Stably infinite
    Theory T is stably infinite iff every *satisfiable* qff formula is satisfiable in a universe of discourse with infinite cardinality

    - *Example:* non-stably infinite theory:
        .. math::

            \text{Signature:}\quad &\{a, b, =\}\\
            \text{Axiom:} \quad &\forall x. x=a \lor x=b

    - *Example:* stably infinite theories:
        :math:`T_=, T_\mathbb{Q}, T_\mathbb{Z}, T_A` 


Nelson-Oppen Method
-----------------------------------------------
Nelson-Oppen method has two phases:

1. Purification
    Seperate formula :math:`F` in :math:`T_1 \cap T_2` into two formulas :math:`F_1` in :math:`T_1` and :math:`F_2` in :math:`T_2`

2. Equality propagation
    Propagate all relevant equalities between theories (different for convex & non-convex)


.. topic:: Purification

    Given formula :math:`F` in :math:`T_1 \cap T_2`, goal is to seperate it into two formulas :math:`F_1` and :math:`F_2` such that:

        1. :math:`F_1` belongs only to :math:`T_1` ('pure')
        2. :math:`F_2` belongs only to :math:`T_2` ('pure')
        3. :math:`F_1 \land F_2` is **equisatisfiable** as :math:`F`

    To purify :math:`F`, exhaustively apply the following:

        1. Consider term :math:`f(..., t_i, ...). 
            If :math:`f \in \Sigma_i` but :math:`t_i` is not a term in :math:`T_i`, replace :math:`t_i` with freash variable :math:`z` and conjoin :math:`z=t_i`

        2. Consider predicate :math:`p(..., t_i, ...). 
            If :math:`p \in \Sigma_i` but :math:`t_i` is not a term in :math:`T_i`, replace :math:`t_i` with freash variable :math:`w` and conjoin :math:`w=t_i`

    After this procedure, we have :math:`F_1 \land F_2` as required.


.. admonition:: TODO

    add example




-----------------------------------------------
SMT Solvers and DPPL(:math:`\tau`) Framework
-----------------------------------------------

