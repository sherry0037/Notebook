==========================
reStructuredText
==========================


----------------------------------
Comment
----------------------------------

.. code-block:: RST

    .. Lines starting with two dots are special commands. But if no command can be found, the line is considered as a comment



----------------------------------
Table of Contents
----------------------------------

.. code-block:: RST

    .. contents::


----------------------------------
Text
----------------------------------

You can  put text in *italic* or in **bold**, you can "mark" text as code with double backquote ``print()``.

.. code-block:: RST

    *italic* 
    **bold**
    ``print()``

----------------------------------
List
----------------------------------

Lists are as simple as in Markdown:


.. code-block:: RST

    - First item
    - Second item
        - Sub item

    or

    * First item
    * Second item
        * Sub item

----------------------------------
Table
----------------------------------

Tables are really easy to write:

.. code-block:: RST

    =========== ========
    Country     Capital
    =========== ========
    France      Paris
    Japan       Tokyo
    =========== ========

=========== ========
Country     Capital
=========== ========
France      Paris
Japan       Tokyo
=========== ========

More complex tables can be done easily (merged columns and/or rows) but I suggest you to read the complete doc for this :)

.. code-block:: RST

    +------------+------------+-----------+ 
    | Header 1   | Header 2   | Header 3  | 
    +============+============+===========+ 
    | body row 1 | column 2   | column 3  | 
    +------------+------------+-----------+ 
    | body row 2 | Cells may span columns.| 
    +------------+------------+-----------+ 
    | body row 3 | Cells may  | - Cells   | 
    +------------+ span rows. | - contain | 
    | body row 4 |            | - blocks. | 
    +------------+------------+-----------+

+------------+------------+-----------+ 
| Header 1   | Header 2   | Header 3  | 
+============+============+===========+ 
| body row 1 | column 2   | column 3  | 
+------------+------------+-----------+ 
| body row 2 | Cells may span columns.| 
+------------+------------+-----------+ 
| body row 3 | Cells may  | - Cells   | 
+------------+ span rows. | - contain | 
| body row 4 |            | - blocks. | 
+------------+------------+-----------+

----------------------------------
Hyperlink
----------------------------------

There are multiple ways to make links:

- By adding an underscore after a word : Github_ and by adding the target URL after the text (this way has the advantage to not insert unnecessary URLs inside readable text).

  .. code-block:: RST

      Github_ 

      .. _Github: https://github.com/

- By typing a full comprehensible URL : https://github.com/ (will be automatically converted to a link)
- By making a more Markdown-like link: `Github <https://github.com/>`_ .

  .. code-block:: RST
      
      `Github <https://github.com/>`_ 

----------------------------------
Latex
----------------------------------

How to write Latex:

.. code-block:: RST

    .. math::

       \frac{ \sum_{t=0}^{N}f(t,k) }{N}

.. math::

   \frac{ \sum_{t=0}^{N}f(t,k) }{N}

Or a simpler way:

.. code-block:: RST

    :math:`\frac{\sum_{t=0}^{N}f(t,k) }{N}`

:math:`\frac{\sum_{t=0}^{N}f(t,k) }{N}`


----------------------------------
Body Elements
----------------------------------

.. topic:: Topic

    A topic is like a block quote with a title, or a self-contained section with no subsections. Use the "topic" directive to indicate a self-contained idea that is separate from the flow of the document. Topics may occur anywhere a section or transition may occur. Body elements and topics may not contain nested topics. 

    ``.. topic:: [Title]``


.. code-block:: RST

    .. sidebar:: Sidebar Title
       :subtitle: Optional Sidebar Subtitle

       Subsequent indented lines comprise
       the body of the sidebar, and are
       interpreted as body elements.


.. sidebar:: Sidebar Title
   :subtitle: Optional Sidebar Subtitle

   Subsequent indented lines comprise
   the body of the sidebar, and are
   interpreted as body elements.

----------------------------------
Documentation and Sources
----------------------------------

Other useful links:

`reStructuredText Directives <http://docutils.sourceforge.net/docs/ref/rst/directives.html#table>`_

`Quick reStructuredText <http://docutils.sourceforge.net/docs/user/rst/quickref.html>`_

`Restructured Text and Sphinx CheatSheet <http://openalea.gforge.inria.fr/doc/openalea/doc/_build/html/source/sphinx/rest_syntax.html#restructured-text-rest-and-sphinx-cheatsheet>`_


