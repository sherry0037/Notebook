Custom Sphinx themes for the documentation of `my projects <https://github.com/the-allanc/>`_.

Themes
======
- **yeen**: Slight modification of the `TriAx Corp <http://pythonhosted.org/sphinxjp.themes.trstyle/>`_ theme.

 - Defaults to blue colours and sidebar on the left.
 - Field lists now use the *mainlightcolor* option for the background colour for headers, rather than a hardcoded blue colour.
 - Optionally adds a Github "Octocat" banner to the repository (created by `Tim Holman <https://github.com/tholman/github-corners>`_).

Usage
=====
To use the theme, specify ``allanc_sphinx[yeen]`` as a dependency, and in your ``conf.py`` file::

    html_theme = 'yeen'

If you want to add the Octocat banner, you just need to define the link you want to go to - for example::

    html_theme_options = {'github_url': 'https://github.com/the-allanc/allanc_sphinx'}
