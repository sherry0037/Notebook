==========================
:math:`\LaTeX` Cheatsheet
==========================


--------------------------------
Math equation (with alignment)
--------------------------------

.. code-block:: RST

    \usepackage{amsmath}
    \begin{equation*}
    \begin{split}

    \end{split}
    \end{equation*}


--------------------------------
Code listing
--------------------------------


.. code-block:: RST
    
    \usepackage{listings}
    \begin{lstlisting}[language=R]
    \end{lstlisting}


--------------------------------
Figures
--------------------------------

**Single figure**

.. code-block:: RST

    \usepackage{graphicx}

    ...

    \begin{figure}{}
      \centering
      \includegraphics[width=0.7\linewidth]{Q2_b.png}
      \caption{ }
    \caption{Histogram approximation for Q2b.}
    \label{fig:Q2_b}
    \end{figure}


**Multiple figures**

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

**Add figure in-place**

.. code-block:: RST

    \usepackage{float}

    \begin{figure}[H]
    ...
    \end{figure}


--------------------------------
Table
--------------------------------

.. code-block:: RST

    %\usepackage{graphicx}
    \begin{table}[]
    \begin{tabular}{lll}
    Summary            & Using formula & Using MC \\
    $E[\mu]$           &               &          \\
    $Var[\mu]$         &               &          \\
    $P[\mu \leq 10.0]$ &               &         
    \end{tabular}
    \end{table}


--------------------------------
Appendix
--------------------------------

.. code-block:: RST
    
    %\usepackage[toc,page]{appendix}
    \begin{appendices}
    \chapter{Some Appendix}
    The contents...
    \end{appendices}

--------------------------------
Pseudo code
--------------------------------

.. code-block:: RST
  
    \usepackage{algorithm}
    \usepackage[noend]{algpseudocode}

    \begin{algorithm}
    \caption{My algorithm}\label{euclid}
    \begin{algorithmic}[1]
    \Procedure{MyProcedure}{}
    \State $\textit{stringlen} \gets \text{length of }\textit{string}$
    \State $i \gets \textit{patlen}$
    \BState \emph{top}:
    \If {$i > \textit{stringlen}$} \Return false
    \EndIf
    \State $j \gets \textit{patlen}$
    \BState \emph{loop}:
    \If {$\textit{string}(i) = \textit{path}(j)$}
    \State $j \gets j-1$.
    \State $i \gets i-1$.
    \State \textbf{goto} \emph{loop}.
    \State \textbf{close};
    \EndIf
    \State $i \gets i+\max(\textit{delta}_1(\textit{string}(i)),\textit{delta}_2(j))$.
    \State \textbf{goto} \emph{top}.
    \EndProcedure
    \end{algorithmic}
    \end{algorithm}
