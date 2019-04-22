.. _Hoare.rst:

==================
Hoare Logic
==================
A summary of the `notes <https://d1b10bmlvqabco.cloudfront.net/attach/jr59i2jepb42au/hwwp1awd4r52zc/jud9gj29h04g/HoareLogicNotes.pdf>`_.

------------

- Contents
	- Program Specification
	- Hoare Logic
	- Program Verification
	- Soundness and Completeness
	- Total Correctness

------------


----------------------------------
Chapter 1: Program Specification
----------------------------------

- Syntax and Semantics

+------------+----------------------+-----------+ 
|            | Syntax               | Semantics | 
+============+======================+===========+ 
| Assignment | V:=E                 |           |
+------------+----------------------+-----------+ 
| Sequences  | C1;...;C_n           |.          |
+------------+----------------------+-----------+ 
| Conditions |IF S THEN C_1 ELSE C_2|           |
+------------+----------------------+-----------+ 
| WHILE      | WHILE S DO C.        |           |
+------------+----------------------+-----------+ 

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
Chapter 2: Hoare Logic
----------------------------------

- Axioms and rules
	==================== ====================================
	**Assignment axiom** :math:`\vdash \{P[E/V]\} V:= E\{P\}`
	==================== ====================================

	- :math:`P[E/V]`: substituting the term E for all occurrences of the variable V in the statement P
	- *Note* only works for this little programming language

