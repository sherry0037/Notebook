============================
Web Crawling using PySpider
============================

I want to collect some data for an NLP project and this is an attempt to "crawl" the data from the web. Although I am probably not going to use it soon, there is a simple guide of how to setup PySpider with some useful links.

------------------------
Quickstart
------------------------
- Install: ``pip install pyspider``
- Run: ``pyspider``, visit http://localhost:5000/

If you don't have phantomjs
-----------------------------------
Follow the guide from StackOverflow_

.. _StackOverflow: https://stackoverflow.com/questions/36993962/installing-phantomjs-on-mac/53308472

1. Download phantomjs latest version (ex: phantomjs-2.1.1-macosx.zip) from http://phantomjs.org/download.html
2. Extract it to some path (ex: ~/Desktop/phantomjs-2.1.1-macosx)
3. Run this command on terminal: ``sudo ln -n ~/Desktop/phantomjs-2.1.1-macosx/bin/phantomjs /usr/local/bin/``
4. Launch phantomjs from the terminal by command: ``phantomjs``
5. Check phantomjs version by command: ``phantomjs -v``
6. Check the phantomjs path by command: ``which phantomjs``

If raised Invalid configuration error ...
-------------------------------------------

Error message:

.. code-block:: bash

     raise ValueError("Invalid configuration:\n  - " + "\n  - ".join(errors))
        ValueError: Invalid configuration:
        - Deprecated option 'domaincontroller': use 'http_authenticator.domain_controller' instead.

Solution:

1. Find "/usr/local/python3/lib/python3.6/site-packages/pyspider/webui/webdav.py"

2. Change ``'domaincontroller': NeedAuthController(app)`` to ``'http_authenticator':{ 'HTTPAuthenticator':NeedAuthController(app) },``

3. Execute ``pyspider all``

`reference 1 <https://github.com/binux/pyspider/issues/889>`_ 


If raise "Error: Could not create web server listening on port 25555"
-------------------------------------------------------------------------------

This is because the port has been used (maybe didn't terminate the process correctly last time)

Solution:

1. Run ``lsof -i:25555``

2. Something like this shows up:

.. code-block:: bash

    COMMAND     PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
    phantomjs 46971   xxx   12u  IPv4 0xe4d24cdcaf5e481f      0t0  TCP *:25555 (LISTEN)

3. Kill it! `kill  46971`

`reference 2 <https://www.crifan.com/pyspider_run_error_could_not_create_web_server_listening_on_por_25555/>`_


If raise "OSError: [Errno 48] Address already in use"
----------------------------------------------------------------------

Solution:

1. Run ``ps -fA | grep python``

2. Kill the process!

`reference 3 <https://stackoverflow.com/questions/19071512/socket-error-errno-48-address-already-in-use>`_


------------------------
Tutorials
------------------------

Here are some tutorials to get you started:

- `An official guide <http://docs.pyspider.org/en/latest/tutorial/HTML-and-CSS-Selector/>`_
- `A nice tutorial <https://stackoverflow.com/questions/19071512/socket-error-errno-48-address-already-in-use>`_
