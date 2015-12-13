[![Documentation Status](https://readthedocs.org/projects/mobbioknowledge-base/badge/?version=latest)](http://mobbioknowledge-base.readthedocs.org/en/latest/?badge=latest)

# mobb.io/knowledge_base docs

To build them locally, install development requirements (in the venv directory):

    pip install -r requirements-dev.txt

To build the documentation for browsing, from this directory run: 

    ``make html`` 

then open ``_build/html/index.html`` in a browser.

To rebuild automatically while editing the documentation, from this directory run:

    ``sphinx-autobuild . _build``

The online editor at http://rst.ninjs.org/ is a helpful tool for checking reStructuredText syntax.

http://mobbioknowledge-base.readthedocs.org/en/latest/ 
