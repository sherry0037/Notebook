.. _Hoare.rst:

==================
Hoare Logic
==================
A summary of the lecture notes of `CS389L <http://www.cs.utexas.edu/~isil/cs389L/>`_.-Automated Logical Reasoning and `notes <https://d1b10bmlvqabco.cloudfront.net/attach/jr59i2jepb42au/hwwp1awd4r52zc/jud9gj29h04g/HoareLogicNotes.pdf>`_.

.. ------------

.. - Contents
	- Program Specification
	- Hoare Logic
	- Program Verification
	- Soundness and Completeness
	- Total Correctness

.. ------------


----------------------------------
Program Specification
----------------------------------

- Syntax and Semantics

+------------+--------------------------------------------+-----------+ 
|            | Syntax                                     | Semantics | 
+============+============================================+===========+ 
| Assignment | :math:`V:=E`                               |           |
+------------+--------------------------------------------+-----------+ 
| Sequences  | :math:`C1;...;C_n`                         |           |
+------------+--------------------------------------------+-----------+  
| Conditions | :math:`\text{IF S THEN $C_1$ ELSE $C_2$}`  |           |
+------------+--------------------------------------------+-----------+ 
| WHILE      | :math:`\text{WHILE S DO C}`                |           |
+------------+--------------------------------------------+-----------+ 

- Hoare's notation
	Hoare triple: :math:`\{P\}C\{Q\}`

	- C: program
	- P, Q: conditions written in mathematical notations and logical operators
	- True 
		1. if C is executed ina state satistying P 
		2. if the execution of C terminates
		3. then the state in which C's execution terminates satisfies Q

	- **Example**: :math:`\{X=1\} X:=X+1 \{X=2\}`

	- :math:`\{P\}C\{Q\}` is *partial correctness specification*
		- not ncessary for the executin of C to terminate
		- *if* C terminates, *then* Q holds
	- :math:`[P]C[Q]` is *Total crrectness specification*
		1. if C is executed ina state satisfying P, then C terminates
		2. After termination Q holds.

----------------------------------
Hoare Logic - Syntax and Semantics
----------------------------------


----------------------------------
Axioms and Rules
----------------------------------

- Assignment 

    - :math:`P[E/V]`: substituting the term E for all occurrences of the variable V in the statement P
    -  *Note* only works for this little programming language

.. math::

    \vdash \{P[E/V]\} V:= E\{P\}


- Pre-condition Strenghthening 

.. math::

    \frac{\vdash \{P'\} S \{Q\}  \qquad P \implies P' } {\vdash \{P\} S \{Q\} }


- Post-condition Weakening

.. math::

    \frac{\vdash \{P\} S \{Q'\}  \qquad Q' \implies Q } {\vdash \{P\} S \{Q\} }


- Composition/Sequential

.. math::

    \frac{\vdash \{P\} C_1 \{Q\}  \qquad \vdash \{P\} C_2 \{Q\}  } {\vdash \{P\} C_1; C_2 \{Q\} }

- If

.. math::

    \frac{\vdash \{P \land C\} S_1 \{Q\}  \qquad \vdash \{P \land \lnot C\} S_2 \{Q\} } {\vdash \{P\} \text{if C then $S_1$ else $S_2$ }\{Q\} }


- While:

.. math::

    \frac{\vdash \{P \land C\} S \{P\} } {\vdash \{P\} \text{while C do S} \{P \land \lnot C\} }



----------------------------------
Soundness and Completeness
----------------------------------


----------------------------------
Automated Reasoning
----------------------------------


----------------------------------
Abstract Interpretation
----------------------------------

