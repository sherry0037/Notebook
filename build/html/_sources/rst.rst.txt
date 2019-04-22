=============
Restructed Text Syntax
=============

File syntax
A simple example of the file syntax:
.. Lines starting with two dots are special commands. But if no command can be found, the line is considered as a comment

.. =========================================================
Main titles are written using equals signs over and under
.. =========================================================

Note that there must be as many equals signs as title characters.

Title are underlined with equals signs too
.. ==========================================

Subtitles with dashes
.. ---------------------

You can  put text in *italic* or in **bold**, you can "mark" text as code with double backquote ``print()``.

Lists are as simple as in Markdown:

- First item
- Second item
    - Sub item

or

* First item
* Second item
    * Sub item

Tables are really easy to write:

=========== ========
Country     Capital
=========== ========
France      Paris
Japan       Tokyo
=========== ========

More complex tables can be done easily (merged columns and/or rows) but I suggest you to read the complete doc for this :)

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

There are multiple ways to make links:

- By adding an underscore after a word : Github_ and by adding the target URL after the text (this way has the advantage to not insert unnecessary URLs inside readable text).
- By typing a full comprehensible URL : https://github.com/ (will be automatically converted to a link)
- By making a more Markdown-like link: `Github <https://github.com/>`_ .

http://docutils.sourceforge.net/docs/user/rst/quickref.html

.. _Github https://github.com/