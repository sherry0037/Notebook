============================
Web Crawling using PySpider
============================

I want to collect some data for an NLP project and this is an attempt to "crawl" the data from the web. Although I am probably not going to use it soon, there is a simple guide of how to setup PySpider with some useful links.

------------------------
Quickstart
------------------------
- Install: `pip install pyspider`
- Run: `pyspider`, visit http://localhost:5000/

If you haven't install phantomjs
-----------------------------------
Follow the guide from StackOverflow_

.. _StackOverflow: https://stackoverflow.com/questions/36993962/installing-phantomjs-on-mac/53308472

1. Download phantomjs latest version (ex: phantomjs-2.1.1-macosx.zip) from http://phantomjs.org/download.html
2. Extract it to some path(ex: ~/Desktop/phantomjs-2.1.1-macosx)
3. Run this command on terminal: `sudo ln -n ~/Desktop/phantomjs-2.1.1-macosx/bin/phantomjs /usr/local/bin/`
4. Launch phantomjs from the terminal by command: `phantomjs`
5. Check phantomjs version by command: `phantomjs -v`
6. Check the phantomjs path by command: `which phantomjs`

https://github.com/binux/pyspider/issues/889

https://www.crifan.com/pyspider_run_error_could_not_create_web_server_listening_on_por_25555/

https://stackoverflow.com/questions/19071512/socket-error-errno-48-address-already-in-use