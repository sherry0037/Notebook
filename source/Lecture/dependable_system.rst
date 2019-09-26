================================
Dependable System
================================


.. contents::
    :depth: 3

Understanding Fault-Tolerant Distributed Systems
================================================================
**Fault-tolerant systems** either exhibit a well-defined failure behavior when components fail, or mask component failures to users (continue to provide their specified standard service despite the occurrence of component failures).

Basic Architectural Concepts
-----------------------------
To achieve fault tolerance, a distributed system architecture incorperates redundant processing components. This section discusses the basic architectural building blocks of these systems, and classifies the failures that these basic blocks can experience.

Services, Servers, and the "Depends" Relation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- A **service** specifies a collection of operations whose execution can be triggered by inputs from service users or the pssage of time. 
    - Operation executions may result in outputs to users and in service state changes
- A **server** performs the operations defined by a service specification.
    - It implements a service without exposing to users the internal service state representation and operation implementation details.
    - Servers can be hardware or software implemented.
- A server ``u`` **depends** on a server ``r`` if the correctness of ``u``'s behavior depends on the correctness of ``r``'s behavior.
    - ``u`` is called a user (or client)
    - ``r`` is called a resource of ``u``
    - this relation is often represented as an acyclic graph in which nodes denote servers and arrows represent the "depends" relation.
    - ``u`` is said to be at a level of abstraction "higher" than ``r``.

.. topic:: Example

    A distributed system consists of software servers which depend on *processor* and *communication services*.

    Processor service is typically provided concurrently to serveral software servers by a multiuser operating system such as Unix or MVS. 
        - These operating systems in turn depend on the raw processor service provided by physical processors.
        - These physical processors depend on lower-level hardware resources such as CPUs, memories, I/O controllers, disks, displyas, keyboards, etc.

    The communication services are implemented by distributed communication servers which implement communication protocols such as TCP/IP and SNA, by depending on lower-level hardware networking services.

    The union of processor and communication service operations provided to application servers are designated as a distributed operating system service.


Failure Classification
~~~~~~~~~~~~~~~~~~~~~~~~
This artical assumes the service specification prescribes both:
    - the server's response for any initial server state, and
    - the real-time interval within which the response should occur.

A server **failure** occurs when the server does not behave in the manner specified.

    - **omission failure**: a server omits to respond to an input
    - **timing failure**: the server's response is functionally correct but untimely (outside the real-time interval specified)
        - can be either early or late
    - **response failure**: the server responds incorrectly
        - *value failure*: value is incorrect 
        - *state transition failure*: state transition is incorrect
    - **crash failure**: after a first omission to produce output, a server omits to produce output to subsequent inputs until its restart. Depending on the state at restart, we have:
        - *amnesia-crash*: server restarts in a predefined initial state that does not depend on the inputs seen before the crash
        - *partial-amnesia-crash*: at restart, some part of the state is the same as before the crash, while the rest of the state is reset to a predefined initial state
        - *pause-crash*: a server restarts in the state it had before the crash
        - *halting-crash*: a crashed server never restarts
    - **arbitrary failure**: includes all the failure classes above

Failure Semantics
~~~~~~~~~~~~~~~~~~~~
**Failure semantics** is the likely failure behaviors of a server. The recovery actions invoked upon detection of a server failure depend on the failure semantics, a fault-tolerant system has to *extend* the standard specification of servers to include, in addition to their failure-free semantics, the failure semantics.

If the specification of a server ``s`` prescribes that the failure behaviors likely to be observed by ``s`` users should be in class F, it is said that "s has F failure semantics".

In general, if the failure specification of a server ``s`` allows ``s`` to exhibit behaviors in the union of two failure classes F and G, we say that ``s`` has F/G failure semantics.
    - F/G is a weaker (less restrictive) failure semantics than F
    - F is a stronger (more restrictive) failure semantics than F/G
    - **arbitrary** failure semantics is the weakest possible failure semantics. The class of arbitrary failure behaviors includes all the failure classes defined previously

.. topic:: Example: Implementing Failure Semantics

    To ensure that a raw hardware processor service has crash failure semantics, one can use duplication and matching: 
    - use two physically independent processors that execute in parallel the same sequence of instructions,
    - compare their results after each instruction execution,
    - a crash occurs when a disagreement between processor outputs is detected.

In general, the stronger a specified failure semantics is, the more expensive and complex it is to build a server that implements it.


Probabilistic Clock Synchronization
================================================================

The goal for clock synchronization is:

for every t, t: global time, :math:`|P(t) - Q(t) \leq \epsilon|`.

We make the assumptions that the clocks are correct. A clock is considered to be correct if for every :math:`t > t'`

.. math::

    |C(t) - C(t') - (t-t')| \leq \rho (t - t')


