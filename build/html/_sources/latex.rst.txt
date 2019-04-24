==========================
Latex
==========================

----------------------------------
Most Frequently Used
----------------------------------

**Math equation (with alignment)**

.. code-block:: RST

    .. Math:
    \begin{equation*}
    \begin{split}

    \end{split}
    \end{equation*}


**Code listing**

.. code-block:: RST

    \begin{lstlisting}[language=R]
    \end{lstlisting}


**Figures**

Single figure

.. code-block:: RST

    \begin{figure}{}
      \centering
      \includegraphics[width=0.7\linewidth]{Q2_b.png}
      \caption{ }
    \caption{Histogram approximation for Q2b.}
    \label{fig:Q2_b}
    \end{figure}

Multiple figures

.. code-block:: RST

    \begin{figure}
    \begin{subfigure}{\textwidth}
      \centering
      \includegraphics[width=0.7\linewidth]{Q3_a.png}
      \caption{ }
      \label{fig:Q3_a}
    \end{subfigure} \newline
    \begin{subfigure}{\textwidth}
      \centering
      \includegraphics[width=0.7\linewidth]{Q3_b.png}
      \caption{ }
      \label{fig:Q3_b}
    \end{subfigure}
    \caption{Histogram approximation to the mixture densities in Q3.}
    \label{fig:Q3}
    \end{figure}

Add figure in-place

.. code-block:: RST

    \usepackage{float}

    \begin{figure}[H]
    ...
    \end{figure}


**Table**

.. code-block:: RST

    \begin{table}[]
    \begin{tabular}{lll}
    Summary            & Using formula & Using MC \\
    $E[\mu]$           &               &          \\
    $Var[\mu]$         &               &          \\
    $P[\mu \leq 10.0]$ &               &         
    \end{tabular}
    \end{table}


**Appendix**

.. code-block:: RST

    \begin{appendices}
    \chapter{Some Appendix}
    The contents...
    \end{appendices}
